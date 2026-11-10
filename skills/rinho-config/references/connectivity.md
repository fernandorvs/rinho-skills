# Pointing a device to a platform

Source: public docs "Comando IP", "Comando CXIP", "Comando CXAPN"
(https://docs.rinho.com.ar/comandos/comunicaciones/datos-moviles/). Fetch them when in doubt.

| Command | Meaning |
|---|---|
| `>SIP0"tracker.example.com",12100<` | Main server by hostname + port (quotes required) |
| `>SIP0190.168.1.100/5000<` | Main server by IP + port (`ip/port`, no quotes) |
| `>SIP1...<` | Main redundant server. `SIP3` WiFi-only main; `SIP4`/`SIP5` backup channel |
| `>QIP0<` | Reply `RIP0"host",port` or `RIP0ip/port`; `RIP0ERR` = slot not set |
| `>SCXIP<main><back><window><` | Protocol per channel: `P` UDP, `T` TCP, `W` Wialon IPS; window 1-9. `>SCXIPPP5<` = UDP/UDP, window 5. Reply `RCXIPPP50` (the trailing `0` is a fourth field of the reply, not of the command) |
| `>SCXAPN host,user,password<` | APN of SIM0 (`SCXAP1` for SIM1). Reply `RCXAPN host,user,password` (password in clear: mask it) |
| `>SCXAA1<` | Auto-APN (acts only when no APN is set) |

**Wialon (TAIP or IPS)**: the full recipe is the application note "Rinho en Wialon"
(https://docs.rinho.com.ar/notas-de-aplicacion/rinho-en-wialon, also under `/en/` and `/pt/`): device types and ports, what
Wialon decodes from each frame, the `;AP=` block, event numbers, commands and troubleshooting. End
every Wialon setup by telling the user to **import the WLP of their model** in Wialon (one file
serves TAIP and Rinho IPS): https://docs.rinho.com.ar/descargas/wialon/rinho-zero-iot.wlp,
rinho-spider-iot.wlp, rinho-smart-iot.wlp. They bring ignition (`in8`), voltage (`pwr_int`),
backup battery (`adc2`, IPS only), inputs, outputs, signal (`cel_level`), event (`rep_num`) and, on
Spider and Smart, 8 CAN sensors (`rpm`, `fuel_lvl`, `refr_temp`, `oil_press`, `odometer`,
`trip_time`, `odoliter`, `acc`), plus two query commands (`QVR`, `QCQ`).

Typical "platform changed server" fix: one line, `>SIP0"new.host",port<` (and `SIP1` if they give a
second host). Same protocol → nothing else; the reports queued on the device drain to the new
server by themselves. Protocol change (`SCXIP`) → the Rinho recipes also send `>SCXDLE<`, which
cancels the pending datalog uploads built for the old protocol. **`SCXDLE` is an action, not
configuration: it is sent on its own from the configurator's monitor or the platform, never
inside the Code-mode block** (the validator marks it `ACTION_NOT_CONFIG`). Do not replace it with
`SCXDL0,...`: that disables the datalog upload permanently.

Secrets: `SCXAPN` and `SWL` carry passwords. Write them with a placeholder (`SCXAPN host,user,****`)
and tell the user to type the real password in the configurator; never ask them to paste it in the
chat. (`SWLNA` is not a secret: it is the literal login for a unit without password, see below.)

## Wialon IPS

Checked on Wialon Hosting with a Zero and a Spider on 1.09.28.T63 (November 2026).

- **Protocol**: `W` in `SCXIP` for the channel (`>SCXIPWP5<` = main Wialon IPS, backup UDP, window 5).
- **Host and port**: the ones of the **unit type** the customer created in Wialon. A unit created
  as **"Rinho IPS"** on Wialon Hosting listens on port **22500** (`nl.gpsgsm.org` in that test);
  **20332 is the generic "Wialon IPS" type and rejects a Rinho IPS unit** with `#AL#0`. Ask the
  user to read host and port from the unit's device-type card, don't assume.
- **Login (`SWL`)**: if the unit has **no password**, always send `>SWLNA<`, whatever the IPS
  platform. On Wialon Hosting an empty `SWL` makes the firmware send `#L#IMEI;`, Wialon answers
  `#AL#0` and the unit receives nothing, **with no warning anywhere in Wialon**; `NA` is accepted.
  Wrong password → `#AL#01`. Traccar-based platforms (GPSWOX is believed to be one) ignore the
  password field altogether: Traccar's Wialon decoder reads only the IMEI and answers `#AL#1` when
  the IMEI exists, so `NA` is harmless there too, and **if the IMEI is not registered it does not
  answer at all** (the device retries as if it had no connection). With a password, write
  `>SWL****<` and let the user type it. The validator accepts `SWL` lines; the serial tool masks the
  value in its output and log.
- **Changing protocol on an installed device**: the reports already queued were built for the old
  protocol and **block delivery** until removed. `>SCXDLE<` alone was not enough: it took `>CBU<`
  (send queue) and `>SDLC<` (datalog). Both are actions that **delete pending reports**: send them
  with `run`, only after the user accepts losing what is queued (check `QBU`/`QDL` first).
- **Template (`SUC00`)**: use the parameter names Wialon already gives the TAIP fields, so one
  Wialon WLP works for both protocols. Must start with `QWD0| ` (the `WD` form in the firmware
  manual is rejected). Measured limits: a 229-character template was refused without any reply, 212
  worked (the validator warns above 212).
  - Zero: `>SUC00 QWD0| $rep_num:1:| #| $,cel_level:1:'CSQ',pwr_int:2:'V/100'<`
  - Spider / Smart: `>SUC00 QWD0| $rep_num:1:| #| $,cel_level:1:'CSQ',pwr_int:2:'V/100',rpm:1:'ECU(0;0)',fuel_lvl:2:'ECU(2;0)',odometer:2:'ECU(3;0)',odoliter:2:'ECU(4;0)',refr_temp:1:'ECU(6;0)',oil_press:1:'ECU(7;0)',trip_time:2:'ECU(10;0)'<`
  - In IPS `'CHN'` is not resolved (the literal text arrives), `'OT'` is in **metres**, `ECU(...)`
    fields arrive as 0 without a CAN bus, and an event id with letters (`1E`) is not a valid
    integer: Wialon drops `rep_num`. Use digit-only event ids with IPS.
- Wialon's `custom_msg` commands reach the device over TAIP and over IPS; Wialon does not keep the
  device's reply.
- Troubleshooting "Wialon shows nothing": `QWL` first (empty = silent login failure), then
  `QIP0`/`QCXIP` against the unit type's host, port and protocol, then `QBU`/`QDL` (see the
  `rinho-diagnostics` skill).
- Finish by importing the WLP of the model (links above): it is the same file for TAIP and IPS.
