# Rinho command index (public docs)

Generated from https://docs.rinho.com.ar/comandos (compat.json). One line per documented command: syntax, example,
and the minimum firmware per model. **Use this when you cannot fetch the docs page.** If a command
or a field is not here, say so to the user instead of guessing: the configurator rejects what it
doesn't know, and the device answers CMDERR silently.

Reading the syntax: letters in lower case are fields, digits in the example show the exact width.
Firmware: a device code absent means that model is not documented for the command; "—" = no
minimum stated. "1.09.28.T53" = release 1.09.28 build T53 (a release without .T is newer than
any T build of that release).

## comandos/comunicaciones

### Comando CID

Configuración de detección de llamadas entrantes por Caller ID con hasta 10 números autorizados  
Docs: https://docs.rinho.com.ar/comandos/comunicaciones/configuracion-audio/comando-cid

Firmware: Spider 1.00.00 · Smart 1.00.00

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `SCID` | set | `SCIDabcc..cc` | `SCID209999999999` | Configura un número telefónico para detección por Caller ID |
| `QCID` | query | `QCIDa` | `QCID2` | Consulta configuración de número telefónico por índice |

### Comando DS

Configuración de hasta 5 números telefónicos de destino para llamadas de voz salientes  
Docs: https://docs.rinho.com.ar/comandos/comunicaciones/configuracion-audio/comando-ds

Firmware: Spider 1.00.00 · Smart 1.00.00

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `SDS` | set | `SDSabb..bb` | `SDS0541155551234` | Configura un número telefónico de destino en el índice especificado |
| `QDS` | query | `QDSa` | `QDS0` | Consulta el número telefónico almacenado en el índice especificado |

### Comando VCC

Inicia una llamada de voz saliente a un número telefónico preconfigurado  
Docs: https://docs.rinho.com.ar/comandos/comunicaciones/configuracion-audio/comando-vcc

Firmware: Smart 1.00.00

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `SVCC` | set | `>SVCCa<` | `>SVCC0<` | Inicia una llamada de voz a un destino preconfigurado |

### Comando VCH

Finaliza una llamada de voz activa en dispositivos con audio incorporado  
Docs: https://docs.rinho.com.ar/comandos/comunicaciones/configuracion-audio/comando-vch

Firmware: Smart 1.00.00

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `SVCH` | action | `>SVCH<` | `>SVCH<` | Finaliza una llamada de voz activa |

### Comando CXSMS

Configura reintentos y tiempos para el envío de reportes vía SMS  
Docs: https://docs.rinho.com.ar/comandos/comunicaciones/configuracion-sms/comando-cxsms

Firmware: Spider 1.00.00 · Smart 1.00.00

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `SCXSMS` | set | `SCXSMSaabbbbb` | `SCXSMS0303000` | Configura cantidad de reintentos y tiempo entre reintentos para reportes vía SMS |
| `QCXSMS` | query | `QCXSMS` | `QCXSMS` | Consulta la configuración actual de reintentos SMS |

### Comando CXSMW

Habilita o deshabilita el filtrado de SMS entrantes mediante lista blanca de números autorizados  
Docs: https://docs.rinho.com.ar/comandos/comunicaciones/configuracion-sms/comando-cxsmw

Firmware: Zero 1.09.08 · Spider 1.09.08 · Smart 1.09.08

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `SCXSMW` | set | `SCXSMWE` | `SCXSMW1` | Habilita o deshabilita el filtrado de SMS entrantes mediante lista blanca |
| `QCXSMW` | query | `QCXSMW` | `QCXSMW` | Consulta el estado actual de la lista blanca SMS |

### Comando SM

Configuración de hasta 5 números telefónicos destino para envío de reportes vía SMS  
Docs: https://docs.rinho.com.ar/comandos/comunicaciones/configuracion-sms/comando-sm

Firmware: Zero 1.00.00 · Spider 1.00.00 · Smart 1.00.00

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `QSM` | query | `QSMa` | `QSM0` | Lee número del índice especificado |
| `SSM` | set | `SSMabb..bb` | `SSM0541155551234` | Guarda número telefónico en índice especificado |

### Comando APW

Configuración de hasta 10 puntos de acceso WiFi para conexión como cliente (modo Station)  
Docs: https://docs.rinho.com.ar/comandos/comunicaciones/configuracion-wifi/comando-apw

Firmware: Zero 1.00.00 · Spider 1.00.00 · Smart 1.00.00

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `QAPW` | query | `QAPWaa` | `QAPW00` | Consulta la configuración de un punto de acceso WiFi |
| `SAPW` | set | `SAPWaasid,pass,ip,subnetmask,gateway` | `SAPW00RedWifi,123456789` | Configura un punto de acceso WiFi para conexión como cliente |

### Comando APX

Configuración del dispositivo como punto de acceso WiFi (Access Point) para conexión directa  
Docs: https://docs.rinho.com.ar/comandos/comunicaciones/configuracion-wifi/comando-apx

Firmware: Zero 1.00.00 · Spider 1.00.00 · Smart 1.00.00

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `QAPX` | query | `QAPX` | `QAPX` | Consulta la configuración actual del Access Point WiFi |
| `SAPX` | set | `SAPXsid,pass,ip,subnetmask,gateway` | `SAPXMiRinho,ClaveSegura,192.168.4.1,255.255.255.0,192.168.4.1` | Configura el dispositivo como punto de acceso WiFi |

### Comando APY

Configuración del segundo Access Point de servicio, usado junto con CXWIF en modo apmode=2  
Docs: https://docs.rinho.com.ar/comandos/comunicaciones/configuracion-wifi/comando-apy

Firmware: Zero 1.09.19 · Spider 1.09.19 · Smart 1.09.19

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `QAPY` | query | `QAPY` | `QAPY` | Consulta la configuración del Access Point de servicio (APY) |
| `SAPY` | set | `SAPYsid,pass,ip,subnetmask,gateway` | `SAPYRedServicio,clave123` | Configura el segundo Access Point de servicio (APY) |

### Comando CXWIF

Configuración general del módulo WiFi: activación, modo de operación y reportes automáticos  
Docs: https://docs.rinho.com.ar/comandos/comunicaciones/configuracion-wifi/comando-cxwif

Firmware: Zero 1.00.00 (AP en modo cliente (SCXWIF1,2 y SAPY) desde v1.09.20) · Spider 1.00.00 (AP en modo cliente (SCXWIF1,2 y SAPY) desde v1.09.20) · Smart 1.00.00 (AP en modo cliente (SCXWIF1,2 y SAPY) desde v1.09.20)

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `QCXWIF` | query | `QCXWIF` | `QCXWIF` | Consulta la configuración actual del módulo WiFi |
| `SCXWIF` | set | `SCXWIFwifipwr,apmode,autodestiny` | `SCXWIF1,0,1` | Configura el comportamiento global del módulo WiFi |

### Comando WS

Configuración del servidor web integrado con botones personalizados, autenticación y consola de comandos  
Docs: https://docs.rinho.com.ar/comandos/comunicaciones/configuracion-wifi/comando-ws

Firmware: Zero 1.00.00 · Spider 1.00.00 · Smart 1.00.00

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `SWS` | set | `SWSa,b,cc..cc,dddd,e,botón1/cmd1,...` | `SWSE,0,Sistema de Control,1234,1,Open Lock/SSSU1211,Reset/SRESET` | Configura el servidor web embebido con botones personalizados y autenticación |
| `QWS` | query | `QWS` | `QWS` | Consulta la configuración actual del servidor web |
| `SWSD` | action | `SWSD` | `SWSD` | Deshabilita el servidor web integrado |

### Comando CFW

DFOTA del módem - actualización remota del firmware del módem celular mediante paquete delta direccional, con telemetría de fase y progreso  
Docs: https://docs.rinho.com.ar/comandos/comunicaciones/datos-moviles/comando-cfw

Firmware: Zero 1.09.24 · Spider 1.09.24 · Smart 1.09.24

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `SCFW` | set | `SCFW[^]"<url>.pack"` | `SCFW"http://fota.rinho.com.ar/dfota/v1-v2.pack"` | Dispara un DFOTA del firmware del módem descargando el paquete delta desde una URL. El prefijo ^ fuerza la actualización aun con batería baja (omite la verificación previa) |
| `QCFW` | query | `QCFW` | `QCFW` | Consulta la fase y el progreso del DFOTA del módem |

### Comando COPS

Configuración del registro de red y selección de operador (automático, RAT forzada 2G/4G, operador manual con fallback o string COPS crudo) más el gestor de registro robusto SCRM  
Docs: https://docs.rinho.com.ar/comandos/comunicaciones/datos-moviles/comando-cops

Firmware: Zero 1.00.00 (Modos 2G/4G/M y SCRM desde v1.09.24) · Spider 1.00.00 (Modos 2G/4G/M y SCRM desde v1.09.24) · Smart 1.00.00 (Modos 2G/4G/M y SCRM desde v1.09.24)

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `SCOP` | set | `SCOP<sim><valor>` | `SCOP04G` | Configura la política de registro de red por SIM (automático, RAT forzada, operador manual con fallback o string COPS crudo) |
| `QCOP` | query | `QCOP<sim>` | `QCOP0` | Consulta la política de registro de red configurada para una SIM |
| `SCRM` | set | `SCRM<n>` | `SCRM1` | Kill-switch del gestor de registro de red. SCRM1=activa (default), SCRM0=desactiva (modo legacy), SCRM=consulta |

### Comando CXAPN

Configura el APN para conexión de datos GPRS/LTE del operador celular  
Docs: https://docs.rinho.com.ar/comandos/comunicaciones/datos-moviles/comando-cxapn

Firmware: Zero 1.00.00 · Spider 1.00.00 · Smart 1.00.00

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `SCXAP` | set | `SCXAPxhost,usuario,password` | `SCXAPN internet.ctimovil.com.ar,,` | Configura el APN para conexión de datos GPRS/LTE del operador celular |
| `QCXAP` | query | `QCXAPx` | `QCXAPN` | Consulta la configuración de APN |
| `SCXAA` | set | `SCXAA[en]` | `SCXAA1` | Habilita o deshabilita el APN automático |
| `QCXAA` | query | `QCXAA` | `QCXAA` | Consulta si el APN automático está habilitado |

### Comando CXIP

Configura el protocolo TCP/UDP para servidores principal y backup  
Docs: https://docs.rinho.com.ar/comandos/comunicaciones/datos-moviles/comando-cxip

Firmware: Zero 1.00.00 · Spider 1.00.00 · Smart 1.00.00

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `SCXIP` | set | `SCXIP[proto_main][proto_back][ventana]` | `SCXIPPP5` | Configura el protocolo TCP/UDP/Wialon IPS para servidores principal y backup |
| `QCXIP` | query | `QCXIP` | `QCXIP` | Consulta la configuración actual del protocolo TCP/UDP |

### Comando CXUDPSRC

Configura el puerto de origen para transmisión UDP  
Docs: https://docs.rinho.com.ar/comandos/comunicaciones/datos-moviles/comando-cxudpsrc

Firmware: Spider 1.00.00 · Smart 1.00.00

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `SCXUDPSRC` | set | `SCXUDPSRCaaaa` | `SCXUDPSRC5000` | Configura el puerto de origen para transmisión UDP |
| `QCXUDPSRC` | query | `QCXUDPSRC` | `QCXUDPSRC` | Consulta el puerto de origen UDP configurado |

### Comando GPRSH

Desconexión forzada del socket UDP/TCP para aplicar cambios de configuración IP  
Docs: https://docs.rinho.com.ar/comandos/comunicaciones/datos-moviles/comando-gprsh

Firmware: Zero 1.00.00 · Spider 1.00.00 · Smart 1.00.00

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `SGPRSH` | action | `SGPRSH` | `SGPRSH` | Fuerza la desconexión del socket de comunicación UDP/TCP activo y dispara una reconexión del módulo WiFi |

### Comando IP

Configuración de hasta 7 destinos IP para envío de reportes por celular y WiFi con soporte de canales MAIN y BACK  
Docs: https://docs.rinho.com.ar/comandos/comunicaciones/datos-moviles/comando-ip

Firmware: Zero 1.00.00 · Spider 1.00.00 · Smart 1.00.00

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `SIP` | set | `SIPabbb.ccc.ddd.eee/ffff` | `SIP0190.168.1.100/5000` | Configura dirección IP y puerto de destino para reportes |
| `SIP` | set | `SIPa"dominio",ffff` | `SIP0"tracking.miempresa.com",5000` | Configura dominio DNS y puerto de destino para reportes |
| `QIP` | query | `QIPa` | `QIP0` | Consulta configuración de dirección IP específica |

### Comando MC

Configuración de hasta 8 disparadores por cambio de código de país (MCC) para detección de fronteras  
Docs: https://docs.rinho.com.ar/comandos/comunicaciones/datos-moviles/comando-mc

Firmware: Zero 1.00.00 · Spider 1.00.00 · Smart 1.00.00

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `SMC` | set | `SMCaabbb` | `SMC00748` | Configura código MCC (Mobile Country Code) para detección de país |
| `QMC` | query | `QMCaa` | `QMC00` | Consulta código MCC configurado en un índice específico |
| `QMC` | query | `QMC` | `QMC` | Consulta el MCC real de la red celular actual (sin índice), no un valor configurado en ningún disparador |

### Comando WL

