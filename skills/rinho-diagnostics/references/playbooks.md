# Playbooks by symptom

Start every case with `>QVR<`. Ask 3-5 queries per round and branch on the replies.
Response formats are in `commands.md`.

## Not reporting / platform shows it offline

1. `>QVR<` `>QCEL<` `>QBU<` `>QDL<`
   - `RCEL,,,0,0,,,99,...` (empty status) → the modem never came up since boot. Ask for
     `>QSSCELPWR<` first: `RSSCELPWR00` = cellular switched off by command, **and it stays off
     after reboots** (saved in NVS). Fix: the user sends `>SSSCELPWR11<` (a write command: never
     send it yourself; confirm with them). Seen in the field: a device reporting fine by WiFi with
     the modem off for months. If `QSSCELPWR` is `11`, then no SIM or a modem failure. Either way,
     WiFi reporting doesn't make the cellular side optional: it is the backup when WiFi is out of reach.
   - `RCEL,SEARCH` → don't blame the antenna yet: look at `creg`, `cereg` and `roaming` (fields
     8-10). The status word comes from the modem's cell report and can still say SEARCH while
     `creg` already reports registered.
     - `creg` and `cereg` both 0 or 2 → no cell: antenna, coverage, SIM seated; check the cellular LED.
     - `creg` 5 + `roaming` 1 + `cereg` 0 (e.g. `RCEL,SEARCH,,0,0,,,99,5,0,1,…`) → the device
       registered only in roaming and never attached to LTE: suspect the SIM (operator, roaming
       agreement, data plan, suspended line) and ask the user which operator it is before touching
       configuration or hardware.
     - `cereg` 3 → registration denied by the operator (SIM or plan).
   - `RCEL,LIMSRV` → camped but not registered: SIM rejected, no data plan, operator not allowed.
     Check `>QCOPS<` for a forced 2G/4G or a manual operator.
   - `RCEL,NOCONN` or `CONNECT` → registered; go on.
   - Registered but no data at all → ask for `>QCXAPN<` **telling the user first to replace the third
     field (the password) with `****` before pasting**. Wrong or empty APN → no data. Fix via
     `rinho-config` (APN command, password left for the user to type in the configurator).
