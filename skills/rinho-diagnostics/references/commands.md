# Read-only query commands

Summary of the public docs. Each command links to its page: **when a reply does not match the
format below, the docs page (and the firmware version from `QVR`) wins.**

Replies come framed: `>R<answer>;ID=<device id>;*<checksum><`. A `;#<msgnum>` field appears only when
the command was sent with a message number (platforms do; the configurator's monitor usually doesn't).

Formats below were checked against the firmware (verified on a Zero IoT 1.09.28.T63 on 2026-11-08).
If a reply doesn't match, trust the device and tell the user what it answered.

## Allowlist (safe to send while diagnosing)

| Query | Reply | Fields / meaning |
|---|---|---|
| `>QVR<` | `RVR RINHO IOT v1.09.28.T63 ZE EG915U LC86G 8MB WIFI NIMBLE 2026-10-14 19:45:02` | Model code (`ZE`/`SP`/`SM`), firmware version (decides which commands and formats exist), modem, GPS, flash, radios (`WIFI`, `NIMBLE` = BLE), build date. [doc](https://docs.rinho.com.ar/comandos/configuracion-general/configuraciones-y-consulta-de-parametros/comando-vr) |
| `>QVRE<` | `RVR <same as QVR> U128\|IN10\|XP3\|TD32\|WP32\|VL16\|DC5\|CID10\|SM5\|DS5\|IP6\|RL196\|ICL100\|UV48\|UC16\|XB8` | Same `RVR` line plus the device's capacities (flags, inputs, outputs, timers, geofences, rules…). Useful to know how many slots a model has. [doc](https://docs.rinho.com.ar/comandos/configuracion-general/configuraciones-y-consulta-de-parametros/comando-vre) |
| `>QCEL<` / `>QCELnn<` | `RCEL,status,radio,mcc,mnc,rat,band,csq,creg,cereg,roaming,sim,reglvl,lac,cellid,rsrp,rsrq,sinr`. **Empty status** (`RCEL,,,0,0,,,99,...`) = the modem never reported a cell since boot. First check `>QSSCELPWR<`: `00` = cellular switched off by command (persists across reboots). Otherwise no SIM or a modem failure. Even if the device still reports by WiFi, the cellular side needs checking. Example `RCEL,CONNECT,eMTC,722,310,eMTC,B28,24,1,1,0,0,7,7425,12345678,-95,-11,12` = connected, LTE-M, Claro AR (722/310), band 28, csq 24, creg 1, cereg 1, not roaming, SIM0, level 7, LAC 7425, cell 12345678, RSRP -95 dBm, RSRQ -11 dB, SINR 12 | `status`: SEARCH (no cell), LIMSRV (not registered), **NOCONN (registered, idle = normal)**, CONNECT (transmitting now). `csq` 0-31 (99 unknown; 0-9 bad, 10-14 fair, 15-19 good, 20+ excellent). LTE `rsrp` good ≥ -90 dBm, poor < -105; `rsrq` good ≥ -10 dB, poor < -15. `nn` = one field 00-16. Full 17 fields from fw 1.09.24; older returns `status,tech,mcc,mnc`. [doc](https://docs.rinho.com.ar/comandos/comunicaciones/status-de-conexion/comando-cel) |
| `>QCOPS<` / `>QCOP0<` `>QCOP1<` | `RCOPS<value>` / `RCOP0<value>` / `RCOP1<value>` | Empty or `0` = automatic. `2G`, `3G`, `4G` = forced. `M,<plmn>` = manual operator. [doc](https://docs.rinho.com.ar/comandos/comunicaciones/datos-moviles/comando-cops) |
| `>QCXAPN<` / `>QCXAP1<` | `RCXAPN host,user,password` (`RCXAP1...` for SIM1) | APN per SIM; empty user/password is normal. **Contains the password: mask it.** [doc](https://docs.rinho.com.ar/comandos/comunicaciones/datos-moviles/comando-cxapn) |
| `>QCXAA<` | `RCXAA0` / `RCXAA1` | Auto-APN (only acts when no APN is set). |
| `>QIP0<` … `>QIP6<` | `RIP0"host",port` or `RIP0ip/port`; a bare `RIP3` (nothing after the digit) = slot empty | 0 main, 1 main redundant, 3 WiFi-only main, 4-5 backup channel, 6 backup WiFi. [doc](https://docs.rinho.com.ar/comandos/comunicaciones/datos-moviles/comando-ip) |
| `>QCXIP<` | `RCXIP<main><back><window>0` | Protocol per channel: P = UDP, T = TCP, W = Wialon IPS. Window 1-9. [doc](https://docs.rinho.com.ar/comandos/comunicaciones/datos-moviles/comando-cxip) |
| `>QMC<` | `RMC<mcc>` | Country code of the current network (722 = Argentina). [doc](https://docs.rinho.com.ar/comandos/comunicaciones/datos-moviles/comando-mc) |
| `>QWIF<` | `RWIF,pwr,apmode,stationindex,ssid,rssi,signallevel` | `pwr` 0/1, `apmode` 0 STA / 1 AP, `signallevel` 0-4. [doc](https://docs.rinho.com.ar/comandos/comunicaciones/status-de-conexion/comando-wif) |
| `>QWL<` | `RWL <password>` / `RWL *` | Wialon password. **Mask it.** [doc](https://docs.rinho.com.ar/comandos/comunicaciones/datos-moviles/comando-wl) |
| `>QWD<` | raw `#D#...` frame | The Wialon frame the device builds now (fw 1.09.19+). |
| `>QCFW<` | `RCFW<phase>,<pct>` | Modem firmware update status: 0 idle … 4 OK, 5 error. [doc](https://docs.rinho.com.ar/comandos/comunicaciones/datos-moviles/comando-cfw) |
| `>QBU<` | `RBUOK<nnnnn>` | Reports waiting to be sent (all priorities), 5 digits: `RBUOK00000` = nothing pending. **Main "is it delivering?" check.** [doc](https://docs.rinho.com.ar/comandos/configuracion-y-consulta-de-estados/datalog-y-buffer-de-reportes/comando-bu) |
| `>QBUE<` | `RBUOK<nnnnn> E<free> L<low> M<mid> H<high>` | Same, split by priority, plus free slots in the queue. |
| `>QDL<` | `RDLOK<total> <pending>[ BUSY]` | Datalog records: total, pending upload. `QBU` is the live report queue and `QDL` the stored log: either one growing means the device is not delivering. [doc](https://docs.rinho.com.ar/comandos/configuracion-y-consulta-de-estados/datalog-y-buffer-de-reportes/comando-dl) |
| `>QCXDL<` | `RCXDL<ena>,<dst>,<retries>,<time>,<order>` | Log auto-upload: destination, retries (0 = infinite), interval in tenths of s, 0 FIFO / 1 LIFO. [doc](https://docs.rinho.com.ar/comandos/configuracion-y-consulta-de-estados/datalog-y-buffer-de-reportes/comando-cxdl) |
| `>QUT<` | `RUT±bbbb` | Internal temperature, °C. [doc](https://docs.rinho.com.ar/comandos/configuracion-y-consulta-de-estados/cpu/comando-ut) |
| `>QIO<` | `RIO;IGN<0/1>;IN<7 bits>;XP<3 bits>;V<dV>;VBU<cV>` | Ignition; inputs always 7 bits `IN06…IN00`, **rightmost = IN00** (1 = open, 0 = to ground; a Zero uses only the last 2, Spider 3, Smart 7); outputs `XP02 XP01 XP00` (1 = on); supply in tenths of V; backup in hundredths of V. E.g. `V127` = 12.7 V, `VBU420` = 4.20 V. [doc](https://docs.rinho.com.ar/comandos/configuracion-y-consulta-de-estados/entradas-y-salidas/comando-io) |
| `>QAINnn<` | `RAINnn±vvvv` | Analog input 00-05, conditioned value. [doc](https://docs.rinho.com.ar/comandos/configuracion-y-consulta-de-estados/entradas-y-salidas/comando-ain) |
| `>QCXAINnn<` | `RCXAINnn±BOTTOM±TOP±XA±XB±YA±YB` | Scaling of that analog input (XA/XB in hundredths of V). |
| `>QARnn<` | `RARaabbcddddeffffggg` | Analog threshold window: input, min, max, debounce. [doc](https://docs.rinho.com.ar/comandos/configuracion-y-consulta-de-estados/entradas-y-salidas/comando-ar) |
| `>QCXXP<` | `RCXXPaabbbbccccdddd` | Output config: duration, ON, OFF in tenths of s. All `9` = output disabled. [doc](https://docs.rinho.com.ar/comandos/configuracion-y-consulta-de-estados/entradas-y-salidas/comando-cxxp) |
| `>QITnn<` | `RITnn<on><off>` | Digital input debounce, tenths of s. Slot 07 is not ignition. |
| `>QCXIGN<` | `RCXIGNa,ton,toff` | Ignition source mode and debounce (on / off). [doc](https://docs.rinho.com.ar/comandos/configuracion-general/configuraciones-de-contexto/comando-cxign) |
| `>QSSCELPWR<` | `RSSCELPWR11` on / `RSSCELPWR00` off | Cellular modem power. **Saved in NVS**: an `SSSCELPWR00` sent once keeps the modem off across reboots. The fix (`>SSSCELPWR11<`) is a write command the user sends. |
| `>QSSGPSPWR<` | `RSSGPSPWR11` / `00` | GPS module power, also saved in NVS. |
| `>QSS<signal>[nn]<` | `RSS<signal>[nn]11` / `00` | Any signal as on/off: `QSSGPS` (valid fix), `QSSCEL`, `QSSBU` (pending data), `QSSIGN`, `QSSU04`, `QSSWP05`, `QSSVL00`, `QSSVBU00`… [doc](https://docs.rinho.com.ar/comandos/configuracion-y-consulta-de-estados/senales/comando-ss) |
| `>QGC<` | `RGC<E/D>,xp,step,interval,max,invert;status:...` | Gradual engine cut: `status` disabled / ready / active, elapsed, remaining. [doc](https://docs.rinho.com.ar/comandos/configuracion-y-consulta-de-estados/senales/comando-gc) |
| `>QTDnn<` | `RTDaabcccccddddd` | Timer config: `b` E/D (saved) R/C (RAM), seconds, metres. Not the elapsed count. [doc](https://docs.rinho.com.ar/comandos/configuracion-y-consulta-de-estados/timers-y-contadores/comando-td) |
| `>QNTnn<` | `RNTaabbbbbccccc` | Counter: max, current. |
| `>QRLnn<` / `>QRL**<` | `RRLnnE;TRG=...;CND=...;ACC={...}` | The rules. Explain them with the `rinho-config` skill. [doc](https://docs.rinho.com.ar/comandos/motor-de-eventos/comando-rl) |
| `>QCXGPS<` | `RCXGPS<0/1/2>` | 0 position always updated; 1 only with ignition; 2 only above 5 km/h. [doc](https://docs.rinho.com.ar/comandos/configuracion-consulta-procesos-y-calculos-referentes-al-gps/comando-cxgps) |
| `>QXBnn<` / `>QCXXBnn<` | `RXBnn<text>` / capture config | Last text captured from a serial/modem/GPS/BLE source. [doc](https://docs.rinho.com.ar/comandos/configuracion-general/buffers-de-captura/comando-cxxb) |
| `>QBTH<` | `RBTH,pwr,up,conn,nconn,sesion,name,mac,rssi,role,link` | Bluetooth link state (fw 1.09.26+). |
| `>QBSnn.<attr><` | raw value | BLE sensor attribute, e.g. `QBS00.rssi`, `QBS00.batt`, `QBS01.fuel_lvl`. `INVALID` = no data. [doc](https://docs.rinho.com.ar/comandos/configuracion-y-consulta-de-estados/bluetooth-low-energy-ble/comando-bs-iot) |

## Do NOT send while diagnosing

| Command | Why |
|---|---|
| `QBUS`, `CBU` | `QBUS` dumps every queued report over serial; `CBU` deletes the queue |
| `QDL+`, `QDL-` (with filters), `QDLC` | Stream or cancel the datalog download |
| `QWIFN` | Active WiFi scan; blocks the channel 1-4 s (use once, only if needed) |
| `SCRM` + digit, `SGPRSH`, `SCFW` | Change state / reconnect / start a modem firmware update |
| `SSS...` | Sets flags, switches outputs, powers modules on/off |
| `SGC...` | Controls the engine cut |
| `SRLnnD`, `SRL**D` | **Erase** rules |
| `G...` (e.g. `GGP00M`) | Generates a report (not harmful, but not a query) |
| `CBU...`, `SDLC`, `SDLF` | Delete queues / log |
| AT, AX, DB, QMEM, PW, SFACTORY | Private / security. Rinho support only |

## LEDs (all models)

| LED | Pattern | Meaning |
|---|---|---|
| GPS | green blink 700 ms | searching for GPS |
| GPS | green blink 300 ms | GPS module error |
| GPS | off | position OK |
| Cellular | red blink | initialized, waiting to connect |
| Cellular | orange blink | connecting data |
| Cellular | green blink | connected |
| either | orange every 5 s | low-power (KO) mode active |
