---
name: rinho-zones
description: Use when a Rinho GPS tracker customer needs many geofences (more than the 32 RG/WP ones) - customer sites, depots, branches, restricted areas, routes - loaded from a list, CSV, KML or KMZ into the zones module (up to 4096 shapes, 512 zones), the zones.bin file the device downloads, and rules that report or act when entering or leaving them (ZNA/ZNB signals, SZONES). Also for Spanish/Portuguese requests like "cargar zonas", "geocercas masivas", "zonas de clientes", "generar zones.bin", "aviso al entrar a un depósito", "cercas virtuais".
license: Apache-2.0
metadata:
  version: "1.5.1"
  author: Rinho Telematics
  homepage: https://config.rinho.com.ar/skills/index.html
---

# Rinho zones (mass geofences)

The zones module loads **up to 4096 shapes** (circle, polygon, route with width) grouped into
**512 logical zones** from one binary file (`zones.bin`) the device downloads over HTTP. You help
the customer to:
1. plan the zones and turn their places into a KML with zone ids,
2. compile it to `zones.bin` (`scripts/rinho.mjs zones build`) and check it,
3. publish it on an HTTP URL and load it on the device,
4. write the rules that report or act on entering/leaving.

Reply in the user's language. Commands are never translated. You write to a device **only** when
the user explicitly asks you to load the file from their PC with the device on USB and names the
port (Step 5). The only tool is `scripts/rinho.mjs` (Node.js 18+, no packages), next to this file;
its output is JSON. For the serial port the same program ships as the `rinho-serial` package.

Read `references/zones.md` once per task: numbering, limits, KML format, commands.

## Step 1 — Context

Ask, only what you don't know:
- **Model and firmware** (`>QVR<` reply). Zones need firmware **1.09.21 or later** (public docs).
  If older, tell them to update (config.rinho.com.ar → Firmware → "Por cable", or Rinho support).
  The `rinho-config` validator flags zone commands and ZNA/ZNB rules when `--fw` is older.
- **What they have**: a list of places (name + coordinates), a CSV/Excel, a KML/KMZ from Google
  Earth, or areas to draw.
- **What each zone should do**: report entering/leaving, alert, only at night, cut, etc.
- **Where they can publish a file** on plain HTTP (see Step 4).

## Step 2 — Plan the zones

- Group places by **what the rules should do with them**, not one zone per place: all branches of
  a client can be one zone; "depots" another. Up to 512 zones.
- Zones that rules use **must be ids 0-99 (ZNA00-ZNA99) or 256-355 (ZNB00-ZNB99)**. Other ids still
  work as geofences but only through `EVAL ZNA(n)`, never in `TRG=`/`CND=`.
- Write the plan as a short table: zone id, rule token, what it contains, what it does.

## Step 3 — Build the KML and compile it

| They have | Command |
|---|---|
| CSV / Excel of points (`name, lat, lon` + optional `radius_m`, `zones`; Spanish/Portuguese headers, `;` and decimal commas OK) | `node scripts/rinho.mjs zones from-csv places.csv -o zones.kml --zone 0 [--radius 150] [--title "..."]` |
| KML/KMZ without zones, one zone per Google Earth folder | `node scripts/rinho.mjs zones assign input.kmz -o zones.kml --by-folder` |
| KML/KMZ without zones, all in one zone | `node scripts/rinho.mjs zones assign input.kml -o zones.kml --zone 0 --radius 150` |
| KML that already has zones you want to replace | add `--overwrite`; `--start N` sets the first id for `--by-folder` |
| See what a KML/KMZ contains | `node scripts/rinho.mjs zones list input.kml` |

Then compile and check:

```bash
node scripts/rinho.mjs zones build zones.kml -o zones.bin
node scripts/rinho.mjs zones inspect zones.bin      # magic, CRC, shapes and zones in the file
```

- In the CSV, a `zones` column (ids separated by `;` or `,`, quoted if needed) wins over `--zone`,
  which is only the default for rows without one. Rows must have the same number of fields as the
  header: a decimal comma in a `,`-separated file is rejected with a hint.
- **KML coordinates are `lon,lat`**. Swapped coordinates put the place in another continent:
  sanity-check a couple of points before publishing.
- Default radius 100 m (circle) and side margin 50 m (route). Suggest from the use: a depot gate
  50-100 m, a client site 150-300 m, a whole town is better as a polygon.
