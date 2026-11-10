# Rule format (SRL)

Source: public docs, "Comando RL" (https://docs.rinho.com.ar/comandos/motor-de-eventos/comando-rl)
and "Lista de disparadores" (https://docs.rinho.com.ar/apendices/lista-de-disparadores).
When in doubt, fetch those pages: they win over this summary.

## Syntax

```
>SRLnnE;TRG=<signals><edge>;CND=<signals>;ACC={<command>}{<command>}...<
```

| Part | Meaning | Required |
|---|---|---|
| `nn` | Rule slot: `00`-`99` (decimal) and `A0`-`FF` (hex). **196 rules.** `0A`-`9F` do not exist. | yes |
| `E` / `D` | Enabled / disabled | yes |
| `TRG=` | Trigger: signals in postfix order, ending with the edge | yes |
| edge | `+` false→true, `-` true→false, `*` any change | yes |
| `CND=` | Extra condition (level, no edge) that must hold when the trigger fires | no |
| `ACC=` | Actions, each command between `{}`, without `>` `<` | no |

Query: `>QRLnn<` (one rule), `>QRL**<` (all).

**`>SRLnnD<` and `>SRL**D<` do not just disable: they ERASE the rule** (TRG, CND and ACC are lost).

## Signals

Indexed signals take a two-digit index: `TD01`, `IN02`, `U09`, `VL00`, `UV05`. Unindexed: `IGN`,
`PWR`, `GPS`, `AC`, `JMD`, `TT`, `BU`, `CEL`... Ranges (from the docs):

| Signal | Range | What it is |
|---|---|---|
| `IN` | 00-09 | Digital inputs (actual count depends on model) |
| `XP` | 00-02 | Outputs (as level) |
| `TD` | 00-31 | Timers (fire when they expire) |
| `TR` | 00-07 | Schedules (time of day) |
| `NT` | 00-31 | Counters (overflow/underflow) |
| `U` | 00-63 | User flags (set with `SSSU<nn><mask><value>`, see docs "Comando SS") |
| `UV` | 00-47 | User triggers derived from a query (see docs "Comando UV") |
| `VL` | 00-15 | Speed thresholds |
| `DV` | 00-03 | Speed change (harsh acceleration/braking) |
| `DC` | 00-02 | Heading change |
| `AR` | 00-11 | Analog ranges (voltage windows) |
| `WP` | 00-31 | Circular geofences (waypoints) |
| `RG` | 00-31 | Polygon geofences (regions) |
| `PK` | — | Parking |
| `BS` | 00-03 | Bluetooth sensors |
| `RL` | 00-195 | State of another rule |

Full list with descriptions: "Lista de disparadores" in the docs.

## Operators (postfix)

Signals first, then the operator: `IGN VL01 &` is written `IGNVL01&`.

| Operator | Meaning |
|---|---|
| `&` | AND |
| `\|` | OR |
| `^` | XOR |
| `&!` `\|!` `^!` | NAND, NOR, XNOR |
| `!` after a signal | NOT that signal: `IN02!`, `IGN!` |

Evaluation is left to right: `IN00!XP00&U09^` = `((NOT IN00) AND XP00) XOR U09`.
The edge at the end of `TRG` applies to the **result**: `TRG=IGNVL01!&+` fires when
"ignition AND NOT moving" becomes true.

Every operator combines two operands. `IN00!IN01!IN02!&` is invalid (three signals, one operator):
write `IN00!IN01!&IN02!&`.

## Actions (ACC)

Each action is a command without `>` `<`, between braces:

| Action | Example | Meaning |
|---|---|---|
| Generate report | `{GCQ00H}` | Report type + 2 digits + priority `H`/`M`/`L` |
| Report to a destination | `{GER09H;@LOG}` | `@LOG` = datalog, other destinations in the docs |
| Text message | `{GTX00L;@SM0;TXT=MOVIMIENTO}` | Text to a destination (`@SM0` = SMS slot 0, `@TRM` = serial) |
| Timer | `{STD04R}` / `{STD04C}` | `R` restart now (not saved), `C` stop |
| Flag | `{SSSU0611}` | Set flag U06 to 1 (`...00` clears it) |
| Output | `{SSSXP0011}` | Output XP00 on (`...00` off) |

### Reports and destinations

- `GCQnnH`: standard position report; `GERnnH`: event report with a two-digit event id `nn` (what
  platforms show as "event 32" for `GER20`, see below). Priority `H`/`M`/`L`. Pick the event ids
  with the customer; the recipes don't reserve any.
- No `;@...` suffix: the report goes to the main server queue. `;@LOG`: through the datalog, which
  the device uploads by the channel set in `CXDL` (the Rinho recipes use `GERnnH;@LOG` for events).
  `;@BACK`: backup server (IP4/IP5). `;@TRM`: serial port. `;@SM0`: SMS to number slot 0.
- If the customer already receives events from the device, use the same report type and
  destination their existing rules use (`>QRL**<` shows them).

**Event numbers as the platform shows them.** In TAIP the two characters after the report type
are the event id **in hexadecimal**, and the device sends them unchanged in every frame family
(`GCQ31` arrives as `RCQ31…`, `GER41` as `RER41…`). What the customer sees is up to the
platform's decoder:
- **Most platforms convert hex to decimal**, `00`-`FF` → 0-255 (SmartTrack included): `GCQ31`
  shows as event 49, `GCQ10` as 16. This is the convention the configurator follows (its own event
  list skips `0A`-`0F`).
- Some platforms show the two characters as written (ASCII): `31` shows as 31.
- Aker: `00`-`99` as written, `A0`-`FF` converted from hex to decimal (`A0` = 160), the same
  scheme as rule slots.
- Wialon, measured on 2026-11-10 (Spider and Zero on 1.09.28.T63, same UC, same id): converts in
  `CQ` and `EQ` frames (`31` → 49, `43` → 67, `12` → 18) and shows as written in `BQ`, `ER`,
  `HQ`, `HV` and `CV`.
Ask which platform receives the reports and tell the user the number they will see for the id and
family you chose. `00`-`09` reads the same everywhere.

**User reports (UC) and the `;AP=` block in Wialon** (same measurement):
- Wialon decodes `;AP=name:type:value` in every family (CQ, BQ, ER with its CAN block, EQ, HQ, HV,
  CV). A template that works for all of them: `$R<fam>| #| Q<fam>,5,<len>| $;AP=...`.
- A name equal to a fixed field (e.g. `pwr_int`) wins over the fixed field.
- Wialon drops silently and keeps the rest of the block: empty values, values that don't match the
  type, type 4, names longer than 32 characters. A repeated name keeps the last value.
- `;PA=` is not decoded.

Timers that fire a rule keep reloading: a one-shot timer needs `{STDnnC}` in the rule that handles
its expiry. Timer definition: `STDnn<E|D><sssss><mmmmm>` = 5 digits of seconds + 5 digits of
metres (`STD04D0030000000` = 300 s, 0 m, defined but disabled). From a rule: `{STDnnR00120}` arms
120 s now (not saved), `{STDnnC}` stops it.

### Digital inputs

`INnn` reads **1 when the input is open (floating) and 0 when it is tied to ground**. A door
sensor that closes to ground when the door is shut gives `IN00 = 0` closed, `1` open; tell the
user to check it in the configurator's monitor (`>QIO<`) before trusting the polarity.

## Examples (from the docs)

```
>SRL07E;TRG=TD01+;CND=U09U01!&;ACC={GCP01H}<     // TD01 expires and U09 AND NOT U01 → report CP01, high priority
>SRL00E;TRG=TD00+;CND=IGN;ACC={GCO02L}<          // periodic report only with ignition on
>SRL23E;TRG=AR02-;ACC={GTX50L;@TRM;TXT=BAJA TENSION}<
>SRL00E;TRG=VL00+;CND=WP05;ACC={GTX00L;@TRM;TXT=Exceso en zona escolar}<
```
