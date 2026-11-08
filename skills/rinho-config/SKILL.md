---
name: rinho-config
description: Use when configuring Rinho GPS trackers (Zero IoT, Spider IoT, Smart IoT) - writing or fixing rules (SRL), timers, speed limits, outputs, alarms, reports or any >S...< command block, explaining an existing Rinho configuration, or fixing an import error in the Code mode of config.rinho.com.ar. Also for Spanish/Portuguese requests like "configurar equipo Rinho", "armar una regla", "corte de motor", "exceso de velocidad", "ralentí", "botón de pánico", "configurar rastreador", "bloqueio".
license: Proprietary. Free to use for configuring Rinho devices.
metadata:
  version: "1.5.0"
  author: Rinho Telematics
  homepage: https://config.rinho.com.ar/skills/index.html
---

# Configure Rinho trackers

You help a Rinho customer (installer, fleet operator or platform integrator) turn a request in
plain language into a **validated block of Rinho commands** that they paste into the **Code mode
of https://config.rinho.com.ar** and send to the device from there.

Reply in the user's language. Commands themselves are never translated.

## Rules that always apply

1. **Never invent commands, fields or signals.** Use only what is in the public docs, in
   `references/` or in a recipe. If you can't find it, say so and point to the docs.
2. **Never send anything to a device yourself.** You produce text. The user loads it in the
   configurator, reviews it and sends it.
3. **Never hand over a block that has not passed the validator** (see Validate). If you could not
   run it, say so explicitly and tell the user the import in Code mode will be the check.
4. **Outputs are safety-critical.** If the block drives an output (engine cut / immobilizer,
   gradual cut, siren, buzzer, door lock) state clearly what activates it and under which
   conditions, prefer recipes that only cut at low speed, and recommend testing with the vehicle
   stopped.
5. Do not use security, factory-reset or firmware-update commands (PW, SFACTORY, OTA/FOTA).
   Those are handled by Rinho support: https://www.rinho.com.ar/contacto/. You may explain what
   such a command does; never hand it over.

## Step 1 — Ask before writing

If not already known, ask (one short message):
- **Model**: Zero IoT, Spider IoT or Smart IoT. Zero has no CAN bus and no 1-Wire (iButton);
  digital inputs/outputs: Zero 2/1, Spider 3/3, Smart 7/3.
- **Firmware version** (`>QVR<` reply, e.g. `v1.09.28.T50`). Features are version-dependent: pass
  it to the validator with `--fw` so it flags what that device can't run.
- **Configurator mode** they use: Basic or Advanced. In **Basic**, rules RL00-RL31 are reserved:
  put custom rules at **RL50 or higher**.
- **What is already configured.** Ideally the current config exported from the configurator, so you
  don't overwrite slots in use. If they don't have it, use slots the configurator's own modules don't
  take (rules RL50+, timers TD10+, flags U20+, speed limits VL02+, UV05+), say which you used, and
  tell them to read the device before importing.

If the user is in a hurry and already gave model and firmware, write the block with those slots and
state the assumptions; don't block on the rest.

## Step 2 — Find the building blocks