- `inspect` must report `crcValid: true` and list exactly the zones of the plan. For each zone:
  `ruleToken` is what goes in `TRG=`/`CND=`/`>QSS..<` (`null` = that id can't be used in rules) and
  `eval` is the `EVAL` expression that always works. `build` refuses a file with shapes that have no
  zones.

## Step 4 — Publish the file

The device downloads with **plain HTTP GET**: the URL must start with `http://`, be reachable from
the internet (from the device's SIM or WiFi) and **not redirect to https**. Any web server, a
storage bucket with public HTTP access, or the customer's own platform works. Check it with
`curl -I http://<host>/zones.bin`: it must answer `200` directly, not `301/302`.

## Step 5 — Load it on the device

Send in this order, **one command at a time** from the configurator's monitor/terminal, from
their platform, or, if the user asks you to and names the USB port, with
`npx -y rinho-serial run <port> ">QZMODE<" --confirmed` (one command per call; show it to the user
first). `zones load` below does the whole sequence. These are device actions, not configuration: the Code-mode importer and the
validator don't model them (`NO_HANDLER`), so don't put them in the rules block.

1. `>QZMODE<` → `RZMODE,<enabled>,<fraction>,<log sectors>,<zone sectors>`. `enabled` = `1` means
   zones are already on: skip to 4 unless the file won't fit (step 4 answers `busy_or_nomem`).
2. `>SZFRAC<n><`: memory reserved for zones. Suggested by file size (a conservative rule of thumb,
   not from the docs): < 50 KB → `16`, 50-200 KB → `8`, 200-500 KB → `4`, ≥ 500 KB → `2`. Smaller
   number = more space, less internal log. It takes effect the next time zones are enabled.
3. `>SZMODE1<`: enables zones. **The device reboots** if zones were off, and it **refuses while the
   internal log has records not sent yet** (`>QDL<` second number must be 0). `>SZMODE1F<` forces
   it and **those records are lost**: only if the user accepts that. To change the fraction with
   zones already on: `SZFRAC<n>`, `SZMODE0`, `SZMODE1` (two reboots).
4. `>SZONES"http://<host>/zones.bin"<` → `RZONES,START`, then `RZONES,OK,...,elem=<n>`: `elem`
   must equal the number of shapes. `RZONES,ERR,busy_or_nomem` = not enough memory: use a smaller
   fraction (steps 2-3).
5. Later, `>QZONES<` → `RZONES,LOADED,...` confirms the file is still there.

To update the zones: rebuild, replace the file at the same URL, send `SZONES"..."` again.

**If the user asks you to load it from their PC** (device on USB, they name the port), the
`rinho-serial` package does steps 1-5 in that order and stops where a decision is theirs:

```
npx -y rinho-serial zones load COM4 --url http://host/zones.bin
```

It opens only that port, checks the firmware (1.09.21+), reads `QZMODE` and, with zones already
on, sends `SZONES` and waits for `RZONES,OK` (up to 3 min), then `QZONES`. With zones **off** it
stops with `ZONES_DISABLED`: explain that enabling them reboots the device and needs the internal
log delivered (`>QDL<` second number 0), and with the user's ok run again with `--enable`
(`--fraction 2|4|8|16` to reserve memory first). `LOG_NOT_EMPTY` means records would be lost:
only with the user's explicit acceptance add `--force-lose-log`. After the reboot it reconnects to
the same port by itself. Report `elements` against the number of shapes in your `zones.bin`
(`rinho.mjs zones inspect`). Changing the fraction with zones already on is two reboots: do that
with the user from the configurator.

## Step 6 — Rules

Write them with the `rinho-config` skill (recipe `evento-zona-masiva`) and validate them:

```
>SRL60E;TRG=ZNA00+;CND=GPS;ACC={GER20H;@LOG}<     // enter zone 0 → event 20
>SRL61E;TRG=ZNA00-;CND=GPS;ACC={GER21H;@LOG}<     // leave zone 0 → event 21
```

`GERnnH;@LOG` is the event report the Rinho recipes use (two-digit event id, high priority,
through the datalog channel). If the customer's platform already gets events from the device, use
the same report type and destination their existing rules use. Agree the event ids with them.

- `+` enter, `-` leave, `*` both.
- **Always `CND=GPS` on leaving, and on any rule that acts on outputs**: when the GPS fix is lost,
  every zone goes to "outside" and fires its leave edge.
- Detection takes up to 1 s. There is no automatic report: nothing is sent unless a rule says so.
- Check a zone now: `>QSSZNA42<` → `RSSZNA4211` inside, `...00` outside.

## Version check

This skill is version 1.5.1. Once per conversation, if you can fetch URLs, GET
https://config.rinho.com.ar/skills/version.json and compare `skills["rinho-zones"].version`.
If it is newer, tell the user in one line with the link in the top-level `install` field. If the fetch fails,
continue silently. Never download or replace skill files yourself.
