# Read-only query commands

Summary of the public docs. Each command links to its page: **when a reply does not match the
format below, the docs page (and the firmware version from `QVR`) wins.**

Replies come framed: `>R<answer>;#<msgnum>;ID=<device id>;*<checksum><`.

## Allowlist (safe to send while diagnosing)

| Query | Reply | Fields / meaning |
|---|---|---|
| `>QVR<` | `RVR <maker> <model> v<ver> u<mem> <modem> <gps> <build date>` | Firmware version: decides which commands and formats exist. [doc](https://docs.rinho.com.ar/comandos/configuracion-general/configuraciones-y-consulta-de-parametros/comando-vr) |
| `>QVRE<` | `RVRE ...` | Adds modem / GPS / BLE firmware, FW type, CRC. [doc](https://docs.rinho.com.ar/comandos/configuracion-general/configuraciones-y-consulta-de-parametros/comando-vre) |
| `>QCEL<` / `>QCELnn<` | `RCEL,status,radio,mcc,mnc,rat,band,csq,creg,cereg,roaming,sim,reglvl,lac,cellid,rsrp,rsrq,sinr`. Example `RCEL,CONNECT,eMTC,722,310,eMTC,B28,24,1,1,0,0,7,7425,12345678,-95,-11,12` = connected, LTE-M, Claro AR (722/310), band 28, csq 24, creg 1, cereg 1, not roaming, SIM0, level 7, LAC 7425, cell 12345678, RSRP -95 dBm, RSRQ -11 dB, SINR 12 | `status`: SEARCH (no cell), LIMSRV (not registered), **NOCONN (registered, idle = normal)**, CONNECT (transmitting now). `csq` 0-31 (99 unknown; 0-9 bad, 10-14 fair, 15-19 good, 20+ excellent). LTE `rsrp` good ≥ -90 dBm, poor < -105; `rsrq` good ≥ -10 dB, poor < -15. `nn` = one field 00-16. Full 17 fields from fw 1.09.24; older returns `status,tech,mcc,mnc`. [doc](https://docs.rinho.com.ar/comandos/comunicaciones/status-de-conexion/comando-cel) |
| `>QCOPS<` / `>QCOP0<` `>QCOP1<` | `RCOP<sim><value>` | Empty = automatic. `2G`, `4G` = forced. `M,<plmn>` = manual operator. [doc](https://docs.rinho.com.ar/comandos/comunicaciones/datos-moviles/comando-cops) |
| `>QCXAPN<` / `>QCXAP1<` | `RCXAPN host,user,password` (`RCXAP1...` for SIM1) | APN per SIM; empty user/password is normal. **Contains the password: mask it.** [doc](https://docs.rinho.com.ar/comandos/comunicaciones/datos-moviles/comando-cxapn) |
| `>QCXAA<` | `RCXAA0` / `RCXAA1` | Auto-APN (only acts when no APN is set). |
| `>QIP0<` … `>QIP6<` | `RIP0"host",port` or `RIP0ip/port`; `RIPnERR` = slot not set | 0 main, 1 main redundant, 3 WiFi-only main, 4-5 backup channel, 6 backup WiFi. [doc](https://docs.rinho.com.ar/comandos/comunicaciones/datos-moviles/comando-ip) |
| `>QCXIP<` | `RCXIP<main><back><window>0` | Protocol per channel: P = UDP, T = TCP, W = Wialon IPS. Window 1-9. [doc](https://docs.rinho.com.ar/comandos/comunicaciones/datos-moviles/comando-cxip) |
| `>QMC<` | `RMC<mcc>` | Country code of the current network (722 = Argentina). [doc](https://docs.rinho.com.ar/comandos/comunicaciones/datos-moviles/comando-mc) |
| `>QWIF<` | `RWIF,pwr,apmode,stationindex,ssid,rssi,signallevel` | `pwr` 0/1, `apmode` 0 STA / 1 AP, `signallevel` 0-4. [doc](https://docs.rinho.com.ar/comandos/comunicaciones/status-de-conexion/comando-wif) |
| `>QWL<` | `RWL <password>` / `RWL *` | Wialon password. **Mask it.** [doc](https://docs.rinho.com.ar/comandos/comunicaciones/datos-moviles/comando-wl) |
| `>QWD<` | raw `#D#...` frame | The Wialon frame the device builds now (fw 1.09.19+). |
| `>QCFW<` | `RCFW<phase>,<pct>` | Modem firmware update status: 0 idle … 4 OK, 5 error. [doc](https://docs.rinho.com.ar/comandos/comunicaciones/datos-moviles/comando-cfw) |
| `>QBU<` | `RBU;GPRS=<n>;SMS=<n>` | Reports waiting to be sent per queue. **Main "is it delivering?" check.** [doc](https://docs.rinho.com.ar/comandos/configuracion-y-consulta-de-estados/datalog-y-buffer-de-reportes/comando-bu) |
| `>QDL<` | `RDLOK<total> <pending>[ BUSY]` | Datalog records: total, pending upload. `QBU` is the live report queue and `QDL` the stored log: either one growing means the device is not delivering. [doc](https://docs.rinho.com.ar/comandos/configuracion-y-consulta-de-estados/datalog-y-buffer-de-reportes/comando-dl) |
| `>QCXDL<` | `RCXDL<ena>,<dst>,<retries>,<time>,<order>` | Log auto-upload: destination, retries (0 = infinite), interval in tenths of s, 0 FIFO / 1 LIFO. [doc](https://docs.rinho.com.ar/comandos/configuracion-y-consulta-de-estados/datalog-y-buffer-de-reportes/comando-cxdl) |
| `>QUT<` | `RUT±bbbb` | Internal temperature, °C. [doc](https://docs.rinho.com.ar/comandos/configuracion-y-consulta-de-estados/cpu/comando-ut) |
| `>QIO<` | `RIO;IGN<0/1>;IN<bits>;XP<bits>;V<dV>;VBU<cV>` | Ignition; inputs MSB first (1 = open, 0 = to ground); outputs (1 = on); supply in tenths of V; backup in hundredths of V. E.g. `V127` = 12.7 V, `VBU420` = 4.20 V. [doc](https://docs.rinho.com.ar/comandos/configuracion-y-consulta-de-estados/entradas-y-salidas/comando-io) |
| `>QAINnn<` | `RAINnn±vvvv` | Analog input 00-05, conditioned value. [doc](https://docs.rinho.com.ar/comandos/configuracion-y-consulta-de-estados/entradas-y-salidas/comando-ain) |
| `>QCXAINnn<` | `RCXAINnn±BOTTOM±TOP±XA±XB±YA±YB` | Scaling of that analog input (XA/XB in hundredths of V). |
| `>QARnn<` | `RARaabbcddddeffffggg` | Analog threshold window: input, min, max, debounce. [doc](https://docs.rinho.com.ar/comandos/configuracion-y-consulta-de-estados/entradas-y-salidas/comando-ar) |
| `>QCXXP<` | `RCXXPaabbbbccccdddd` | Output config: duration, ON, OFF in tenths of s. All `9` = output disabled. [doc](https://docs.rinho.com.ar/comandos/configuracion-y-consulta-de-estados/entradas-y-salidas/comando-cxxp) |
| `>QITnn<` | `RITnn<on><off>` | Digital input debounce, tenths of s. Slot 07 is not ignition. |
| `>QCXIGN<` | `RCXIGNa,ton,toff` | Ignition source mode and debounce (on / off). [doc](https://docs.rinho.com.ar/comandos/configuracion-general/configuraciones-de-contexto/comando-cxign) |
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
| `QBUS` | Dumps every queued report over serial |
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