Configuración de la password de Wialon IPS 1.0 y generación manual de mensajes de datos (#D# / #SD#) para integración con plataformas compatibles  
Docs: https://docs.rinho.com.ar/comandos/comunicaciones/datos-moviles/comando-wl

Firmware: Zero 1.09.19 (Wialon IPS 1.0) · Spider 1.09.19 (Wialon IPS 1.0) · Smart 1.09.19 (Wialon IPS 1.0)

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `SWL` | set | `SWLpassword` | `SWL1234` | Configura la password (clave de autenticación) del protocolo Wialon IPS 1.0 |
| `QWL` | query | `QWL` | `QWL` | Consulta la password de Wialon IPS 1.0 configurada |
| `QWD` | query | `QWD[n]` | `QWD` | Genera manualmente un mensaje de datos Wialon IPS (data #D# o short data #SD#) |

### Comando CXJMD

Configuración del tiempo de espera para confirmar fin de interferencia de jamming  
Docs: https://docs.rinho.com.ar/comandos/comunicaciones/deteccion-de-jamming/comando-cxjmd

Firmware: Zero 1.00.00 · Spider 1.00.00 · Smart 1.00.00

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `SCXJMD` | set | `SCXJMD[tiempo]` | `SCXJMD015` | Configura el tiempo de espera antes de confirmar que una interferencia de jamming ha finalizado |
| `QCXJMD` | query | `QCXJMD` | `QCXJMD` | Consulta la configuración actual del tiempo de espera para detección de jamming |

### Comando JMD

Detección de jammers o inhibidores de señal celular con señal de estado para motor de eventos  
Docs: https://docs.rinho.com.ar/comandos/comunicaciones/deteccion-de-jamming/comando-jmd

Firmware: Spider 1.00.00 · Smart 1.00.00

### Comando CXSIM

Configura el modo de selección automática o manual de tarjeta SIM  
Docs: https://docs.rinho.com.ar/comandos/comunicaciones/seleccion-de-sim/comando-cxsim

Firmware: Zero 1.00.00 · Spider 1.00.00 · Smart 1.00.00

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `SCXSIM` | set | `SCXSIMa` | `SCXSIM1` | Configura el modo de selección de SIM (automático o manual) |
| `QCXSIM` | query | `QCXSIM` | `QCXSIM` | Consulta el modo de selección de SIM configurado |

### Comando SIM

Selección y cambio entre las dos SIMcards disponibles en dispositivos Dual SIM  
Docs: https://docs.rinho.com.ar/comandos/comunicaciones/seleccion-de-sim/comando-sim

Firmware: Smart 1.00.00

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `SSIM` | set | `SSIMa` | `SSIM1` | Selecciona la SIMcard activa en dispositivos Dual SIM |
| `QSIM` | query | `QSIM` | `QSIM` | Consulta cuál SIMcard está activa |

### Comando CEL

Consulta del estado de conexión celular con telemetría fina de señal (RSRP/RSRQ/SINR), localización por celda (LAC/Cell ID), tecnología, operador y estado LPWA  
Docs: https://docs.rinho.com.ar/comandos/comunicaciones/status-de-conexion/comando-cel

Firmware: Zero 1.00.00 (Telemetría fina (17 campos) desde v1.09.24) · Spider 1.00.00 (Telemetría fina (17 campos) desde v1.09.24) · Smart 1.00.00 (Telemetría fina (17 campos) desde v1.09.24)

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `QCEL` | query | `QCEL` | `QCEL` | Consulta el estado de conexión celular en tiempo real (respuesta completa de 17 campos) |
| `QCEL` | query | `QCEL[nn]` | `QCEL14` | Consulta un único campo del estado celular por índice (00 a 16, siempre dos dígitos) |

### Comando WIF

Consulta del estado de conexión WiFi incluyendo modo, SSID, intensidad y calidad de señal  
Docs: https://docs.rinho.com.ar/comandos/comunicaciones/status-de-conexion/comando-wif

Firmware: Spider 1.00.00 (Scan QWIFN desde v1.09.26) · Smart 1.00.00 (Scan QWIFN desde v1.09.26)

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `QWIF` | query | `QWIF` | `QWIF` | Consulta el estado de la conexión WiFi del dispositivo |
| `QWIFN` | query | `QWIFN` | `QWIFN` | Escanea las redes WiFi visibles desde el equipo (diagnóstico de instalación) |

## comandos/configuracion-consulta-procesos-y-calculos-referentes-al-gps

### Comando CXCQ

Configura filtros de calidad GPS para reportes y umbrales de velocidad  
Docs: https://docs.rinho.com.ar/comandos/configuracion-consulta-procesos-y-calculos-referentes-al-gps/comando-cxcq

Firmware: Spider 1.00.00

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `SCXCQ` | set | `SCXCQabc` | `SCXCQ31004` | Configura filtros de calidad GPS para reportes y umbrales de velocidad |
| `QCXCQ` | query | `QCXCQ` | `QCXCQ` | Consulta configuración del filtro de calidad GPS |

### Comando CXGPS

Configura el modo de actualización de datos GPS del dispositivo  
Docs: https://docs.rinho.com.ar/comandos/configuracion-consulta-procesos-y-calculos-referentes-al-gps/comando-cxgps

Firmware: Zero 1.00.00 · Spider 1.00.00 · Smart 1.00.00

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `SCXGPS` | set | `SCXGPSa` | `SCXGPS1` | Configura el modo de actualización de datos GPS del dispositivo |
| `QCXGPS` | query | `QCXGPS` | `QCXGPS` | Consulta el modo de actualización GPS configurado |

### Comando CXOD

Configura parámetros de calibración y ajuste del odómetro GPS  
Docs: https://docs.rinho.com.ar/comandos/configuracion-consulta-procesos-y-calculos-referentes-al-gps/comando-cxod

Firmware: Spider 1.00.00

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `SCXOD` | set | `SCXODab` | `SCXOD5110` | Configura parámetros de calibración y ajuste del odómetro GPS |
| `QCXOD` | query | `QCXOD` | `QCXOD` | Consulta la configuración actual del odómetro GPS |

### Comando DC

Configura disparadores para detectar cambios de rumbo por ángulo de giro  
Docs: https://docs.rinho.com.ar/comandos/configuracion-consulta-procesos-y-calculos-referentes-al-gps/comando-dc

Firmware: Spider 1.00.00

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `SDC` | set | `SDCaabccc` | `SDC00*045` | Configura disparadores para detectar cambios de rumbo por ángulo de giro |
| `QDC` | query | `QDCaa` | `QDC00` | Obtiene configuración actual de disparador de cambio de rumbo |

### Comando DV

Configura disparadores para detectar aceleraciones y frenadas bruscas mediante GPS  
Docs: https://docs.rinho.com.ar/comandos/configuracion-consulta-procesos-y-calculos-referentes-al-gps/comando-dv

Firmware: Zero 1.00.00 · Spider 1.00.00 · Smart 1.00.00

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `SDV` | set | `SDVaabccc` | `SDV01-050` | Define tipo y magnitud de la detección de aceleración/frenada |
| `QDV` | query | `QDVaa` | `QDV01` | Obtiene configuración actual del disparador de aceleración/frenada |

### Comando OD

Consulta, reseteo y configuración de odómetros de distancia recorrida total y parcial  
Docs: https://docs.rinho.com.ar/comandos/configuracion-consulta-procesos-y-calculos-referentes-al-gps/comando-od

Firmware: Zero 1.00.00 · Spider 1.00.00 · Smart 1.00.00

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `QOD` | query | `QOD` | `QOD` | Consulta los valores de ambos odómetros (total y parcial) |
| `SODT` | action | `SODT[,valor]` | `SODT,150.5` | Reinicia el odómetro total a cero, o lo configura a un valor específico |
| `SODP` | action | `SODP[,valor]` | `SODP,25.3` | Reinicia el odómetro parcial a cero, o lo configura a un valor específico |

### Comando PK

Configura el modo parking con geocerca virtual para detectar si el vehículo es remolcado  
Docs: https://docs.rinho.com.ar/comandos/configuracion-consulta-procesos-y-calculos-referentes-al-gps/comando-pk

Firmware: Zero 1.00.00 · Spider 1.00.00 · Smart 1.00.00

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `SPKE` | set | `SPKE[bbbbcccc]` | `SPKE00500015` | Habilita el modo parking cuando se envía sin parámetros. Con parámetros solo guarda radio y tiempo, sin habilitar |
| `SPKD` | set | `SPKD[bbbbcccc]` | `SPKD` | Deshabilita el modo parking cuando se envía sin parámetros. Con parámetros solo guarda radio y tiempo, sin deshabilitar |
| `SPKA` | action | `SPKA` | `SPKA` | Captura la posición actual como centro de la geocerca (resetea estado) |
| `QPK` | query | `QPK` | `QPK` | Obtiene la configuración actual del modo parking |

### Comando RG

Definición y gestión de geocercas poligonales de 3 a 32 vértices  
Docs: https://docs.rinho.com.ar/comandos/configuracion-consulta-procesos-y-calculos-referentes-al-gps/comando-rg

Firmware: Spider 1.00.00 (32 regiones) · Smart 1.00.00 (32 regiones)

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `SRG` | set | `SRGaab,+LLLLLLL+XXXXXXXX,+LLLLLLL+XXXXXXXX,...` | `SRG000,-3459418-05842814,-3459500-05842736,-3459423-05842623` | Configura una región geográfica (geocerca poligonal) con múltiples vértices |
| `QRG` | query | `QRGaab` | `QRG000` | Consulta la configuración de una región geográfica específica |
| `SRG` | action | `SRG*55AA` | `SRG*55AA` | Borra todas las regiones geográficas configuradas |

### Comando TK

Monitor de inclinación mediante acelerómetro para detectar grúas, vuelcos o movimientos no autorizados  
Docs: https://docs.rinho.com.ar/comandos/configuracion-consulta-procesos-y-calculos-referentes-al-gps/comando-tk

Firmware: Zero 1.09.12 · Spider 1.09.12 · Smart 1.09.12

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `STK` | set | `STK[mode][,auto,stab_time,detect_time,angle]` | `STKE,0,10,2,3.5` | Configura el monitor de inclinación mediante acelerómetro |
| `QTK` | query | `QTK` | `QTK` | Consulta el estado del monitor de inclinación |

### Comando VL

Configura hasta 16 límites de velocidad con alertas automáticas  
Docs: https://docs.rinho.com.ar/comandos/configuracion-consulta-procesos-y-calculos-referentes-al-gps/comando-vl

Firmware: Zero 1.00.00 · Spider 1.00.00 · Smart 1.00.00

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `SVL` | set | `SVLaaubbbbcddd` | `SVL00E00500010` | Configura un límite de velocidad con parámetros específicos |
| `QVL` | query | `QVLaa` | `QVL00` | Consulta la configuración de un límite de velocidad específico |

### Comando WP

Configura hasta 32 waypoints o geocercas circulares con alertas de entrada/salida  
Docs: https://docs.rinho.com.ar/comandos/configuracion-consulta-procesos-y-calculos-referentes-al-gps/comando-wp

Firmware: Zero 1.00.00 · Spider 1.00.00 · Smart 1.00.00

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `SWP` | set | `SWPaab+/-ccccccc+/-ddddddddeeeeeeffffff` | `SWP00E-2778467-06425658002000000000` | Configura un waypoint o geocerca circular completa |
| `QWP` | query | `QWPaa` | `QWP00` | Consulta la configuración de un waypoint específico |

### Comando ZONES

Descarga asíncrona de geocercas por HTTP hacia el equipo (SZONES), con cancelación (SZONESC), borrado del slot (SZONESD) y consulta de estado (QZONES)  
Docs: https://docs.rinho.com.ar/comandos/configuracion-consulta-procesos-y-calculos-referentes-al-gps/comando-zones

Firmware: Zero 1.09.21 (SZONESC / SZONESD desde v1.09.21) · Spider 1.09.21 (SZONESC / SZONESD desde v1.09.21) · Smart 1.09.21 (SZONESC / SZONESD desde v1.09.21)

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `SZONES` | action | `SZONES"url"` | `SZONES"http://host/zones.bin"` | Inicia la descarga asíncrona de un archivo de geocercas por HTTP GET |
| `SZONESC` | action | `SZONESC` | `SZONESC` | Cancela una descarga de geocercas en curso (idempotente) |
| `SZONESD` | action | `SZONESD` | `SZONESD` | Descarga y borra el slot de geocercas del equipo (alias limpio del legacy SZONES**D) |
| `QZONES` | query | `QZONES` | `QZONES` | Consulta el estado del slot de geocercas (cargado / vacío / deshabilitado) |
| `SZMODE` | set | `SZMODE[modo][F]` | `SZMODE1` | Habilita o deshabilita el almacenamiento de geocercas. Reinicia el equipo |
| `QZMODE` | query | `QZMODE` | `QZMODE` | Consulta el modo de geocercas y el reparto de memoria |
| `SZFRAC` | set | `SZFRAC[fraccion]` | `SZFRAC8` | Define qué porción de la memoria se reserva para geocercas |
| `QZFRAC` | query | `QZFRAC` | `QZFRAC` | Consulta la porción de memoria reservada para geocercas |

### Comandos LAT y LON

Configuración manual de coordenadas GPS para equipos de ubicación fija  
Docs: https://docs.rinho.com.ar/comandos/configuracion-consulta-procesos-y-calculos-referentes-al-gps/comandos-lat-y-lon

Firmware: Zero 1.00.00 · Spider 1.00.00 · Smart 1.00.00

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `SLAT` | set | `SLATvalor` | `SLAT-26.82414` | Configura manualmente la latitud del dispositivo en grados decimales (equipos fijos o sin señal GPS) |
| `QLAT` | query | `QLAT` | `QLAT` | Consulta la latitud actualmente configurada/filtrada |
| `SLON` | set | `SLONvalor` | `SLON-65.20619` | Configura manualmente la longitud del dispositivo en grados decimales |
| `QLON` | query | `QLON` | `QLON` | Consulta la longitud actualmente configurada/filtrada |

## comandos/configuracion-general

### Comando KO

Configura modo de bajo consumo y ahorro de energía del dispositivo GPS  
Docs: https://docs.rinho.com.ar/comandos/configuracion-general/alimentacion/comando-ko

Firmware: Zero 1.00.00 · Spider 1.00.00 · Smart 1.00.00

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `SKO` | set | `SKOabbbbccccdddd` | `SKOE072002400240` | Configura modo de bajo consumo y ahorro de energía del dispositivo |
| `QKO` | query | `QKO` | `QKO` | Consulta configuración del modo de bajo consumo |
| `SKO` | action | `SKO+` | `SKO+` | Fuerza DORMIDO TOTAL si IGN está desconectado |
| `SKO` | action | `SKO-` | `SKO-` | Reinicia servicio, enciende GPS y GSM |
| `SKO` | action | `SKO++` | `SKO++` | Fuerza ULTRA BAJO CONSUMO incluyendo CPU |
| `SKO` | set | `SKO++aaaabcdefg` | `SKO++0060001000` | Configura modo ultra bajo consumo extendido con encendido periódico |
| `QKO` | query | `QKO++` | `QKO++` | Consulta configuración del modo ultra bajo consumo extendido |

### Comando CXXB

Configura capturadores de texto para procesar datos de puertos seriales y módulos  
Docs: https://docs.rinho.com.ar/comandos/configuracion-general/buffers-de-captura/comando-cxxb

Firmware: Spider 1.00.00 · Smart 1.00.00

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `SCXXB` | set | `SCXXBaabdlllttt,s..s,e..e,m,hf` | `SCXXB00M0004001,+CSQ: ,\2C,A,0` | Configura capturadores de texto para procesar datos de puertos seriales y módulos |
| `QCXXB` | query | `QCXXBaa` | `QCXXB00` | Consulta la configuración de un capturador de texto específico |

### Comando XB

Buffers de captura de texto para almacenar datos de periféricos y generar reportes personalizados  
Docs: https://docs.rinho.com.ar/comandos/configuracion-general/buffers-de-captura/comando-xb

Firmware: Zero 1.00.00 · Spider 1.00.00 · Smart 1.00.00

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `SXB` | set | `SXBindiceTexto` | `SXB001234567890` | Carga texto en un buffer de captura específico |
| `QXB` | query | `QXBindice` | `QXB00` | Consulta el contenido de un buffer de captura |

### Comando CXBP

Configura parámetros del módem satelital STELLAR DS100 para red ORBCOMM  
Docs: https://docs.rinho.com.ar/comandos/configuracion-general/configuraciones-de-contexto/comando-cxbp

Firmware: Spider 1.00.00 (Requiere módulo STELLAR DS100)

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `SCXBP` | set | `SCXBPaabcde` | `SCXBP01020101` | Configura parámetros del módem satelital STELLAR DS100 para red ORBCOMM |
| `QCXBP` | query | `QCXBP` | `QCXBP` | Consulta parámetros del módem satelital STELLAR DS100 |

### Comando CXCELDEB

Configura tiempos de anti-rebote para la señal de conexión celular CEL  
Docs: https://docs.rinho.com.ar/comandos/configuracion-general/configuraciones-de-contexto/comando-cxceldeb

Firmware: Spider 1.00.00 · Smart 1.00.00

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `SCXCELDEB` | set | `SCXCELDEBaaabbb` | `SCXCELDEB010060` | Configura tiempos de anti-rebote para la señal de conexión celular CEL |
| `QCXCELDEB` | query | `QCXCELDEB` | `QCXCELDEB` | Consulta la configuración de anti-rebote de la señal CEL |

### Comando CXIGN

Configura el modo de funcionamiento de la señal de ignición (IGN) y sus tiempos de estabilización  
Docs: https://docs.rinho.com.ar/comandos/configuracion-general/configuraciones-de-contexto/comando-cxign

Firmware: Zero 1.00.00 (Umbrales SCXIGNV desde v1.09.26.T15) · Spider 1.00.00 (Umbrales SCXIGNV desde v1.09.26.T15) · Smart 1.00.00 (Umbrales SCXIGNV desde v1.09.26.T15)

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `SCXIGN` | set | `SCXIGNa[,ton[,toff]]` | `SCXIGN0,50,150` | Configura el modo de funcionamiento de la señal de ignición (IGN) y los tiempos de estabilización |
| `QCXIGN` | query | `QCXIGN` | `QCXIGN` | Consulta el modo de funcionamiento actual de la ignición y sus tiempos |
| `SCXIGNV` | set | `SCXIGNV1,on,off \| SCXIGNV0` | `SCXIGNV1,138,131` | Fija umbrales manuales de tensión para el modo 2 (ignición por tensión) o vuelve a los automáticos |
| `QCXIGNV` | query | `QCXIGNV` | `QCXIGNV` | Consulta los umbrales de tensión efectivos y la tensión medida en el momento |

### Comando CXRF

Habilita el módulo receptor RF433 MHz para códigos ASK/OOK  
Docs: https://docs.rinho.com.ar/comandos/configuracion-general/configuraciones-de-contexto/comando-cxrf

Firmware: legacy devices only

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `SCXRF` | set | `SCXRFa` | `SCXRF1` | Configura el estado del módulo receptor RF433 MHz |
| `QCXRF` | query | `QCXRF` | `QCXRF` | Consulta el estado del módulo receptor RF433 MHz |

### Comando CXTAIP

Ajusta el formato de las tramas TAIP — orden del número de mensaje, tipo de checksum y etiqueta de canal en las colas drenadas  
Docs: https://docs.rinho.com.ar/comandos/configuracion-general/configuraciones-de-contexto/comando-cxtaip

Firmware: Zero 1.00.00 (Etiqueta de canal (3er dígito) desde v1.09.25) · Spider 1.00.00 (Etiqueta de canal (3er dígito) desde v1.09.25) · Smart 1.00.00 (Etiqueta de canal (3er dígito) desde v1.09.25)

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `SCXTAIP` | set | `SCXTAIPo[c[l]]` | `SCXTAIP012` | Configura orden del número de mensaje, checksum y etiqueta de canal |
| `QCXTAIP` | query | `QCXTAIP` | `QCXTAIP` | Consulta la configuración TAIP vigente |

### Comando CXTRM

Configura la velocidad de comunicación del puerto serial  
Docs: https://docs.rinho.com.ar/comandos/configuracion-general/configuraciones-de-contexto/comando-cxtrm

Firmware: Spider 1.00.00 (38400 y 57600 bps desde v1.09.28.T40) · Smart 1.00.00 (38400 y 57600 bps desde v1.09.28.T40)

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `SCXTR` | set | `SCXTRpaa..aa` | `SCXTR09600` | Configura la velocidad (baudrate) del puerto serial/terminal |
| `QCXTR` | query | `QCXTRp` | `QCXTR0` | Consulta la velocidad configurada del puerto serial/terminal |

### Comando CXTX

Configura el formato de reporte generado por el comando TX  
Docs: https://docs.rinho.com.ar/comandos/configuracion-general/configuraciones-de-contexto/comando-cxtx

Firmware: Spider 1.00.00 · Smart 1.00.00

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `SCXTX` | set | `SCXTXab` | `SCXTX15` | Configura el tipo de reporte que se genera al ejecutar el comando TX |
| `QCXTX` | query | `QCXTX` | `QCXTX` | Consulta la configuración actual del tipo de reporte TX |

### Comando CXTY

Configura el modo de captura transparente del puerto serial  
Docs: https://docs.rinho.com.ar/comandos/configuracion-general/configuraciones-de-contexto/comando-cxty

Firmware: Spider 1.00.00 · Smart 1.00.00

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `SCXTY` | set | `SCXTYaabbcd` | `SCXTY020300` | Configura el modo de captura transparente del puerto serial |
| `QCXTY` | query | `QCXTY` | `QCXTY` | Consulta la configuración del modo de captura transparente |

### Comando CXVC

Configura parámetros de audio para llamadas de voz  
Docs: https://docs.rinho.com.ar/comandos/configuracion-general/configuraciones-de-contexto/comando-cxvc

Firmware: Spider 1.00.00 · Smart 1.00.00

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `SCXVC` | set | `SCXVCaaabbcc` | `SCXVC1800605` | Configura parámetros de audio para llamadas de voz (duración, ganancia micrófono, volumen speaker) |
| `QCXVC` | query | `QCXVC` | `QCXVC` | Consulta configuración actual de parámetros de audio para llamadas de voz |

### Comando CXVIN

Calibra las lecturas de tensión de las entradas analógicas del dispositivo  
Docs: https://docs.rinho.com.ar/comandos/configuracion-general/configuraciones-de-contexto/comando-cxvin

Firmware: Spider 1.00.00 · Smart 1.00.00

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `QCXVIN` | query | `QCXVIN` | `QCXVIN` | Consulta el factor de calibración actual de las entradas analógicas |
| `SCXVIN` | set | `SCXVINbbbb` | `SCXVIN2400` | Calibra las lecturas de tensión de las entradas analógicas |

### Comando RESET

Reinicia el equipo por software, confirmando antes de resetear  
Docs: https://docs.rinho.com.ar/comandos/configuracion-general/configuraciones-y-consulta-de-parametros/comando-reset

Firmware: Zero 1.00.00 · Spider 1.00.00 · Smart 1.00.00

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `SRESET` | action | `SRESET` | `SRESET` | Reinicia el equipo por software |

### Comando RTC

Sincronización del reloj interno del dispositivo GPS mediante NTP, HTTP o configuración manual  
Docs: https://docs.rinho.com.ar/comandos/configuracion-general/configuraciones-y-consulta-de-parametros/comando-rtc

Firmware: Zero 1.09.17 (NTP/HTTP + manual; WiFi según modelo) · Spider 1.09.17 (NTP/HTTP + manual) · Smart 1.09.17 (NTP/HTTP + manual)

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `SRTC` | set | `SRTC AUTO` | `SRTC AUTO` | Sincroniza automáticamente probando todos los métodos disponibles |
| `SRTC` | set | `SRTC CEL NTP` | `SRTC CEL NTP` | Sincroniza via celular usando protocolo NTP |
| `SRTC` | set | `SRTC CEL HTTP` | `SRTC CEL HTTP` | Sincroniza via celular usando API HTTP |
| `SRTC` | set | `SRTC WIF NTP` | `SRTC WIF NTP` | Sincroniza via WiFi usando protocolo NTP nativo |
| `SRTC` | set | `SRTC WIF HTTP` | `SRTC WIF HTTP` | Sincroniza via WiFi usando API HTTP |
| `SRTC` | set | `SRTCddmmaahhmmss` | `SRTC181224143025` | Configura fecha y hora manualmente |

### Comando TAG

Etiqueta de configuración para identificar y documentar la programación del dispositivo GPS  
Docs: https://docs.rinho.com.ar/comandos/configuracion-general/configuraciones-y-consulta-de-parametros/comando-tag

Firmware: Spider 1.00.00 · Smart 1.00.00

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `STAG` | set | `STAGetiqueta` | `STAG CONFIGURACION BASICA` | Configura una etiqueta de identificación para el dispositivo |
| `QTAG` | query | `QTAG` | `QTAG` | Consulta la etiqueta actual del dispositivo |

### Comando VR

Consulta la versión de firmware, modelo, hardware y módulos del dispositivo GPS  
Docs: https://docs.rinho.com.ar/comandos/configuracion-general/configuraciones-y-consulta-de-parametros/comando-vr

Firmware: Zero 1.00.00 · Spider 1.00.00 · Smart 1.00.00

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `QVR` | query | `QVR` | `QVR` | Consulta información completa sobre el firmware instalado |

### Comando VRE

Consulta de versión extendida - la misma respuesta de VR más la cantidad de recursos (flags, timers, reglas, geocercas...) que maneja el firmware  
Docs: https://docs.rinho.com.ar/comandos/configuracion-general/configuraciones-y-consulta-de-parametros/comando-vre

Firmware: Zero 1.00.00 · Spider 1.00.00 · Smart 1.00.00

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `QVRE` | query | `QVRE` | `QVRE` | Consulta la versión del firmware y la cantidad de recursos configurables que maneja |

### Configura ID

Configura el identificador único del dispositivo GPS incluido en todos los mensajes transmitidos  
Docs: https://docs.rinho.com.ar/comandos/configuracion-general/configuraciones-y-consulta-de-parametros/configura-id

Firmware: Zero 1.00.00 · Spider 1.00.00 · Smart 1.00.00

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `SID` | set | `SIDidentificador` | `SIDFLOTA01` | Configura el identificador alfanumérico del dispositivo, incluido en todos los mensajes transmitidos |
| `QID` | query | `QID` | `QID` | Consulta el identificador actualmente configurado |

### Consulta IMEI

Consulta el número IMEI del módulo celular para identificación y registro del dispositivo  
Docs: https://docs.rinho.com.ar/comandos/configuracion-general/configuraciones-y-consulta-de-parametros/consulta-imei

Firmware: Zero 1.00.00 · Spider 1.00.00 · Smart 1.00.00

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `QIMEI` | query | `QIMEI` | `QIMEI` | Consulta el IMEI de 15 dígitos del módulo celular |

### Consulta SN

Consulta el número de serie electrónico del dispositivo para trazabilidad y garantías  
Docs: https://docs.rinho.com.ar/comandos/configuracion-general/configuraciones-y-consulta-de-parametros/consulta-sn

Firmware: Zero 1.00.00 · Spider 1.00.00 · Smart 1.00.00

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `QSN` | query | `QSN` | `QSN` | Consulta el número de serie electrónico del dispositivo |

## comandos/configuracion-y-consulta-de-estados

### Comando BE IoT

Configuración de hasta 5 disparadores por detección de presencia de sensores iBeacon con radio y timeout configurables.  
Docs: https://docs.rinho.com.ar/comandos/configuracion-y-consulta-de-estados/bluetooth-low-energy-ble/comando-be-iot

Firmware: Zero 1.00.00 · Spider 1.00.00 · Smart 1.00.00

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `QBE` | query | `QBE` | `QBE` | Consulta la configuración de disparadores por detección de presencia de sensores iBeacon |
| `SBE` | set | `SBEaabbbbbbbbbbbb,cc..cc,dd..dd` | `SBE00AC233F252487,5,10` | Configura un disparador por detección de presencia de sensores iBeacon |

### Comando BS IoT

Configuración de hasta 4 sensores BLE con soporte para iBeacon, Eddystone, sensores Rinho y Escort.  
Docs: https://docs.rinho.com.ar/comandos/configuracion-y-consulta-de-estados/bluetooth-low-energy-ble/comando-bs-iot

Firmware: Zero 1.00.00 · Spider 1.00.00 · Smart 1.00.00

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `SBS` | set | `SBSaab,id,type,time,distance,outval` | `SBS01E,A1B2C3D4E5F6,30,60,10,INVALID` | Configura un sensor Bluetooth Low Energy (BLE) |
| `QBS` | query | `QBSaa.atributo` | `QBS01.fuel_lvl` | Consulta el valor de un atributo específico del sensor BLE |

### Comando BSW IoT

Sistema de auto-detección Bluetooth Smart Wildcard con filtrado por UUID, Major, Minor y distancia para iBeacon.  
Docs: https://docs.rinho.com.ar/comandos/configuracion-y-consulta-de-estados/bluetooth-low-energy-ble/comando-bsw-iot

Firmware: Zero 1.00.00 · Spider 1.00.00 · Smart 1.00.00

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `SBSW` | set | `SBSWiiE,targets[,UUID[,major[,minor[,timeout[,distance]]]]]` | `SBSW00E,FF,2F234454CF6D4A0FADF2F4911BA9FFA6,,,5,2` | Configura sistema BSW (Bluetooth Smart Wildcard) para detección automática de iBeacon |
| `SBSW` | action | `SBSWiiE` | `SBSW00E` | Habilitar BSW con configuración existente |
| `SBSW` | action | `SBSWiiD` | `SBSW00D` | Deshabilitar BSW manteniendo configuración |
| `SBSW` | action | `SBSWiiX` | `SBSW00X` | Desconectar dispositivo actual de BSW |
| `SBSW` | action | `SBSWii*` | `SBSW00*` | Limpiar completamente configuración BSW |
| `QBSW` | query | `QBSWii` | `QBSW00` | Consulta configuración y estado de BSW |
| `QBSW` | query | `QBSWii.campo` | `QBSW00.active` | Consulta campo específico de BSW |

### Comando BTH IoT

Consulta el estado de la conexión Bluetooth (alimentación, stack, cliente conectado, MAC y RSSI) y expone la señal BTH para reglas y EVAL  
Docs: https://docs.rinho.com.ar/comandos/configuracion-y-consulta-de-estados/bluetooth-low-energy-ble/comando-bth-iot

Firmware: Zero 1.09.26 · Spider 1.09.26 · Smart 1.09.26

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `QBTH` | query | `QBTH[ii]` | `QBTH` | Consulta el estado completo de la conexión Bluetooth |

### Comando CXBTH

Configura el módulo Bluetooth BLE para comunicación SPP  
Docs: https://docs.rinho.com.ar/comandos/configuracion-y-consulta-de-estados/bluetooth-low-energy-ble/comando-cxbth

Firmware: Zero 1.01.17 (Requiere firmware .nimble.ota) · Spider 1.01.17 (Requiere firmware .nimble.ota) · Smart 1.01.17 (Requiere firmware .nimble.ota)

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `SCXBTH` | set | `SCXBTHa,nombre,pin,timeout,peer` | `SCXBTH1,MiDispositivo,1234,10,TRACTOR_01` | Configura el módulo Bluetooth BLE para comunicación SPP y el enlace entre equipos |
| `QCXBTH` | query | `QCXBTH` | `QCXBTH` | Consulta la configuración del módulo Bluetooth BLE |

### Comando CANPF

Perfilador del bus CAN: escucha el bus durante un tiempo y reporta qué identificadores aparecen, con qué frecuencia y con qué datos.  
Docs: https://docs.rinho.com.ar/comandos/configuracion-y-consulta-de-estados/canbus/comando-canpf

Firmware: Spider 1.09.20 (Sólo equipos con bus CAN. Desde v1.09.20 (T1)) · Smart 1.09.20 (Sólo equipos con bus CAN. Desde v1.09.20 (T1))

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `SCANPF` | set | `SCANPF[segundos][;@destino]` | `SCANPF60;@LOG` | Inicia el perfilado del bus CAN durante N segundos |
| `QCANPF` | query | `QCANPF[índice]` | `QCANPF03` | Consulta el resultado del perfilado, o el detalle de un identificador |

### Comando CXCAN

Configura el módulo CAN para lectura de datos del vehículo  
Docs: https://docs.rinho.com.ar/comandos/configuracion-y-consulta-de-estados/canbus/comando-cxcan

Firmware: Spider 1.00.00 (Parámetros eee y f desde v1.02.06; 1M desde v1.09.21) · Smart 1.00.00 (Parámetros eee y f desde v1.02.06; 1M desde v1.09.21)

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `SCXCAN` | set | `SCXCANa,bbbb,cc..cc,dd,eee,f` | `SCXCAN1,500K,OBDII,11,100,0` | Configura el módulo CAN para lectura de datos del vehículo |
| `QCXCAN` | query | `QCXCAN` | `QCXCAN` | Consulta la configuración actual del módulo CAN |

### Comando CXDTC

Lectura de códigos de falla de la ECU (DTC): J1939 (SPN/FMI) en camiones y maquinaria, OBD2 (P0xxx) en livianos.  
Docs: https://docs.rinho.com.ar/comandos/configuracion-y-consulta-de-estados/canbus/comando-cxdtc

Firmware: Spider 1.09.27 · Smart 1.09.27

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `SCXDTC` | set | `SCXDTC[en][,agesec[,busclr[,autorpt[,destino]]]]` | `SCXDTC1,3,0,1,GP` | Habilita y configura la lectura de códigos de falla de la ECU |
| `QCXDTC` | query | `QCXDTC` | `QCXDTC` | Consulta la configuración actual del módulo de fallas |
| `QDTC` | query | `QDTC` | `QDTC` | Consulta la lista de fallas activas en el momento |
| `CDTC` | command | `CDTC` | `CDTC` | Borra la lista de fallas almacenada en el equipo |

### Comando CXECU

Configuración de hasta 29 parsers personalizados para extraer datos específicos de mensajes CAN del vehículo.  
Docs: https://docs.rinho.com.ar/comandos/configuracion-y-consulta-de-estados/canbus/comando-cxecu

Firmware: Spider 1.00.00 · Smart 1.00.00

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `SCXECU` | set | `SCXECU[idx][enable],[can_id],[start_bit],[length],[factor],[offset],[min],[max],[endian],[signed]` | `SCXECU00E,3E8,24,16,0.125,0,0,8000,BE,U` | Configura parsers personalizados para extraer datos específicos de mensajes CAN |
| `QCXECU` | query | `QCXECU[idx]` | `QCXECU00` | Consulta la configuración de un parser ECU específico |
| `CCXECU` | action | `CCXECU[idx] \| CCXECU99` | `CCXECU00` | Borra la configuración de un parser ECU específico, o de todos con CCXECU99 |

### Comando CXEKU

Variante protegida de configuración de parser CAN/ECU (uso interno de fábrica)  
Docs: https://docs.rinho.com.ar/comandos/configuracion-y-consulta-de-estados/canbus/comando-cxeku

Firmware: Spider 1.09.19 · Smart 1.09.19

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `SCXEKU` | set | `SCXEKU` | `SCXEKU` | Carga una configuración de parser CAN/ECU en formato protegido (uso interno de Rinho) |
| `QCXEKU` | query | `QCXEKU[idx]` | `QCXEKU00` | Consulta la configuración protegida de un parser CAN/ECU cargado por SCXEKU (uso interno de Rinho) |

### Comando ECU

Consulta de variables CAN del vehículo con formato de salida y timeout configurables.  
Docs: https://docs.rinho.com.ar/comandos/configuracion-y-consulta-de-estados/canbus/comando-ecu

Firmware: Spider 1.00.00 · Smart 1.00.00

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `QECU` | query | `QECUxxaa..aa:bb..bb:cc..cc` | `QECU00%d:2:-40` | Consulta variables CAN del vehículo con formato de salida y timeout configurables |

### Comandos CANxx

Familia de comandos CAN00-CAN05 para lectura de parámetros del vehículo vía J1939, OBD-II y protocolos de trailer.  
Docs: https://docs.rinho.com.ar/comandos/configuracion-y-consulta-de-estados/canbus/comandos-canxx

Firmware: Spider 1.00.00 · Smart 1.00.00

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `QCAN` | query | `QCAN00` | `QCAN00` | Lee el banco de parámetros J1939 (RPM, acelerador, odómetro, combustible, temperatura, presión de aceite) para vehículos pesados/comerciales |
| `QCAN` | query | `QCAN01` | `QCAN01` | Lee el banco de parámetros OBD-II/J1850 para vehículos livianos, incluyendo VIN |
| `QCAN` | query | `QCAN02` | `QCAN02` | Banco reservado para trailer ABS/EBS; en el firmware actual la rama está deprecada y no devuelve ningún dato |
| `QCAN` | query | `QCAN03` | `QCAN03` | Lee el banco J1939 en formato de ancho fijo, pensado para integrarse en reportes de usuario Ux |
| `QCAN` | query | `QCAN05` | `QCAN05` | Lee la tasa de consumo de combustible (FuelRate) por hardware y por software, y el sensor MAF |

### Comando UT

Consulta la temperatura interna del CPU del dispositivo para monitoreo térmico y alertas por sobrecalentamiento  
Docs: https://docs.rinho.com.ar/comandos/configuracion-y-consulta-de-estados/cpu/comando-ut

Firmware: Zero 1.00.00 · Spider 1.00.00 · Smart 1.00.00

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `QUT` | query | `QUT` | `QUT` | Consulta la temperatura interna del CPU del dispositivo |

### Comando BU

Consulta y gestiona el buffer de reportes pendientes para GPRS, SMS y Satelital con opciones de borrado y descarga  
Docs: https://docs.rinho.com.ar/comandos/configuracion-y-consulta-de-estados/datalog-y-buffer-de-reportes/comando-bu

Firmware: Spider 1.00.00 · Smart 1.00.00

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `QBU` | query | `QBU` | `QBU` | Consulta la cantidad de reportes guardados en el buffer interno |
| `CBU` | action | `CBU` | `CBU` | Borra la cola de reportes acumulados para destinos GPRS y SMS |
| `QBUS` | query | `QBUS` | `QBUS` | Descarga todos los reportes acumulados en el buffer a través del puerto serial |
| `CBUORB` | action | `CBUORB` | `CBUORB` | Borra los reportes encolados para destino satelital ORBCOMM |

### Comando CXDL

Configura el envío automático de registros del datalog al servidor  
Docs: https://docs.rinho.com.ar/comandos/configuracion-y-consulta-de-estados/datalog-y-buffer-de-reportes/comando-cxdl

Firmware: Spider 1.05.00 (Campo Order y QCXDLST desde v1.09.23) · Smart 1.05.00 (Campo Order y QCXDLST desde v1.09.23)

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `SCXDL` | set | `SCXDL<Ena>,<Dst>,<Reint>,<Time>,<Order>` | `SCXDL1,WIF,3,100,0` | Configura el envío automático de registros del datalog al servidor |
| `SCXDLE` | action | `SCXDLE` | `SCXDLE` | Cancela el envío de reportes pendientes manteniendo los datos en el LOG |
| `QCXDL` | query | `QCXDL` | `QCXDL` | Consulta la configuración de envío automático de datalog |
| `QCXDLST` | query | `QCXDLST` | `QCXDLST` | Consulta el estado runtime del motor de envío de datalog (diagnóstico) |

### Comando DL

Datalogger de 50k registros con almacenamiento masivo, filtros por fecha/evento y descarga ordenada de reportes  
Docs: https://docs.rinho.com.ar/comandos/configuracion-y-consulta-de-estados/datalog-y-buffer-de-reportes/comando-dl

Firmware: Zero 1.05.00 (Parámetros opcionales v1.08.02+) · Spider 1.05.00 (Parámetros opcionales v1.08.02+) · Smart 1.05.00 (Parámetros opcionales v1.08.02+)

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `QDL` | query | `QDL` | `QDL` | Consulta la cantidad de reportes totales y pendientes en el datalog |
| `SDLC` | set | `SDLC` | `SDLC` | Borrado lógico - marca los registros como descargados sin eliminarlos |
| `SDLF` | set | `SDLF` | `SDLF` | Borrado físico - elimina físicamente todos los registros del datalog |
| `QDL` | query | `QDLabb..bb` | `QDL+,I070324123000,E080324150050,F0099F0` | Consulta registros del datalog con orden y filtros opcionales |
| `QDLC` | query | `QDLC` | `QDLC` | Cancela la descarga de registros en curso |
| `QDL` | query | `QDLa,A` | `QDL+,A` | Dump forense - entrega todo el log ignorando el piso de descarga (cxdlFloor). Solo lectura, combinable con los filtros de fecha I/E |

### Calibración de Ejes

Calibración del acelerómetro para mediciones precisas de inclinación, movimiento y detección de maniobras  
Docs: https://docs.rinho.com.ar/comandos/configuracion-y-consulta-de-estados/dinamica-de-movimiento/calibracion-de-ejes

Firmware: Zero 1.09.15 · Spider 1.09.15 · Smart 1.09.15

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `SCALINT` | action | `SCALINT` | `SCALINT` | Inicia la calibración inteligente automática: detecta reposo para Roll/Pitch y usa GPS estable para Yaw, reintentando hasta completarse |
| `SCALSTART` | action | `SCALSTART` | `SCALSTART` | Inicia la calibración automática completa (Roll/Pitch en reposo + Yaw por detección de movimiento frontal) |
| `SCALSTART1` | action | `SCALSTART1` | `SCALSTART1` | Inicia sólo la calibración de Roll/Pitch mediante detección de reposo |
| `SCALSTART2` | action | `SCALSTART2` | `SCALSTART2` | Inicia sólo la calibración de Yaw mediante detección de movimiento frontal |
| `SCALSTOP` | action | `SCALSTOP` | `SCALSTOP` | Detiene cualquier proceso de calibración automática en curso |
| `SCAL` | set | `SCALenable[,roll][,pitch][,yaw][,ox][,oy][,oz]` | `SCAL1,10,20,30` | Habilita/deshabilita la calibración manual y opcionalmente fija ángulos y offsets específicos; los campos omitidos preservan el valor actual |
| `QCAL` | query | `QCAL` | `QCAL` | Consulta la configuración actual de calibración (habilitado, ángulos y offsets) |
| `QCALMAT` | query | `QCALMAT` | `QCALMAT` | Consulta la matriz de transformación completa de calibración |
| `QCALSTAT` | query | `QCALSTAT` | `QCALSTAT` | Consulta el estado actual del proceso de calibración automática |

### Comando AC

Consulta valores de aceleración en los 3 ejes (X, Y, Z) del acelerómetro integrado para detección de movimiento y maniobras  
Docs: https://docs.rinho.com.ar/comandos/configuracion-y-consulta-de-estados/dinamica-de-movimiento/comando-ac

Firmware: Spider 1.00.00 · Smart 1.00.00

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `QAC` | query | `QAC` | `QAC` | Consulta los valores de aceleración en los 3 ejes del acelerómetro integrado |

### Comando CXAC

Configura el umbral de detección del acelerómetro interno  
Docs: https://docs.rinho.com.ar/comandos/configuracion-y-consulta-de-estados/dinamica-de-movimiento/comando-cxac

Firmware: Zero 1.00.00 · Spider 1.00.00 · Smart 1.00.00

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `SCXAC` | set | `SCXACaaa` | `SCXAC016` | Configura el umbral de disparo para la detección de movimiento del acelerómetro |
| `QCXAC` | query | `QCXAC` | `QCXAC` | Consulta el umbral actual del acelerómetro |

### Comando HM

Configura 8 detectores de maniobras bruscas (aceleración, frenada, giros, choques) por GPS y acelerómetro  
Docs: https://docs.rinho.com.ar/comandos/configuracion-y-consulta-de-estados/dinamica-de-movimiento/comando-hm

Firmware: Zero 1.09.15 · Spider 1.09.15 · Smart 1.09.15

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `SHM` | set | `SHMxx[E\|D\|R][,eventType,magnitude,min_speed_kmh,hysteresis_percent,time_window_ms,cooldown_s]` | `SHM00E,0,7,5,0,1000,5` | Configura un detector de maniobras bruscas por GPS o acelerómetro |
| `QHM` | query | `QHMxx` | `QHM00` | Consulta la configuración actual de un detector HM |

### Comando MVT

Configura umbrales de movimiento y vibración en los 3 ejes del acelerómetro para detección anti-robo y alarmas  
Docs: https://docs.rinho.com.ar/comandos/configuracion-y-consulta-de-estados/dinamica-de-movimiento/comando-mvt

Firmware: Spider 1.00.00 · Smart 1.00.00

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `SMVT` | set | `SMVTaabbbbccccdddd` | `SMVT00015001500150` | Configura los deltas de detección de movimiento en los 3 ejes del acelerómetro para una instancia |
| `QMVT` | query | `QMVTaa` | `QMVT00` | Consulta los deltas de movimiento configurados para una instancia |

### Comando UV

Disparadores de usuario configurables para monitorear variables internas y detectar cambios en valores del dispositivo GPS  
Docs: https://docs.rinho.com.ar/comandos/configuracion-y-consulta-de-estados/disparadores-de-usuario/comando-uv

Firmware: Zero 1.00.00 · Spider 1.00.00 · Smart 1.00.00

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `QUV` | query | `QUVnn` | `QUV00` | Consulta configuración del disparador de usuario |
| `SUV` | set | `SUVnnabb.bb,cc,dd,ee,ff` | `SUV00UQIO,7,1,1,2` | Configura disparador de usuario completo |
| `SUV` | action | `SUVnn*` | `SUV00*` | Elimina disparador específico |
| `SUV` | action | `SUV*55AA` | `SUV*55AA` | Elimina todos los disparadores de usuario |
| `SUV` | action | `SUVnnR` | `SUV00R` | Pone a 0 la señal UVnn (reset) |

### Comando AIN

Lectura y consulta de entradas analógicas AD0-AD5 con valores de tensión acondicionados  
Docs: https://docs.rinho.com.ar/comandos/configuracion-y-consulta-de-estados/entradas-y-salidas/comando-ain

Firmware: Zero 1.00.00 (2 entradas AD) · Spider 1.00.00 (3 entradas AD) · Smart 1.00.00 (7 entradas AD)

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `QAIN` | query | `QAINíndice` | `QAIN00` | Lee el valor de tensión acondicionada en una entrada analógica específica |

### Comando AR

Configuración de umbrales analógicos para tensión y temperatura con ventanas de disparo  
Docs: https://docs.rinho.com.ar/comandos/configuracion-y-consulta-de-estados/entradas-y-salidas/comando-ar

Firmware: Zero 1.00.00 (8 umbrales, debounce v1.04.03+, DT2-DT7) · Spider 1.00.00 (8 umbrales, debounce v1.04.03+, DT2-DT7) · Smart 1.00.00 (8 umbrales, debounce v1.04.03+, DT2-DT7)

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `QAR` | query | `QARaa` | `QAR02` | Consulta configuración de umbrales analógicos para tensión y temperatura |
| `SAR` | set | `SARaabbcddddeffffggg` | `SAR0206+1050+1380` | Configura umbrales analógicos para generar eventos AR+ y AR- |

### Comando CXAIN

Configura la interpretación y transformación de señales analógicas en las entradas AD mediante interpolación lineal de dos puntos  
Docs: https://docs.rinho.com.ar/comandos/configuracion-y-consulta-de-estados/entradas-y-salidas/comando-cxain

Firmware: Zero 1.00.00 · Spider 1.00.00 · Smart 1.00.00

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `SCXAIN` | set | `SCXAINaabccccdeeeefgggghiiiijkkkklmmmm` | `SCXAIN01+0000+0100+0000+0500+0000+0100` | Configura la curva de conversión de una entrada analógica (dos puntos + clamp de salida) |
| `QCXAIN` | query | `QCXAINaa` | `QCXAIN01` | Consulta la configuración de una entrada analógica específica |

### Comando CXOW

Habilita el módulo OneWire para sensores de temperatura y llaves iButton  
Docs: https://docs.rinho.com.ar/comandos/configuracion-y-consulta-de-estados/entradas-y-salidas/comando-cxow

Firmware: Spider 1.02.03 (Parámetro b desde v1.09.04 (modo 2, auto-asignación de DT, desde v1.09.28.T16); c desde v1.09.28.T61) · Smart 1.02.03 (Parámetro b desde v1.09.04 (modo 2, auto-asignación de DT, desde v1.09.28.T16); c desde v1.09.28.T61)

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `SCXOW` | set | `SCXOWa,b,c` | `SCXOW1,0,1` | Configura el módulo OneWire para sensores de temperatura y llaves iButton |
| `QCXOW` | query | `QCXOW` | `QCXOW` | Consulta la configuración del módulo OneWire |

### Comando CXXP

Configura el modo de activación de salidas digitales con soporte de blink y temporización  
Docs: https://docs.rinho.com.ar/comandos/configuracion-y-consulta-de-estados/entradas-y-salidas/comando-cxxp

Firmware: Spider 1.00.00 · Smart 1.00.00

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `SCXXP` | set | `SCXXPaabbbbccccdddd` | `SCXXP00000500010004` | Configura el modo de activación de salidas digitales con soporte de blink y temporización |
| `QCXXP` | query | `QCXXP` | `QCXXP` | Consulta la configuración de salidas digitales |

### Comando DT (equipos legacy)

Consulta de temperatura OneWire en equipos legacy (C3, C5, SP, UL), con uno o dos sensores  
Docs: https://docs.rinho.com.ar/comandos/configuracion-y-consulta-de-estados/entradas-y-salidas/comando-dt

Firmware: legacy devices only

### Comando DT IoT

Gestión de hasta 8 sensores de temperatura OneWire en dispositivos IoT  
Docs: https://docs.rinho.com.ar/comandos/configuracion-y-consulta-de-estados/entradas-y-salidas/comando-dt-iot

Firmware: Spider 1.00.00 · Smart 1.00.00

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `QDT` | query | `QDT` | `QDT` | Consulta todas las temperaturas de los 8 sensores OneWire |
| `SDT` | set | `SDTaab,cccccccccccccccc` | `SDT071,2887F03F0700007F` | Configura ID único para un sensor específico |
| `QDT` | query | `QDTaa` | `QDT07` | Consulta sensor específico por índice |

### Comando FC IoT

Medidores de frecuencia y disparadores por umbral para dispositivos IoT  
Docs: https://docs.rinho.com.ar/comandos/configuracion-y-consulta-de-estados/entradas-y-salidas/comando-fc-iot

Firmware: Spider 1.05.02 (1 puerto FCT) · Smart 1.05.02 (2 puertos FCT)

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `QFC` | query | `QFCaa` | `QFC00` | Consulta el valor de frecuencia de un medidor |
| `SFC` | set | `SFCaaSbb..bb,cc..cc` | `SFC00S10000,1` | Configura factor de escala y base de tiempo para medidor de frecuencia |
| `QFCT` | query | `QFCTaa` | `QFCT00` | Consulta configuración de disparador por umbral de frecuencia |
| `SFCT` | set | `SFCTaabccccc` | `SFCT00012000` | Configura disparador por umbral de frecuencia |

### Comando IO

QIO - Consulta completa de estados de entradas, salidas, ignición y voltajes  
Docs: https://docs.rinho.com.ar/comandos/configuracion-y-consulta-de-estados/entradas-y-salidas/comando-io

Firmware: Zero 1.00.00 · Spider 1.00.00 · Smart 1.00.00

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `QIO` | query | `QIO` | `QIO` | Consulta el estado completo de entradas, salidas, ignición y voltajes |

### Comando IT

Configuración de tiempos de debounce (antirebote) para entradas digitales  
Docs: https://docs.rinho.com.ar/comandos/configuracion-y-consulta-de-estados/entradas-y-salidas/comando-it

Firmware: Zero 1.00.00 (IN00-IN02, IGN) · Spider 1.00.00 (IN00-IN06, IGN) · Smart 1.00.00 (IN00-IN03, IGN)

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `SIT` | set | `SITíndice retardo_on retardo_off` | `SIT0200100003` | Configura tiempos de debounce para entradas digitales e ignición |
| `QIT` | query | `QITíndice` | `QIT02` | Consulta configuración de debounce de una entrada |

### Comando OW

Consulta de dispositivos OneWire, llaves iButton y sensores de temperatura  
Docs: https://docs.rinho.com.ar/comandos/configuracion-y-consulta-de-estados/entradas-y-salidas/comando-ow

Firmware: Spider 1.00.00 (1 puerto OW) · Smart 1.00.00 (2 puertos OW)

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `QOW` | query | `QOWxx` | `QOW00` | Consulta el estado de dispositivos conectados a los puertos OneWire |
| `OWP00` | generate | `OWP00` | `OWP00` | Evento de actividad en puerto OneWire 0 |
| `OWP01` | generate | `OWP01` | `OWP01` | Evento de actividad en puerto OneWire 1 |

### Comando PB IoT

Contador de ráfagas de pulsos — lee códigos que un equipo externo emite como cantidad de pulsos (códigos de destello, fallas de controladores)  
Docs: https://docs.rinho.com.ar/comandos/configuracion-y-consulta-de-estados/entradas-y-salidas/comando-pb-iot

Firmware: Spider 1.09.25 · Smart 1.09.25

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `SPB` | set | `SPBii[,]gap,deb,par,min,max` | `SPB00,200,20,1,2,32` | Configura el contador de ráfagas en una línea |
| `SPB` | set | `SPBiiGms \| SPBiiDms \| SPBiiPp \| SPBiiRmin,max \| SPBiiZ` | `SPB00G200` | Ajusta un parámetro individual (G=gap, D=debounce, P=paridad, R=rango) o resetea el contador (Z) |
| `QPB` | query | `QPBii` | `QPB00` | Consulta configuración y último conteo del puerto |

### Comando PC IoT

Contadores de pulsos de alta velocidad para odómetro, combustible y sensores  
Docs: https://docs.rinho.com.ar/comandos/configuracion-y-consulta-de-estados/entradas-y-salidas/comando-pc-iot

Firmware: Spider 1.05.02 (1 puerto PC) · Smart 1.05.02 (2 puertos PC)

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `QPC` | query | `QPCaa` | `QPC00` | Consulta el valor y parámetros de un contador de pulsos |
| `SPC` | set | `SPCaaKbb..bb` | `SPC00K500` | Configura el factor de escala de un contador de pulsos |
| `SPC` | set | `SPCaaScc..cc` | `SPC00S1000.500` | Define el valor actual del contador de pulsos |
| `SPC` | action | `SPCaaR` | `SPC00R` | Reinicia el contador de pulsos a cero |

### Comando GC

Sistema de corte gradual de motor (Gradual Cut) para detención controlada y segura de vehículos a distancia  
Docs: https://docs.rinho.com.ar/comandos/configuracion-y-consulta-de-estados/senales/comando-gc

Firmware: Zero 1.00.00 · Spider 1.00.00 · Smart 1.00.00

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `SGC` | set | `SGC[comando][,xp,step,interval,max,invert]` | `SGCE,1,20,5,300,0` | Configura y controla el sistema de corte gradual de motor |
| `QGC` | query | `QGC` | `QGC` | Consulta el estado actual del sistema de corte gradual |

### Comando SS

Consulta y configuración de estados de señales del Motor de Eventos  
Docs: https://docs.rinho.com.ar/comandos/configuracion-y-consulta-de-estados/senales/comando-ss

Firmware: Zero 1.00.00 · Spider 1.00.00 · Smart 1.00.00

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `QSS` | query | `QSSseñal` | `QSSGPS` | Consulta el estado de una señal sin índice |
| `QSS` | query | `QSSseñalbb` | `QSSWP05` | Consulta el estado de una señal con índice |
| `SSS` | set | `SSSseñalcc` | `SSSGPSPWR11` | Configura el estado de una señal sin índice |
| `SSS` | set | `SSSseñalbbcc` | `SSSU0011` | Configura el estado de una señal con índice |

### Comando HO

Horómetro para contabilizar tiempo de uso en maquinaria y vehículos  
Docs: https://docs.rinho.com.ar/comandos/configuracion-y-consulta-de-estados/timers-y-contadores/comando-ho

Firmware: Spider 1.00.00 · Smart 1.00.00

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `QHO` | query | `QHO` | `QHO` | Consulta el estado y tiempo acumulado del horómetro |
| `SHO` | set | `SHOa` | `SHOE` | Configura la acción del horómetro (iniciar, detener o reiniciar) |
| `SHOS` | set | `SHOS,valor` | `SHOS,150.5` | Configura el horómetro a un valor específico en horas |

### Comando NT

Contadores aritméticos numéricos para contar eventos y generar disparos  
Docs: https://docs.rinho.com.ar/comandos/configuracion-y-consulta-de-estados/timers-y-contadores/comando-nt

Firmware: Spider 1.00.00 · Smart 1.00.00

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `SNT` | set | `SNTaabbbbb` | `SNT0200299` | Configura el valor máximo de un contador numérico |
| `QNT` | query | `QNTaa` | `QNT02` | Consulta el valor actual de un contador numérico |
| `SNT` | set | `SNTaa+n` | `SNT02+30` | Incrementa el valor de un contador numérico |
| `SNT` | set | `SNTaa-n` | `SNT02-15` | Decrementa el valor de un contador numérico |
| `SNT` | set | `SNTaaR` | `SNT03R` | Reinicia el contador a cero |
| `SNT` | set | `SNTaaS` | `SNT03S` | Pone el contador al valor máximo configurado |

### Comando TD

Timers y contadores de tiempo/distancia para generar disparos periódicos  
Docs: https://docs.rinho.com.ar/comandos/configuracion-y-consulta-de-estados/timers-y-contadores/comando-td

Firmware: Spider 1.00.00 · Smart 1.00.00

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `STD` | set | `STDaabcccccddddd` | `STD00E0030000000` | Configura un timer/contador de tiempo y distancia |
| `STD` | set | `STDaabccccc` | `STD00E00300` | Configura un timer/contador solo por tiempo (formato abreviado) |
| `QTD` | query | `QTDaa` | `QTD00` | Consulta la configuración de un timer específico |

### Comando TR

Disparadores programables por fecha y hora usando el reloj RTC  
Docs: https://docs.rinho.com.ar/comandos/configuracion-y-consulta-de-estados/timers-y-contadores/comando-tr

Firmware: Spider 1.00.00 · Smart 1.00.00

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `STR` | set | `STRaabbccdd` | `STR00083000` | Configura un disparador programable por fecha y hora |
| `QTR` | query | `QTRaa` | `QTR00` | Consulta la configuración de un disparador por hora |

## comandos/gestion-y-logistica

### Comando DRV

Chofer del equipo - un solo conductor por equipo que alimenta a Wialon, Traccar, el TRIP, las plantillas y las reglas  
Docs: https://docs.rinho.com.ar/comandos/gestion-y-logistica/comando-drv

Firmware: Zero 1.09.28.T53 (Ventana, aviso y corte desde v1.09.28.T54; reportes automáticos y confirmación desde v1.09.28.T56; chofer por Bluetooth (SCXDRVB) desde v1.09.28.T57; tacógrafo desde v1.09.28.T58; habilitación H desde v1.09.28.T60) · Spider 1.09.28.T53 (Ventana, aviso y corte desde v1.09.28.T54; reportes automáticos y confirmación desde v1.09.28.T56; chofer por Bluetooth (SCXDRVB) desde v1.09.28.T57; tacógrafo desde v1.09.28.T58; habilitación H desde v1.09.28.T60) · Smart 1.09.28.T53 (Ventana, aviso y corte desde v1.09.28.T54; reportes automáticos y confirmación desde v1.09.28.T56; chofer por Bluetooth (SCXDRVB) desde v1.09.28.T57; tacógrafo desde v1.09.28.T58; habilitación H desde v1.09.28.T60)

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `SCXDRV` | set | `SCXDRVorigen,lista,retención,ventana,aviso,corte,velcorte,rptlogin,rptlogout,rptinválido,rptsinchofer,dst,xpok` | `SCXDRV2,1,60,120,2,G,3,U101H,U102H,U103H,U104H,LOG,2` | Configura de dónde sale el chofer, el listado, la retención y la exigencia de chofer con aviso y corte |
| `QCXDRV` | query | `QCXDRV` | `QCXDRV` | Consulta la configuración del chofer |
| `SCXDRVB` | set | `SCXDRVBslot,modo,id,toggle` | `SCXDRVB1,P,I,0` | Configura el chofer por Bluetooth sobre un slot BSW (iBeacon o Eddystone-UID) |
| `QCXDRVB` | query | `QCXDRVB` | `QCXDRVB` | Consulta el chofer por Bluetooth |
| `QDRV` | query | `QDRV` | `QDRV` | Consulta el chofer actual |
| `SDRV` | set | `SDRV código` | `SDRV JPEREZ01` | Login manual del chofer |
| `CDRV` | action | `CDRV` | `CDRV` | Logout del chofer actual |

### Comando IC

Gestión de sesiones de conductor para control de flotas y jornadas laborales  
Docs: https://docs.rinho.com.ar/comandos/gestion-y-logistica/comando-ic

Firmware: Zero 1.00.00 (Manual. Slot ICL en RIC desde v1.09.23) · Spider 1.00.00 (iButton, RF433, BTH, Manual. Slot ICL en RIC desde v1.09.23) · Smart 1.00.00 (iButton, BTH, Manual. Slot ICL en RIC desde v1.09.23)

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `SIC` | set | `SIC código,modo` | `SIC AA01234567890123,0` | Inicia una sesión de conductor con código de identificación y modo |
| `SIC` | set | `SIC ,modo` | `SIC ,1` | Cambia el modo de la sesión activa manteniendo la misma sesión |
| `SIC` | action | `SIC*` | `SIC*` | Finaliza la sesión de conductor activa |
| `QIC` | query | `QIC` | `QIC` | Consulta los datos de la sesión de conductor activa |

### Comando ICL

Gestión de lista de códigos autorizados para control de acceso  
Docs: https://docs.rinho.com.ar/comandos/gestion-y-logistica/comando-icl

Firmware: Zero 1.00.00 (Manual) · Spider 1.00.00 (iButton, RF433, BT, Manual) · Smart 1.00.00 (iButton, BT, Manual)

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `SICL` | set | `SICLpp código` | `SICL00 JUANPEREZ` | Guarda código de identificación en posición específica |
| `SICL` | set | `SICL+ código` | `SICL+ CHOFERNUEVO` | Agrega código en primera posición libre de la lista |
| `QICL` | query | `QICLpp` | `QICL00` | Consulta código almacenado en posición específica |
| `SICL` | generate | `SICLpp #n` | `SICL05 #8` | Genera código numérico aleatorio de n dígitos en posición específica |
| `SICL` | action | `SICLpp*` | `SICL02*` | Elimina código en posición específica |
| `SICL` | action | `SICL*S` | `SICL*S` | Elimina código de sesión activa |
| `SICL` | action | `SICL*55AA` | `SICL*55AA` | Elimina toda la lista de códigos (confirmación obligatoria) |

## comandos/modo-diagnostico-e-ingenieria

### Comando QWD

Genera en demanda la trama Wialon IPS (#D# o #SD#) con los datos actuales del equipo, para diagnóstico de la integración  
Docs: https://docs.rinho.com.ar/comandos/modo-diagnostico-e-ingenieria/comando-qwd

Firmware: Zero 1.09.19 · Spider 1.09.19 · Smart 1.09.19

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `QWD` | query | `QWD[t]` | `QWD` | Genera la trama Wialon de datos con la posición y telemetría actual |

## comandos/modulo-trip

### Configuración

Parámetros de IDLE, NEUTRAL y configuración del módulo TRIP  
Docs: https://docs.rinho.com.ar/comandos/modulo-trip/configuracion

Firmware: Zero 1.09.20 (Módulo TRIP en Zero IoT desde v1.09.20) · Spider 1.09.17 (Requiere firmware con el módulo TRIP (desde v1.09.17 T8); sin él los comandos TP responden error) · Smart 1.09.17 (Requiere firmware con el módulo TRIP (desde v1.09.17 T8); sin él los comandos TP responden error)

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `STPIDLECFG` | set | `STPIDLECFG,<time_s>,<rpm>` | `STPIDLECFG,300,1000` | Configura tiempo mínimo y RPM máximo para considerar el vehículo en estado IDLE |
| `QTPIDLECFG` | query | `QTPIDLECFG` | `QTPIDLECFG` | Consulta la configuración de umbrales IDLE |
| `STPNEUCFG` | set | `STPNEUCFG,<speed_kmh>` | `STPNEUCFG,30` | Configura la velocidad máxima para detectar el estado NEUTRAL |
| `QTPNEUCFG` | query | `QTPNEUCFG` | `QTPNEUCFG` | Consulta la velocidad configurada para NEUTRAL |
| `STPNEUEN` | set | `STPNEUEN,<enable>` | `STPNEUEN,1` | Habilita o deshabilita la detección de estado NEUTRAL |
| `QTPNEUEN` | query | `QTPNEUEN` | `QTPNEUEN` | Consulta si la detección NEUTRAL está habilitada |

### Contadores GC

Gestión de contadores genéricos de eventos de conducción  
Docs: https://docs.rinho.com.ar/comandos/modulo-trip/contadores-gc

Firmware: Zero 1.09.20 (Módulo TRIP en Zero IoT desde v1.09.20) · Spider 1.09.17 (Requiere firmware con el módulo TRIP (desde v1.09.17 T8); sin él los comandos TP responden error) · Smart 1.09.17 (Requiere firmware con el módulo TRIP (desde v1.09.17 T8); sin él los comandos TP responden error)

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `QTPGC` | query | `QTPGC,<index>` | `QTPGC,0` | Consulta el valor de un contador genérico individual |
| `QTPGCL` | query | `QTPGCL` | `QTPGCL` | Lista el estado de habilitación de los 8 contadores GC |
| `STPGCE` | set | `STPGCE,<index>,<enable>` | `STPGCE,0,1` | Habilita o deshabilita un contador genérico (o todos con '*') |
| `STPGCI` | set | `STPGCI,<index>` | `STPGCI,0` | Incrementa en +1 un contador genérico |
| `STPGCS` | set | `STPGCS,<index>,<value>` | `STPGCS,0,0` | Establece el valor de un contador genérico directamente |

### Control de Viajes

Comandos para iniciar, detener y consultar estado de viajes  
Docs: https://docs.rinho.com.ar/comandos/modulo-trip/control-de-viajes

Firmware: Zero 1.09.20 (Módulo TRIP en Zero IoT desde v1.09.20) · Spider 1.09.17 (Requiere firmware con el módulo TRIP (desde v1.09.17 T8); sin él los comandos TP responden error) · Smart 1.09.17 (Requiere firmware con el módulo TRIP (desde v1.09.17 T8); sin él los comandos TP responden error)

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `STPSTART` | action | `STPSTART[,<driver_code>]` | `STPSTART,CHOFER001` | Inicia un nuevo viaje, capturando GPS y timestamp de inicio |
| `STPSTOP` | action | `STPSTOP[,<flags>][;@<dst>]` | `STPSTOP;@GPRS` | Finaliza el viaje activo; opcionalmente emite el viaje completo a un destino con ;@dst |
| `STPSEND` | action | `STPSEND[,<flags>];@<dst>` | `STPSEND;@GPRS` | Emite el viaje (actual o el último finalizado) hacia un destino, dividido en partes; de solo lectura, también se acepta por QTP |
| `QTPSEND` | query | `QTPSEND[,<flags>];@<dst>` | `QTPSEND,H;@GPRS` | Variante de solo lectura de STPSEND: emite el viaje hacia un destino (equivalente a STPSEND) |
| `QTPSTATUS` | query | `QTPSTATUS` | `QTPSTATUS` | Consulta el estado actual del viaje de forma compacta |
| `STPDRV` | set | `STPDRV[,<codigo>]` | `STPDRV,JUAN123` | Asigna o limpia el código de conductor del viaje activo |
| `QTPDRV` | query | `QTPDRV` | `QTPDRV` | Consulta el código de conductor actual |
| `QTPRPM` | query | `QTPRPM` | `QTPRPM` | Consulta RPM promedio y máximo del viaje activo o del último finalizado |
| `QTPSPD` | query | `QTPSPD` | `QTPSPD` | Consulta velocidad promedio y máxima del viaje activo o del último finalizado |
| `QTPTEMP` | query | `QTPTEMP` | `QTPTEMP` | Consulta temperatura de motor promedio, mínima y máxima |

### Drive Score

Sistema de puntuación de conducción configurable  
Docs: https://docs.rinho.com.ar/comandos/modulo-trip/drive-score

Firmware: Zero 1.09.20 (Módulo TRIP en Zero IoT desde v1.09.20) · Spider 1.09.17 (Requiere firmware con el módulo TRIP (desde v1.09.17 T8); sin él los comandos TP responden error) · Smart 1.09.17 (Requiere firmware con el módulo TRIP (desde v1.09.17 T8); sin él los comandos TP responden error)

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `QTPSCO` | query | `QTPSCO` | `QTPSCO` | Obtiene el puntaje de conducción (Drive Score) del viaje actual o último finalizado |
| `STPSCOEN` | set | `STPSCOEN,<enable>` | `STPSCOEN,1` | Habilita o deshabilita el cálculo de Drive Score |
| `QTPSCOEN` | query | `QTPSCOEN` | `QTPSCOEN` | Consulta si el Drive Score está habilitado |
| `STPSCOCFG` | set | `STPSCOCFG,<p0>,<p1>,<p2>,<p3>,<p4>,<p5>,<p6>,<p7>,<p8>,<p9>,<p10>,<p11>,<p12>,<p13>,<p14>,<p15>` | `STPSCOCFG,-5,-5,-8,-10,-5,0,0,0,-1,2,-10,-5,-5,-3,100,100` | Configura los 16 pesos y parámetros del cálculo de Drive Score |
| `QTPSCOCFG` | query | `QTPSCOCFG` | `QTPSCOCFG` | Consulta la configuración actual de pesos del Drive Score |

### Estadísticas Globales

Tacógrafo y odómetro de por vida del vehículo  
Docs: https://docs.rinho.com.ar/comandos/modulo-trip/estadisticas-globales

Firmware: Zero 1.09.20 (Módulo TRIP en Zero IoT desde v1.09.20) · Spider 1.09.17 (Requiere firmware con el módulo TRIP (desde v1.09.17 T8); sin él los comandos TP responden error) · Smart 1.09.17 (Requiere firmware con el módulo TRIP (desde v1.09.17 T8); sin él los comandos TP responden error)

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `QTPGTOT` | query | `QTPGTOT` | `QTPGTOT` | Consulta las estadísticas globales completas del vehículo con promedios calculados |
| `QTPGREC` | query | `QTPGREC` | `QTPGREC` | Consulta los récords históricos (máximos y mínimos) de por vida |
| `QTPGSCO` | query | `QTPGSCO` | `QTPGSCO` | Consulta los scores globales acumulados |
| `QTPGGC` | query | `QTPGGC` | `QTPGGC` | Consulta la suma histórica de los 8 contadores GC |
| `QTPGKMS` | query | `QTPGKMS` | `QTPGKMS` | Consulta los kilómetros totales acumulados (getter rápido) |
| `QTPGTRIPS` | query | `QTPGTRIPS` | `QTPGTRIPS` | Consulta el total de viajes completados (getter rápido) |
| `QTPGHRS` | query | `QTPGHRS` | `QTPGHRS` | Consulta horas totales de conducción y en ralentí (getter rápido) |
| `STPGRST` | action | `STPGRST` | `STPGRST` | Resetea todas las estadísticas globales de forma irreversible |

### Histogramas

Distribución de tiempo por rangos de velocidad, RPM, acelerador y turbo  
Docs: https://docs.rinho.com.ar/comandos/modulo-trip/histogramas

Firmware: Zero 1.09.20 (Módulo TRIP en Zero IoT desde v1.09.20) · Spider 1.09.17 (Requiere firmware con el módulo TRIP (desde v1.09.17 T8); sin él los comandos TP responden error) · Smart 1.09.17 (Requiere firmware con el módulo TRIP (desde v1.09.17 T8); sin él los comandos TP responden error)

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `QTPRRPM` | query | `QTPRRPM[,<idx>]` | `QTPRRPM,6` | Consulta el histograma de RPM en formato RAW (segundos), completo o por franja |
| `QTPPRPM` | query | `QTPPRPM[,<idx>]` | `QTPPRPM` | Consulta el histograma de RPM en formato porcentual, completo o por franja |
| `QTPRSPD` | query | `QTPRSPD[,<idx>]` | `QTPRSPD` | Consulta el histograma de velocidad en formato RAW (segundos) |
| `QTPPSPD` | query | `QTPPSPD[,<idx>]` | `QTPPSPD` | Consulta el histograma de velocidad en formato porcentual |
| `QTPRACC` | query | `QTPRACC[,<idx>]` | `QTPRACC` | Consulta el histograma de acelerador en formato RAW (segundos) |
| `QTPPACC` | query | `QTPPACC[,<idx>]` | `QTPPACC` | Consulta el histograma de acelerador en formato porcentual |
| `QTPRTBO` | query | `QTPRTBO[,<idx>]` | `QTPRTBO` | Consulta el histograma de turbo en formato RAW (segundos) |
| `QTPPTBO` | query | `QTPPTBO[,<idx>]` | `QTPPTBO` | Consulta el histograma de turbo en formato porcentual |
| `QSSFRP` | query | `QSSFRPnn` | `QSSFRP05` | Consulta el estado de una franja del histograma de RPM como señal del motor de reglas |
| `QSSFVL` | query | `QSSFVLnn` | `QSSFVL00` | Consulta el estado de una franja del histograma de velocidad como señal del motor de reglas |
| `QSSFAC` | query | `QSSFACnn` | `QSSFAC05` | Consulta el estado de una franja del histograma de acelerador como señal del motor de reglas |
| `QSSFTB` | query | `QSSFTBnn` | `QSSFTB05` | Consulta el estado de una franja del histograma de turbo como señal del motor de reglas |
| `QSSFTM` | query | `QSSFTMnn` | `QSSFTM05` | Consulta el estado de una franja del histograma de temperatura de motor como señal del motor de reglas |

### Reportes

Reportes R0-R10 con datos completos del viaje  
Docs: https://docs.rinho.com.ar/comandos/modulo-trip/reportes

Firmware: Zero 1.09.20 (Módulo TRIP en Zero IoT desde v1.09.20) · Spider 1.09.17 (Requiere firmware con el módulo TRIP (desde v1.09.17 T8); sin él los comandos TP responden error) · Smart 1.09.17 (Requiere firmware con el módulo TRIP (desde v1.09.17 T8); sin él los comandos TP responden error)

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `QTPR0` | query | `QTPR0` | `QTPR0` | Consulta timestamps, GPS y métricas básicas del viaje (parte 1 de 13) |
| `QTPR1` | query | `QTPR1` | `QTPR1` | Consulta velocidad, RPM, temperatura, conductor y contadores GC del viaje (parte 2 de 13) |
| `QTPR10` | query | `QTPR10` | `QTPR10` | Consulta el reporte completo de Drive Score del viaje (parte final del reporte extendido) |

## comandos/motor-de-eventos

### Comando RL

Reglas lógicas del Motor de Eventos  
Docs: https://docs.rinho.com.ar/comandos/motor-de-eventos/comando-rl

Firmware: Zero 1.00.00 · Spider 1.00.00 · Smart 1.00.00

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `QRL` | query | `QRLnn` | `QRL07` | Consulta una regla lógica específica |
| `QRL` | query | `QRL**` | `QRL**` | Consulta todas las reglas lógicas configuradas |
| `SRL` | set | `SRLnnE;TRG=disparador;CND=condicion;ACC={accion}` | `SRL07E;TRG=TD01+;CND=U09U01!&;ACC={GCP01H}` | Configura una regla lógica con disparador, condición y acción |
| `SRL` | set | `SRLnnD` | `SRL07D` | Deshabilita una regla lógica específica y BORRA su disparador/condición/acción (no es un simple toggle) |
| `SRL` | set | `SRL**D` | `SRL**D` | Deshabilita todas las reglas lógicas y BORRA el disparador/condición/acción de cada una (no es un simple toggle) |

## mensajes-de-posicion/confirmacion-de-la-recepcion

### Confirmación de la recepción (ACK)

Protocolo de acknowledge para confirmar recepción de reportes  
Docs: https://docs.rinho.com.ar/mensajes-de-posicion/confirmacion-de-la-recepcion

Firmware: Zero — · Spider — · Smart —

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `ACK` | action | `ACK;#NNNN;ID=XXXX;*CC` | `ACK;#0021;ID=1234;*72` | Confirma la recepción de un reporte (formato completo: número de mensaje + ID + checksum). El firmware no valida el contenido después de "ACK"; el número de mensaje se obtiene del campo #NNNN del sobre del comando. |
| `ACK` | action | `ACK;#NNNN` | `ACK;#0021` | Variante corta (típicamente por SMS), sin ID ni checksum. |
| `SAK` | action | `SAK;#NNNN;ID=XXXX;*CC` | `SAK;#0021;ID=1234;*72` | Alias exacto de ACK: en la tabla del parser, "SAK" apunta al mismo handler que "ACK" (el firmware) y produce el mismo efecto. |

## mensajes-de-posicion/generadores-de-reportes

### Black Box

Comando CXBB para configurar el módulo Black Box: frecuencia de captura, muestras futuras y pasadas en dispositivos Rinho.  
Docs: https://docs.rinho.com.ar/mensajes-de-posicion/generadores-de-reportes/comando-cxbb

Firmware: Zero 1.09.15 · Spider 1.09.15 · Smart 1.09.15

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `SCXBB` | set | `SCXBB,tipo,frecuencia,muestras_futuras,muestras_pasadas` | `SCXBB,CQ,1,20,10` | Configura el módulo Black Box para captura de reportes |
| `QCXBB` | query | `QCXBB` | `QCXBB` | Consulta la configuración actual del Black Box |
| `GBB` | action | `GBBnnP` | `GBB01H` | Dispara la captura del Black Box |

### Comando G

Comando G para generar reportes de diferentes tipos con destino configurable y texto adjunto en dispositivos Rinho.  
Docs: https://docs.rinho.com.ar/mensajes-de-posicion/generadores-de-reportes/comando-g

Firmware: Zero — · Spider — · Smart —

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `G` | generate | `Gaabbc[;@dd..dd][;TXT=ee..ee]` | `GCYBFH;@SM0;TXT=Alta temperatura` | Genera reportes de diferentes tipos, con posibilidad de especificar destino y adosar mensajes de texto |

### Comando KA

Comando KA para configurar keep-alive multi-destino y mantener conexiones activas con intervalo de 0 a 3600 segundos.  
Docs: https://docs.rinho.com.ar/mensajes-de-posicion/generadores-de-reportes/comando-ka

Firmware: Zero — · Spider — · Smart —

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `SKA` | set | `SKA[tiempo],[destino]` | `SKA60,B` | Configura keep alive multi-destino para mantener conexiones activas |
| `QKA` | query | `QKA` | `QKA` | Consulta la configuración del keep alive |

### Comando TX

Comando TX para generar reportes de texto plano sin datos GPS en dispositivos Rinho.  
Docs: https://docs.rinho.com.ar/mensajes-de-posicion/generadores-de-reportes/comando-tx

Firmware: Zero — · Spider — · Smart —

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `GTX` | generate | `GTXbbc[;@dd..dd][;TXT=ee..ee]` | `GTX40H;@SM0;TXT=Bateria baja detectada` | Genera reporte de texto plano sin datos GPS |
| `STX` | set | `STXMENSAJE[;@dd..dd]` | `STXHOLA;@SM1` | Envía texto directo sin datos GPS |

### Comando TY

Comando TY para enviar mensajes de texto plano con soporte de caracteres especiales, hasta 150 caracteres.  
Docs: https://docs.rinho.com.ar/mensajes-de-posicion/generadores-de-reportes/comando-ty

Firmware: Zero — · Spider — · Smart —

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `STY` | set | `STY,mensaje` | `STY,Hola mundo` | Envía mensajes de texto plano con soporte para caracteres especiales |
| `STY` | set | `STYa` | `STYE` | Habilita o deshabilita la captura de texto |

## mensajes-de-posicion/reportes-analogicos

### Reporte AD

Reporte AD: estado de los conversores analógico-digitales con tensiones de entradas analógicas, batería principal y respaldo.  
Docs: https://docs.rinho.com.ar/mensajes-de-posicion/reportes-analogicos/reporte-ad

Firmware: Zero — · Spider — · Smart —

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `GAD` | generate | `GADbbc[;@dd..dd]` | `GAD00H` | Genera el reporte AD (conversores analógico-digitales) hacia el destino indicado. |
| `QAD` | query | `QAD` | `QAD` | Consulta el estado actual de los conversores analógico-digitales. |

### Reporte AE

Reporte AE: estado de conversores analógico-digitales con signo y magnitud de los valores leídos.  
Docs: https://docs.rinho.com.ar/mensajes-de-posicion/reportes-analogicos/reporte-ae

Firmware: Zero — · Spider — · Smart —

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `GAE` | generate | `GAEbbc[;@dd..dd]` | `GAE00H` | Genera el reporte AE (conversores analógico-digitales con signo) hacia el destino indicado. |
| `QAE` | query | `QAE` | `QAE` | Consulta el estado actual de los conversores analógico-digitales con signo. |

## mensajes-de-posicion/reportes-aplicados

### Reporte DF

Reporte DF (Diagnostic Faults): lista completa de códigos de falla activos de la ECU, con la fecha y hora del momento en que apareció la falla.  
Docs: https://docs.rinho.com.ar/mensajes-de-posicion/reportes-aplicados/reporte-df

Firmware: Spider 1.09.27 · Smart 1.09.27

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `GDF` | generate | `GDFbbc` | `GDF00H` | Genera el reporte DF (fallas de la ECU) hacia el destino indicado, con el generador genérico G (ReportList: "DF"→RPT_DTC). |
| `QDF` | query | `QDF` | `QDF` | Consulta las fallas activas y responde de inmediato (sin encolar, que=false) con el mismo formato RDF que el reporte automático. |

### Reporte HT

Reporte HT: genera enlace web clickeable para mostrar posición en Google Maps. Ideal para enviar ubicación por SMS.  
Docs: https://docs.rinho.com.ar/mensajes-de-posicion/reportes-aplicados/reporte-ht

Firmware: Zero — · Spider — · Smart —

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `GHT` | generate | `GHTbbc` | `GHT01H` | Genera el reporte HT. El firmware SIEMPRE arma un link de Google Maps; bb es sólo el número de evento (Mem->IdRpt), no selecciona proveedor de mapa. |
| `QHT` | query | `QHT` | `QHT` | Genera un reporte HT con el link de Google Maps de la posición actual. Cualquier carácter después de "QHT" es ignorado por el firmware. |

### Reporte IB

Reporte IB: datos GPS, estado I/O, identificación iButton y temperatura. Estructura extendida del reporte GP.  
Docs: https://docs.rinho.com.ar/mensajes-de-posicion/reportes-aplicados/reporte-ib

Firmware: Spider — · Smart —

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `GIB` | generate | `GIBbbc[;@dd..dd]` | `GIB00H` | Genera el reporte IB (posición + I/O + iButton + temperatura) hacia el destino indicado. |
| `QIB` | query | `QIB` | `QIB` | Consulta la posición actual en formato IB. |

### Reporte LC

Reporte LC (Locator): localización aproximada por triangulación de celdas GSM sin señal GPS.  
Docs: https://docs.rinho.com.ar/mensajes-de-posicion/reportes-aplicados/reporte-lc

Firmware: Zero — · Spider — · Smart —

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `SLC` | action | `SLCX` | `SLCX` | Solicita una captura de posición por celda. En el firmware actual sólo valida que el carácter siguiente sea 'X' y responde un OK fijo: la línea que dispararía la captura real está comentada, por lo que el comando no inicia ninguna consulta efectiva. |

### Reporte MV

Reporte MV: log de movimiento para accidentología y perfil de conducción. 100 registros de acelerómetro, uno cada 80 ms, de unos 4 s antes a unos 4 s después del disparo.  
Docs: https://docs.rinho.com.ar/mensajes-de-posicion/reportes-aplicados/reporte-mv

Firmware: Zero — · Spider — · Smart —

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `GMV` | generate | `GMVbbc` | `GMV00H` | Dispara el volcado de 100 registros de acelerómetro (unos 4 s antes y unos 4 s después del comando, uno cada 80 ms) y los emite como reportes MV. Desde v1.09.28 manda antes una trama RCAL de 9 campos con la calibración de montaje de ese momento. |

### Reporte SC

Reporte SC: estado del sensor de caudal de combustible Telemetric (Siid) conectado al puerto serie.  
Docs: https://docs.rinho.com.ar/mensajes-de-posicion/reportes-aplicados/reporte-sc

Firmware: Spider — · Smart —

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `GSC` | generate | `GSCbbc` | `GSC00H` | Genera el reporte SC (estado del caudalímetro Telemetric) hacia el destino indicado. |
| `QSC` | query | `QSC` | `QSC` | Consulta el estado actual del caudalímetro. |

## mensajes-de-posicion/reportes-binarios

### Reporte B3

Reporte binario TAU compacto de 47 bytes (posición, estado y celda). Asignable a eventos como los reportes de texto, reduce ~70% el tráfico.  
Docs: https://docs.rinho.com.ar/mensajes-de-posicion/reportes-binarios/reporte-b3

Firmware: Zero 1.09.20 · Spider 1.09.20 · Smart 1.09.20

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `GB3` | generate | `GB3bbc` | `GB300H` | Genera el reporte binario TAU B3 (47 bytes) hacia el destino configurado, usando el mismo generador genérico G que los reportes de texto. |

### Reporte B9

Reporte binario TAU de 89 bytes con acelerómetro XYZ y trail de 10 posiciones GPS. Asignable a eventos como los reportes de texto, reduce ~70% el tráfico.  
Docs: https://docs.rinho.com.ar/mensajes-de-posicion/reportes-binarios/reporte-b9

Firmware: Zero 1.09.20 · Spider 1.09.20 · Smart 1.09.20

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `GB9` | generate | `GB9bbc` | `GB900H` | Genera el reporte binario TAU B9 (89 bytes: posición + acelerómetro + trail) hacia el destino configurado. |

### Reporte BP

Reporte BP: formato binario compacto para transmisión eficiente. Compatible con comunicación satelital Iridium 9602 SBD.  
Docs: https://docs.rinho.com.ar/mensajes-de-posicion/reportes-binarios/reporte-bp

Firmware: Spider — · Smart —

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `GBP` | generate | `GBPbbc[;@dd..dd]` | `GBP00H` | Genera el reporte BP. En el firmware actual el reporte SIEMPRE se envía por el puerto serie local, sin importar el destino @dd solicitado, y nunca pasa por la cola de reintentos/ACK: AddRpt() lo intercepta y llama directamente a rptBpToPort(), retornando sin encolar. |

## mensajes-de-posicion/reportes-de-usuario

### Comando DA

Acciones dinámicas — renderiza una plantilla UC inline y la ejecuta o la rutea a un destino, sin gastar slots UC  
Docs: https://docs.rinho.com.ar/mensajes-de-posicion/reportes-de-usuario/comando-da

Firmware: Zero 1.09.25 · Spider 1.09.25 · Smart 1.09.25

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `SDA` | set | `SDA plantilla[;@DEST[!]]` | `SDA $SSSXP0\| "011";@TRM` | Renderiza la plantilla UC y ejecuta el comando resultante |
| `QDA` | query | `QDA plantilla[;@DEST[!]]` | `QDA QIO` | Renderiza la plantilla UC sin ejecutarla — devuelve o rutea el texto |

### Comando EVAL

Comando EVAL: motor de evaluación de expresiones matemáticas, lógicas y de estado del sistema con variables y formato printf.  
Docs: https://docs.rinho.com.ar/mensajes-de-posicion/reportes-de-usuario/comando-eval

Firmware: Zero — · Spider — · Smart —

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `EVAL` | query | `EVAL expresión[:formato[:valor_error]]` | `EVAL 10 + 5` | Evalúa una expresión matemática, lógica o de estado del sistema y devuelve el resultado formateado |

### Comando UC

Configuración de reportes de usuario personalizados concatenando comandos, texto y campos calculados  
Docs: https://docs.rinho.com.ar/mensajes-de-posicion/reportes-de-usuario/comando-uc

Firmware: Zero 1.00.00 · Spider 1.00.00 · Smart 1.00.00

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `SUC` | set | `SUCnn <estructura>` | `SUC00 $POS\| QPQ,5,64` | Define o reemplaza la estructura del reporte de usuario UCnn (campos, texto y comandos a concatenar) |
| `SUC` | set | `SUCnn*` | `SUC00*` | Elimina la configuración del reporte de usuario UCnn |
| `QUC` | query | `QUCnn` | `QUC00` | Lee la estructura configurada del reporte de usuario UCnn |

### Reporte Ux

Consulta, ejecución y generación de reportes de usuario personalizados configurados con UC  
Docs: https://docs.rinho.com.ar/mensajes-de-posicion/reportes-de-usuario/comando-ux

Firmware: Zero 1.00.00 · Spider 1.00.00 · Smart 1.00.00

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `QU` | query | `QUn` | `QU0` | Lee y muestra el contenido renderizado del reporte de usuario UCn (ejecuta los comandos internos y concatena resultados) |
| `SU` | action | `SUn` | `SU1` | Ejecuta el reporte UCn como si fuera un comando del equipo (procesa la estructura y corre los comandos embebidos, sin enviarlo a servidor) |
| `GU` | generate | `GUnbbc[;@dst]` | `GU0XXH` | Genera el reporte de usuario UCn y lo encola para envío a un destino |

## mensajes-de-posicion/reportes-extendidos

### Reporte BQ

Reporte BQ: extendido filtrado con datos de batería interna del dispositivo Rinho.  
Docs: https://docs.rinho.com.ar/mensajes-de-posicion/reportes-extendidos/reporte-bq

Firmware: Zero — · Spider — · Smart —

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `GBQ` | generate | `GBQbbc[;@dd..dd]` | `GBQ00H` | Genera el reporte BQ hacia el destino indicado |
| `QBQ` | query | `QBQ` | `QBQ` | Consulta la posicion actual en formato BQ |

### Reporte BR

Reporte BR: extendido filtrado con batería interna y temperatura del sensor OW0.  
Docs: https://docs.rinho.com.ar/mensajes-de-posicion/reportes-extendidos/reporte-br

Firmware: Zero — · Spider — · Smart —

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `GBR` | generate | `GBRbbc[;@dd..dd]` | `GBR00H` | Genera el reporte BR hacia el destino indicado |
| `QBR` | query | `QBR` | `QBR` | Consulta la posicion actual en formato BR |

### Reporte BV

Reporte BV: extendido filtrado con batería interna y temperaturas de dos sensores (OW0 y OW1).  
Docs: https://docs.rinho.com.ar/mensajes-de-posicion/reportes-extendidos/reporte-bv

Firmware: Zero — · Spider — · Smart —

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `GBV` | generate | `GBVbbc[;@dd..dd]` | `GBV00H` | Genera el reporte BV hacia el destino indicado |
| `QBV` | query | `QBV` | `QBV` | Consulta la posicion actual en formato BV |

### Reporte CP

Reporte CP: extendido con datos GPS sin filtrar (raw), incluyendo PDOP alto y pocos satélites.  
Docs: https://docs.rinho.com.ar/mensajes-de-posicion/reportes-extendidos/reporte-cp

Firmware: Zero — · Spider — · Smart —

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `GCP` | generate | `GCPbbc[;@dd..dd]` | `GCP00H` | Genera el reporte CP hacia el destino indicado |
| `QCP` | query | `QCP` | `QCP` | Consulta la posicion actual en formato CP |

### Reporte CQ

Reporte CQ: compacto extendido filtrado con odómetro, tensión batería, estado GPS y celular. Uno de los más utilizados.  
Docs: https://docs.rinho.com.ar/mensajes-de-posicion/reportes-extendidos/reporte-cq

Firmware: Zero — · Spider — · Smart —

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `GCQ` | generate | `GCQbbc[;@dd..dd]` | `GCQ00H` | Genera el reporte CQ hacia el destino indicado |
| `QCQ` | query | `QCQ` | `QCQ` | Consulta la posicion actual en formato CQ |

### Reporte CR

Reporte CR: extendido filtrado con temperatura del sensor OW0.  
Docs: https://docs.rinho.com.ar/mensajes-de-posicion/reportes-extendidos/reporte-cr

Firmware: Spider — · Smart —

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `GCR` | generate | `GCRbbc[;@dd..dd]` | `GCR00H` | Genera el reporte CR hacia el destino indicado |
| `QCR` | query | `QCR` | `QCR` | Consulta la posicion actual en formato CR |

### Reporte CT

Reporte CT: extendido filtrado con identificación iButton leído.  
Docs: https://docs.rinho.com.ar/mensajes-de-posicion/reportes-extendidos/reporte-ct

Firmware: Spider — · Smart —

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `GCT` | generate | `GCTbbc[;@dd..dd]` | `GCT00H` | Genera el reporte CT hacia el destino indicado |
| `QCT` | query | `QCT` | `QCT` | Consulta la posicion actual en formato CT |

### Reporte CU

Reporte CU: extendido filtrado con identificación de chofer activo del listado ICL para control de acceso.  
Docs: https://docs.rinho.com.ar/mensajes-de-posicion/reportes-extendidos/reporte-cu

Firmware: Spider — · Smart —

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `GCU` | generate | `GCUbbc[;@dd..dd]` | `GCU00H` | Genera el reporte CU hacia el destino indicado |
| `QCU` | query | `QCU` | `QCU` | Consulta la posicion actual en formato CU |

### Reporte CV

Reporte CV: extendido filtrado con dos sensores de temperatura (OW0/OW1) para cadena de frío y transporte refrigerado.  
Docs: https://docs.rinho.com.ar/mensajes-de-posicion/reportes-extendidos/reporte-cv

Firmware: Spider — · Smart —

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `GCV` | generate | `GCVbbc[;@dd..dd]` | `GCV00H` | Genera el reporte CV hacia el destino indicado |
| `QCV` | query | `QCV` | `QCV` | Consulta la posicion actual en formato CV |

### Reporte CW

Reporte CW: compacto optimizado con odómetro en decimal y campos reorganizados para facilitar parsing e integración.  
Docs: https://docs.rinho.com.ar/mensajes-de-posicion/reportes-extendidos/reporte-cw

Firmware: Spider — · Smart —

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `GCW` | generate | `GCWbbc[;@dd..dd]` | `GCW00H` | Genera el reporte CW hacia el destino indicado |
| `QCW` | query | `QCW` | `QCW` | Consulta la posicion actual en formato CW |

### Reporte EQ

Reporte EQ: extendido con datos CAN OBD-II (CAN01) para vehículos livianos. Compatible con Wialon y RedGPS.  
Docs: https://docs.rinho.com.ar/mensajes-de-posicion/reportes-extendidos/reporte-eq

Firmware: Spider — · Smart —

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `GEQ` | generate | `GEQbbc[;@dd..dd]` | `GEQ00H` | Genera el reporte EQ hacia el destino indicado |
| `QEQ` | query | `QEQ` | `QEQ` | Consulta la posicion actual en formato EQ |

### Reporte ER

Reporte ER: extendido con datos CAN J1939 (CAN00) para vehículos pesados. Compatible con Wialon y RedGPS.  
Docs: https://docs.rinho.com.ar/mensajes-de-posicion/reportes-extendidos/reporte-er

Firmware: Spider — · Smart —

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `GER` | generate | `GERbbc[;@dd..dd]` | `GER00H` | Genera el reporte ER hacia el destino indicado |
| `QER` | query | `QER` | `QER` | Consulta la posicion actual en formato ER |

### Reporte HQ

Reporte HQ: extendido filtrado con horómetro y batería interna.  
Docs: https://docs.rinho.com.ar/mensajes-de-posicion/reportes-extendidos/reporte-hq

Firmware: Zero — · Spider — · Smart —

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `GHQ` | generate | `GHQbbc[;@dd..dd]` | `GHQ00H` | Genera el reporte HQ hacia el destino indicado |
| `QHQ` | query | `QHQ` | `QHQ` | Consulta la posicion actual en formato HQ |

### Reporte HR

Reporte HR: extendido filtrado con horómetro, batería interna y temperatura del sensor OW0.  
Docs: https://docs.rinho.com.ar/mensajes-de-posicion/reportes-extendidos/reporte-hr

Firmware: Spider — · Smart —

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `GHR` | generate | `GHRbbc[;@dd..dd]` | `GHR00H` | Genera el reporte HR hacia el destino indicado |
| `QHR` | query | `QHR` | `QHR` | Consulta la posicion actual en formato HR |

### Reporte HV

Reporte HV: extendido filtrado con horómetro, batería interna y dos sensores de temperatura (OW0/OW1).  
Docs: https://docs.rinho.com.ar/mensajes-de-posicion/reportes-extendidos/reporte-hv

Firmware: Spider — · Smart —

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `GHV` | generate | `GHVbbc[;@dd..dd]` | `GHV00H` | Genera el reporte HV hacia el destino indicado |
| `QHV` | query | `QHV` | `QHV` | Consulta la posicion actual en formato HV |

## mensajes-de-posicion/reportes-standards

### Reporte CY

Reporte CY: posición estándar con fecha, hora, coordenadas, velocidad, rumbo, altitud, estado GPS e I/O.  
Docs: https://docs.rinho.com.ar/mensajes-de-posicion/reportes-standards/reporte-cy

Firmware: Zero — · Spider — · Smart —

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `GCY` | generate | `GCYbbc[;@dd..dd][;TXT=ee..ee]` | `GCY33H;TXT=Exceso velocidad` | Genera el reporte CY (posición estándar) hacia el destino indicado, con texto libre opcional. |
| `QCY` | query | `QCY` | `QCY` | Consulta la posición actual en formato CY. |

### Reporte GP

Reporte GP (General Position): reporte base de posición con fecha, hora, coordenadas, velocidad, rumbo e I/O.  
Docs: https://docs.rinho.com.ar/mensajes-de-posicion/reportes-standards/reporte-gp

Firmware: Zero — · Spider — · Smart —

| Command | Type | Syntax | Example | What it does |
|---|---|---|---|---|
| `GGP` | generate | `GGPbbc[;@dd..dd]` | `GGP00H` | Genera el reporte GP (posición general) hacia el destino indicado. |
| `QGP` | query | `QGP` | `QGP` | Consulta la posición actual en formato GP. |