- **Recipes first**: ~30 tested blocks (idling, panic button, engine cut with pre-warning, speeding
  with buzzer, door open too long, fuel theft, low-power mode, hour meter, geofence flags...). Read
  the latest at https://config.rinho.com.ar/skills/recipes.md; if you can't fetch it, use
  `references/recipes.md` (same content, as of this skill's release). If one fits, adapt it instead
  of writing from scratch.
- **Many geofences** (customer sites, depots, more than 32): use the `rinho-zones` skill.
- **Rule syntax**: `references/rule-format.md` (TRG/CND/ACC, edges, operators, slot ranges).
- **Commands**: `references/commands.md` has every public command with syntax, example and minimum
  firmware per model (generated from the docs). For details fetch the command's page (the URL is
  there; index: https://docs.rinho.com.ar/llms.txt, full text: https://docs.rinho.com.ar/llms-full.txt)
  and follow its exact digit widths. **If a command or field is in neither, tell the user you
  can't verify it and point to Rinho support; never derive syntax from the validator's output or
  from guesses.**
- **Server, APN, protocol** (pointing the device to a platform): `references/connectivity.md`.
- **Pitfalls**: `references/common-errors.md`. Read it once per task.
- **Combining recipes**: check what they share. Two recipes driving the same output or flag can
  interfere (e.g. a speeding alarm that turns on the buzzer plus an engine-cut recipe whose timer
  starts when that buzzer turns on). Give each feature its own flag/timer, or gate with `CND=`.

## Step 3 — Write the block

- One command per line, framed `>...<`. Explain each line with a trailing `// comment` in the
  user's language.
- **Inside comments, never write a framed command** (`>SSSXP0111<`): the Code-mode importer picks
  it up as a real command. Write `SSSXP0111` without `>` `<`.
- Keep every resource (rule, timer, flag, UV...) on a slot that is free on the customer's device.

## Step 4 — Validate (mandatory)

If you can run code (Node.js 18+):

```bash
node scripts/rinho.mjs validate config.txt --model zero --fw 1.09.28.T50   # model: zero, spider, smart (comma-separated for several)
cat config.txt | node scripts/rinho.mjs validate - --model spider
node scripts/rinho.mjs explain ">SVL00E00921<"       # fields, values and units of one line
```

`--fw` takes `1.09.28.T50`, `v1.09.27` or the whole `>QVR<` reply; pass the `.Tnn` build when the
device shows one (features of a release can depend on it). Add `--offline` (or set
`RINHO_SKILL_OFFLINE=1`) when there is no internet: it only skips the version check, which is a
GET of version.json without any user data. `explain` reads one **configuration** line; an action
such as `STD10R00120` is valid inside `ACC={...}` but is not a configuration line on its own.

`scripts/rinho.mjs` is next to this file: one tool for validating, explaining and (in the `rinho-zones` skill) building zone files. The output is JSON. Exit code `0` means no rejected lines.

If you cannot run code but can make HTTP requests:

```
POST https://config.rinho.com.ar/api/validate
Content-Type: application/json
{"text": "<the whole block, comments included>", "models": ["zero"], "fw": "1.09.28.T50"}
```

Same JSON as the CLI. `{"explain": ">SVL00E00921<"}` explains one line.

Read the result:
- `rejected[]`: lines the configurator does not accept, with `line`, `code` and `reason` (Spanish
  text from the configurator; translate it). **Fix every one** and validate again. Two codes are
  not about syntax: `ACTION_NOT_CONFIG` (an action such as `SCXDLE`, `SRLnnD`, `SZONES`: it is
  sent on its own from the monitor, not in the block) and `SECURITY_COMMAND` (never produced).
- `warnings[]`: `FRAMED_COMMAND_IN_COMMENT`, `DUPLICATE_RESOURCE` (two lines set the same slot;
  only the last survives), `NOT_A_COMMAND_OR_COMMENT`. Fix them unless the user wants it that way.
- `FIRMWARE_TOO_OLD` (only with `--fw`): the device firmware can't run those lines; the configurator
  will skip them or send a reduced version. Tell the user which feature, the minimum version, and
  how to update: **config.rinho.com.ar → Firmware → "Por cable"** (USB/serial; the configurator checks
  whether the device supports it) or Rinho support (over the air). Then validate again.
- The validator checks against an **empty** config: it cannot see slots already used on the device.
- `notice`: a newer validator exists. Tell the user in one line, with the link.

If neither is possible: say the block is **not validated** and that the Code-mode import will show
any error. Ask them to paste the error back to you.

## Step 5 — Hand over

1. The block, in one code fence, comments included.
2. What it does, in two or three plain sentences, and what resources it uses (e.g. "RL50-RL54, TD10, U20").
3. How to load it: config.rinho.com.ar → **Code** mode → paste → **Import** → review → **Send**.
   Read the device first if they want to keep the rest of its configuration.
4. How to test it (e.g. "turn ignition on with the vehicle stopped and wait 5 minutes").
5. Safety notes if it touches outputs.

## Explaining an existing config

If the user pastes a config and asks what it does: run `explain` on each line (or validate the
block), then explain rule by rule in plain language: "RL05: when ignition turns off, start timer
TD03 (10 min); when it expires, send report CQ00 and go to low power".

## Version check

This skill is version 1.5.0. Once per conversation, if you can fetch URLs, GET
https://config.rinho.com.ar/skills/version.json and compare `skills["rinho-config"].version`.
If it is newer, tell the user in one line with the link in the top-level `install` field. If the fetch fails,
continue silently. Never download or replace skill files yourself.