2. `>QIP0<` `>QIP1<` `>QCXIP<` (`>QMC<` only if `QCEL` didn't show the MCC)
   - **Most common cause of "stopped reporting" with good signal: the platform moved servers** and
     the device still points to the old host. Ask the platform for host, port and protocol and
     compare with `RIP0`. A bare `RIP1` (nothing after the digit) = that slot is empty (no redundant server).
   - Server IP/port not the platform's → reports go elsewhere. IP0 = main server, IP1 = main
     redundant, IP3 = WiFi-only main (if set, WiFi uses only IP3), IP4/IP5 = backup channel.
   - Protocol letter in `RCXIP` (P = UDP, T = TCP, W = Wialon IPS) must match the platform.
3. `RBUOK01312` = 1312 reports queued. A big `QBU`/`QDL` backlog with good signal → the device produces reports but can't deliver
   them. On a bench with ignition off the queue may not grow, so don't wait for growth: judge by the
   backlog and by whether it drains after the fix. Queue growing between two `QBU` queries a minute
   apart (vehicle in use) → same conclusion: connectivity, server not listening, or (TCP/Wialon) server not acknowledging.
   Queue at 0 and not growing → delivering. If the platform still shows nothing, the problem is on
   the platform side (wrong device ID, wrong port or protocol on the server).
4. Wialon IPS, nothing arrives (checked on Wialon Hosting, Nov 2026): in this order.
   - `>QWL<` first. **Empty** (`RWL` with nothing) on Wialon Hosting = the login fails silently:
     the device sends `#L#IMEI;`, Wialon answers `#AL#0` and shows no error. A unit **without
     password** needs `>SWLNA<` (literal `NA`, valid on every IPS platform); with a password, `SWL`
     + the password (the user types it; if they paste the reply, masked). `#AL#01` = wrong password.
     On Traccar-based platforms (GPSWOX, presumably) the password is ignored and **no answer to the
     login means the IMEI is not registered** on the platform: check the unit there, not the device.
   - `>QIP0<` / `>QCXIP<`: host, port and `W` must match the **unit type** in Wialon. A "Rinho
     IPS" unit on Wialon Hosting listens on 22500; **20332 is the generic "Wialon IPS" type and
     rejects a Rinho IPS unit**.
   - Protocol changed recently with reports queued (`QBU` > 0 and not draining): the queued
     reports were built for the old protocol and block delivery. Fix is `>CBU<` + `>SDLC<`
     (actions that delete the pending reports: only with the user's ok; `SCXDLE` alone was not
     enough).
   - `>QWD<` shows the exact frame the device would send, to compare with what Wialon expects.
     `>QUC00<` shows the template; it must start with `QWD0| ` and stay under ~212 characters.
     The device speaks Wialon IPS 1.0.

## A parameter or sensor is missing in Wialon (position arrives, a field doesn't)

`>QWD<` `>QUC00<` `>QCXIP<`
- What Wialon decodes from each frame family (TAIP CQ/BQ/ER/EQ/HQ/HV/CV and IPS), with the
  parameter names, is the table in section 4 of the application note "Rinho en Wialon":
  https://docs.rinho.com.ar/notas-de-aplicacion/rinho-en-wialon. Compare the frame from `QWD` with it.
- The unit needs the WLP of its model imported (https://docs.rinho.com.ar/descargas/wialon/,
  files rinho-zero-iot.wlp, rinho-spider-iot.wlp, rinho-smart-iot.wlp; one file for TAIP and IPS):
  it maps `in8` ignition, `pwr_int`, `adc2` (IPS only), `cel_level`, `rep_num` and the CAN sensors.
- CAN fields (`rpm`, `fuel_lvl`, …) arrive as 0 without a CAN bus (Spider/Smart) and never on Zero.
- In IPS the template (`QUC00`) decides which parameters travel; `'CHN'` is not resolved and `'OT'`
  is in metres. Event ids with letters make Wialon drop `rep_num`.

## Reports arrive late or in bursts

`>QBU<` `>QDL<` `>QCEL<` `>QCXDL<`
- Big `QBU` / pending `QDL` count that drains when coverage returns → intermittent coverage (look
  at `csq`, `rsrp`, `rsrq` in `QCEL`). Expected behavior: the device stores and forwards.
- `QCXDL` shows log auto-upload settings (destination, retries, interval, FIFO/LIFO). LIFO sends
  newest first, so old positions arrive late.

## Too many or too few periodic reports

`>QTD00<` (and the other timers the rules use) `>QRL**<`
- `RTDaabcccccddddd`: `ccccc` seconds and `ddddd` metres of the timer. Find the rule whose
  `TRG=TDnn+` generates the report and check its `CND` (e.g. only with `IGN`).

## No GPS fix / wrong or frozen position

`>QSSGPS<` `>QCXGPS<`
- `RSSGPS00` → no valid fix: sky view, antenna, wait 2-3 min outdoors. GPS LED blinking every
  700 ms = searching, 300 ms = GPS module error.
- `RCXGPS1` → position only updates with ignition on; `RCXGPS2` → only above 5 km/h. A frozen
  position while parked is then expected.

## Ignition or inputs not detected

`>QIO<` `>QSSIGN<` `>QCXIGN<` `>QIT00<`
- `RIO;IGN0;IN...`: compare `IGN` and each input bit with the real state (input bit 1 = open,
  0 = to ground). If the bit doesn't change when the wire does → wiring.
- Ignition has its own debounce (`QCXIGN`, default 10 s). Short pulses are filtered on purpose.
  `QITnn` is the debounce of digital inputs, and slot 07 is not ignition.
- Bit changes but rules don't react → check the rules (`QRL**`) and their slots.

## Output / engine cut / buzzer not working

`>QIO<` `>QCXXP00<` (one query per output: `QCXXP01`, `QCXXP02`) `>QGC<` `>QRL**<`
- `XP` bits in `QIO` show the output state now.
- `QCXXPnn` all `9` for an output → that output is disabled and ignores commands (Spider ships XP01
  and XP02 disabled). `>QCXXP<` without the index gets no answer.
- `QGC` status `active` → a gradual engine cut is running (it survives reboots on recent firmware).
- Rule that should drive it: check its `TRG`/`CND` against the current signals (`QSS<signal>`).

## Reboots or hangs

`>QVR<` `>QUT<` `>QIO<`
- `QUT` internal temperature. Very high → overheating.
- `QIO` `V` (tenths of a volt) and `VBU` (backup battery, hundredths): unstable or low supply
  causes resets. Backup LiPo below 3.5 V needs charging.
- Old firmware → recommend updating with Rinho support.

## Battery drain (vehicle battery or backup)

`>QIO<` `>QSSVBU00<` `>QRL**<`
- Look for the low-power rules (ignition off → KO / sleep). Without them the device stays fully
  on. Both LEDs flashing orange every 5 s = low-power mode active.
- Use the `rinho-config` recipe "bajo-consumo-ign" to add it.

## BLE sensor not arriving

`>QVR<` `>QBTH<` `>QBS00.rssi<` `>QBS00.batt<`
- `QVR` without NIMBLE/BLE on Spider/Smart → firmware without BLE.
- `QBS00` → `INVALID`: sensor off, out of range, or wrong MAC in the slot.
- Low or jumping RSSI → distance or metal in between.
- On Zero, WiFi and BLE are mutually exclusive.

## WiFi not connecting

`>QWIF<`
- `pwr` 0 → WiFi off. `signallevel` 0-1 → weak.
- The device sees 2.4 GHz networks only.
- `>QWIFN<` scans visible networks, but it blocks the channel 1-4 s. Use it once, only if needed.
