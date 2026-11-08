# Pointing a device to a platform

Source: public docs "Comando IP", "Comando CXIP", "Comando CXAPN"
(https://docs.rinho.com.ar/comandos/comunicaciones/datos-moviles/). Fetch them when in doubt.

| Command | Meaning |
|---|---|
| `>SIP0"tracker.example.com",12100<` | Main server by hostname + port (quotes required) |
| `>SIP0190.168.1.100/5000<` | Main server by IP + port (`ip/port`, no quotes) |
| `>SIP1...<` | Main redundant server. `SIP3` WiFi-only main; `SIP4`/`SIP5` backup channel |
| `>QIP0<` | Reply `RIP0"host",port` or `RIP0ip/port`; `RIP0ERR` = slot not set |
| `>SCXIP<main><back><window>0<` | Protocol per channel: `P` UDP, `T` TCP, `W` Wialon IPS. `>SCXIPPP50<` = UDP/UDP, window 5. Reply `RCXIPPP50` |
| `>SCXAPN host,user,password<` | APN of SIM0 (`SCXAP1` for SIM1). Reply `RCXAPN host,user,password` (password in clear: mask it) |
| `>SCXAA1<` | Auto-APN (acts only when no APN is set) |

Typical "platform changed server" fix: one line, `>SIP0"new.host",port<` (and `SIP1` if they give a
second host). Same protocol → nothing else; the reports queued on the device drain to the new
server by themselves. Protocol change (`SCXIP`) → the Rinho recipes also send `>SCXDLE<`, which
cancels the pending datalog uploads built for the old protocol. **`SCXDLE` is an action, not
configuration: it is sent on its own from the configurator's monitor or the platform, never
inside the Code-mode block** (the validator marks it `ACTION_NOT_CONFIG`). Do not replace it with
`SCXDL0,...`: that disables the datalog upload permanently.

Secrets: `SCXAPN` and `SWL` carry passwords. Write them with a placeholder (`SCXAPN host,user,****`)
and tell the user to type the real password in the configurator; never ask them to paste it in the chat.
