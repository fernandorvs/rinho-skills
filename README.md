# Rinho skills for AI agents

Official skills for **Rinho GPS trackers** (Zero IoT, Spider IoT, Smart IoT), so the AI you already
use (Claude, Claude Code, Codex, Cursor and any agent that reads the open
[Agent Skills](https://agentskills.io) format) can configure, troubleshoot and load geofences
on your devices using Rinho's public documentation and the **same validator as the official
configurator** at [config.rinho.com.ar](https://config.rinho.com.ar).

| Skill | Version | What it does |
|---|---|---|
| `rinho-config` | 1.6.0 | Turns a request ("buzzer above 110 km/h for 10 s") into a validated block of commands to paste into the configurator's Code mode. 31 tested recipes, every public command with its syntax and minimum firmware. |
| `rinho-diagnostics` | 1.5.0 | Troubleshoots a device (not reporting, no GPS, inputs, outputs, battery, BLE) with read-only query commands and playbooks by symptom. |
| `rinho-zones` | 1.5.0 | Builds the mass-geofence file (`zones.bin`, up to 4096 shapes) from a CSV, KML or KMZ, explains how to load it, and writes the enter/leave rules. |

The skills produce text you review and send from the configurator. They write to a device only
when you explicitly ask them to, with the device on your PC's USB and the port you name, through
the [`rinho-serial`](https://www.npmjs.com/package/rinho-serial) package (`npx -y rinho-serial`):
it re-validates for the real model and firmware, backs up what it touches, sends one line at a
time and stops at the first error. Replies are in your language.

## Install

**Any agent that supports Agent Skills** (Claude Code, Codex, Cursor, …):

```bash
npx skills add fernandorvs/rinho-skills                  # pick the skills to install
npx skills add fernandorvs/rinho-skills -a claude-code   # for Claude Code (otherwise they land in .agents/skills/, which it doesn't read)
npx skills add fernandorvs/rinho-skills --list           # see what's here
```

**Claude Code** (as a plugin, with updates):

```
/plugin marketplace add fernandorvs/rinho-skills
/plugin install rinho@rinho
```

**Claude.ai / Claude Desktop**: download the `.zip` of each skill from
[config.rinho.com.ar/skills](https://config.rinho.com.ar/skills/index.html) and add it under
Settings → Capabilities → Skills.

**Manual**: copy `skills/<name>/` into your agent's skills folder (`.claude/skills/`,
`.agents/skills/`, …).

The skills ship one tool, `scripts/rinho.mjs` (Node.js 18+, no dependencies; build `fe00d56c1440`):
`validate`, `explain`, `zones from-csv|assign|list|build|inspect`. The same file plus the serial
driver is the npm package `rinho-serial` (`ports`, `query`, `send`, `run`, `zones load`). Agents that
can't run scripts use the same validator over HTTP: `POST https://config.rinho.com.ar/api/validate`.

## This repository is a mirror

It is generated from the Rinho configurator on every release and **pull requests here are not
merged**: report problems or ideas at https://www.rinho.com.ar/contacto/ or in an issue, and the
fix ships from the source. The skills check `https://config.rinho.com.ar/skills/version.json`
once per conversation and tell you when a newer version exists; they never update themselves.

## Links

- Documentation: https://docs.rinho.com.ar (for agents: [llms.txt](https://docs.rinho.com.ar/llms.txt), [compat.json](https://docs.rinho.com.ar/compat.json))
- Configurator: https://config.rinho.com.ar
- Rinho Telematics: https://www.rinho.com.ar
