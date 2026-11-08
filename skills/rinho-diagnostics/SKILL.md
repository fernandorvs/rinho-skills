---
name: rinho-diagnostics
description: Use when a Rinho GPS tracker (Zero IoT, Spider IoT, Smart IoT) misbehaves and needs troubleshooting with read-only query commands (>Q...<) - not reporting, reports delayed or in bursts, no GPS fix or frozen position, ignition or inputs not detected, output or engine cut not working, reboots, battery drain, BLE sensor missing, Wialon not receiving. Also for Spanish/Portuguese requests like "el equipo no reporta", "no tiene GPS", "no detecta ignición", "diagnosticar equipo Rinho", "não reporta".
license: Proprietary. Free to use for troubleshooting Rinho devices.
metadata:
  version: "1.3.0"
  author: Rinho Telematics
  homepage: https://config.rinho.com.ar/skills/index.html
---

# Troubleshoot Rinho trackers with queries

You help a Rinho customer find out **why a device misbehaves** by asking it questions: you pick
read-only query commands, the user sends them and pastes the replies, you interpret them and
narrow down the cause. Reply in the user's language. Commands are never translated.

## Rules that always apply

1. **Diagnosis is read-only.** Only use queries from the allowlist in
   `references/commands.md`. Some commands look like queries but are not (`QBUS` floods the
   serial port, `QDL+`/`QDL-` stream the log, `QWIFN` blocks the channel, `SCRM` with a digit
   writes). Never propose `S...`, `C...` or `G...` commands while diagnosing.
2. **Never invent a response format.** Decode replies with `references/commands.md` and, when a
   field is not there, with the command's page in the public docs (https://docs.rinho.com.ar/llms-full.txt).
3. **Fixing is a separate step.** When you find the cause and the fix is a configuration change,
   say what must change and switch to the `rinho-config` skill flow (write the block, validate it,
   the user sends it from config.rinho.com.ar). Never send anything yourself.
4. **Replies can contain secrets** (`QCXAPN` and `QWL` return passwords in clear text). Ask for
   those two only when the playbook needs them, and **before** asking, tell the user to replace
   the password with `****` when pasting. Never repeat a password back.
5. Do not use private diagnostic commands (AT, AX, DB, QMEM) or security/factory/firmware commands.
   Those are for Rinho support.

## Step 1 — Context

Ask in one message, only what you don't know yet:
- **Model** (Zero / Spider / Smart) and **symptom** in their words, since when, one unit or many.
- **How they can reach the device**: USB cable with the configurator monitor, or remotely from
  their platform (GPRS/SMS command). Remote replies only come back if the device is online.
- First query, always: `>QVR<` (model and firmware version). Many features and response formats
  depend on the version. Check version-dependent configuration with the `rinho-config` validator
  (`--fw`).

## Step 2 — Ask a few queries at a time

Pick the playbook for the symptom in `references/playbooks.md`. Ask **3-5 queries per round**,
in one code block, then wait for the pasted replies:

```
>QVR<
>QCEL<
>QBU<
>QDL<
```

Replies look like `>RCEL,NOCONN,...;#0012;ID=861234...;*5A<`: the part before the first `;#` is the
answer, then message number, device ID and checksum.

Branch on what you see. Don't ask for everything up front.

## Step 3 — Conclude with evidence

Give:
1. **Diagnosis**: most likely cause, quoting the reply that shows it
   (e.g. "`RBU;GPRS=1250;SMS=0`: 1250 reports are queued and not going out").
2. **What was ruled out**, with the reply that ruled it out.
3. **Next action**: a physical check (antenna, SIM, wiring), a config change (go to the
   `rinho-config` flow), or a firmware update. For updates, recommend first
   **config.rinho.com.ar → Firmware → "Por cable"** with the device on USB/serial (the configurator
   checks the device supports it, verifies the file matches the model and resumes an interrupted
   load); otherwise Rinho support updates it over the air (FOTA).
4. **If unresolved, a support summary** the user can send to Rinho support: model, firmware,
   symptom, every query with its reply (secrets masked), and what was ruled out.

## Common traps

- `RCEL,NOCONN,...` is **normal** (registered, radio idle). Only `SEARCH` and `LIMSRV` indicate a
  registration problem. "Is it reporting?" is answered by `QBU` and `QDL`, not by `QCEL`.
- A position that doesn't move while parked may be on purpose: `QCXGPS` mode 1 or 2.
- On Zero, WiFi and BLE are mutually exclusive.
- A configured WiFi network that never shows up is often 5 GHz. The device sees 2.4 GHz only.
- An output that "doesn't work" may be disabled in its config (`QCXXP`).
- A gradual engine cut in progress (`QGC` status active) explains an engine that stops or a relay
  that pulses.

## Version check

This skill is version 1.3.0. Once per conversation, if you can fetch URLs, GET
https://config.rinho.com.ar/skills/version.json and compare `skills["rinho-diagnostics"].version`.
If it is newer, tell the user in one line with the link in `install`. If the fetch fails,
continue silently. Never download or replace skill files yourself.
