# Rinho configuration recipes

Generated from the recipe library of the Rinho configurator. Each recipe is a tested block
of commands for one feature. Names and descriptions are in Spanish, as customers ask for them.

How to use a recipe:
1. Pick the recipe whose description matches the request. Check `requires` against the device model.
2. Start from **Resolved example** (defaults applied). Change parameter values as needed, keeping
   each parameter's digit format (e.g. `5d` = 5 digits zero-padded: 60 → `00060`).
3. If the customer's device already uses some of those slots (rules RL, timers TD, flags U, UV, VL…),
   renumber using **Template**: every `@type:name` placeholder is one resource; give it a free slot and
   replace it everywhere it appears (definitions and references inside TRG/CND/ACC).
   Rule slots: `00`-`99` and `A0`-`FF` only. In Basic mode, custom rules go at RL50 or higher.
4. **Example validated on** lists the models where the resolved example passes the validator as is.
   On other models, inputs/outputs may not exist (Zero has 2 inputs / 1 output, Spider 3/3, Smart 7/3).
5. Validate the final block with `scripts/rinho.mjs validate` before handing it over.

## Index

- [Alarma de batería con antirrebote (baja, OK, desconectada)](#alarma-bateria-antirrebote) — `alarma-bateria-antirrebote`
- [Alcoholímetro: buzzer de habilitación al encender ignición](#alcoholimetro-buzzer-habilitacion) — `alcoholimetro-buzzer-habilitacion`
- [Bajo consumo al quitar ignición](#bajo-consumo-ign) — `bajo-consumo-ign`
- [Botón de pánico (pulsador mantenido N décimas)](#boton-panico-pulsador-mantenido) — `boton-panico-pulsador-mantenido`
- [Clave del AP WiFi que cambia sola todos los dias](#clave-wifi-diaria-por-fecha) — `clave-wifi-diaria-por-fecha`
- [Corte remoto seguro: sólo detenido y autorizado por flag](#corte-condicionado-velocidad) — `corte-condicionado-velocidad`
- [Corte de motor con preaviso de buzzer](#corte-motor-preaviso-buzzer) — `corte-motor-preaviso-buzzer`
- [Flag 'en alguna zona' para usar como condición](#dentro-de-alguna-zona-flag) — `dentro-de-alguna-zona-flag`
- [Evento por entrada digital (puerta, tapa, desenganche)](#evento-entrada-digital) — `evento-entrada-digital`
- [Evento al entrar y salir de una zona del catálogo masivo (ZNA/ZNB)](#evento-zona-masiva) — `evento-zona-masiva`
- [Exceso de velocidad con buzzer (un solo límite)](#exceso-velocidad-buzzer) — `exceso-velocidad-buzzer`
- [Exceso de velocidad sostenido N segundos](#exceso-velocidad-sostenido) — `exceso-velocidad-sostenido`
- [Frenada y aceleración bruscas confirmadas por timer](#frenada-acelerada-confirmada) — `frenada-acelerada-confirmada`
- [Horómetro por ignición](#horometro-ignicion) — `horometro-ignicion`
- [Ignición por cable O por movimiento (velocidad GPS)](#ignicion-cable-o-movimiento) — `ignicion-cable-o-movimiento`
- [Jamming validado con guarda y corte gradual](#jamming-validado-corte) — `jamming-validado-corte`
- [Parking automático con posición fija](#parking-auto-posicion-fija) — `parking-auto-posicion-fija`
- [Posición fija clavada en cada arranque (activo inmóvil)](#posicion-fija-al-arrancar) — `posicion-fija-al-arrancar`
- [Puerta abierta demasiado tiempo (alarma con sirena)](#puerta-abierta-demasiado-tiempo) — `puerta-abierta-demasiado-tiempo`
- [Ralentí (motor encendido y detenido más de N minutos)](#ralenti) — `ralenti`
- [Reinicio programado del equipo cada N horas](#reinicio-programado) — `reinicio-programado`
- [Señal y reporte de corte gradual (GC) en curso](#reporte-corte-gradual-gc) — `reporte-corte-gradual-gc`
- [Reporte en paralelo a una segunda plataforma (@BACK)](#reporte-espejo-segunda-plataforma) — `reporte-espejo-segunda-plataforma`
- [Reporte periódico del nivel de combustible de varillas BLE (sensor BS, Escort TD)](#reporte-nivel-varillas-ble) — `reporte-nivel-varillas-ble`
- [Lector RFID por serie: reportar solo cuando cambia la tarjeta](#rfid-serial-solo-cambios) — `rfid-serial-solo-cambios`
- [Alerta de robo de combustible por CAN/OBD (nivel ECU)](#robo-combustible-can) — `robo-combustible-can`
- [Alerta de robo de combustible por varilla BLE (sensor BS)](#robo-combustible-varilla-ble) — `robo-combustible-varilla-ble`
- [Prender y apagar una salida mandando una palabra por SMS](#salida-por-sms-palabra-clave) — `salida-por-sms-palabra-clave`
- [Score de conducción por viaje (frenadas, aceleradas, excesos, over-rev)](#score-conduccion-viaje) — `score-conduccion-viaje`
- [Sincronizar hora por red](#sincronizar-hora-red) — `sincronizar-hora-red`
- [Límites de velocidad por zona con buzzer](#velocidad-por-zona-buzzer) — `velocidad-por-zona-buzzer`

## Alarma de batería con antirrebote (baja, OK, desconectada)

<a id="alarma-bateria-antirrebote"></a>

**id:** `alarma-bateria-antirrebote`

Eventos de batería baja, batería OK y batería desconectada sobre la tensión de alimentación (fuente 06), sin reportar el rebote de la tensión al arrancar el motor. El antirrebote lo hace el propio rango analógico: cada AR tiene un tiempo de establecimiento (décimas de segundo) y su señal cambia recién cuando la tensión se sostiene del otro lado ese tiempo; un cambio que vuelve antes se descarta. Con tres rangos que no se solapan, entrar a cada uno es el evento. Tres AR + tres reglas, sin timer.

- **Requires hardware:** none (`?` = optional)
- **Uses modules:** ar
- **Footprint:** 3 rules, 3 ar
- **Example validated on:** zero, spider, smart
- **Modes:** advanced

| Parameter | Default | Format | Meaning |
|---|---|---|---|
| `descMax` | `500` | `4d` | Tope del rango 'desconectada' en centésimas de volt (500 = 5,00 V). El rango va de 0 a este valor. |
| `bajaMax` | `1000` | `4d` | Tope del rango 'baja' en centésimas de volt (1000 = 10,00 V). El rango va de descMax a este valor. |
| `okMin` | `1100` | `4d` | Piso del rango 'OK' en centésimas de volt (1100 = 11,00 V). Entre bajaMax y okMin queda una banda muerta que evita ida y vuelta entre baja y OK. |
| `debDecimas` | `50` | `3d` | Tiempo de establecimiento de cada rango, en décimas de segundo (50 = 5 s): la tensión tiene que sostenerse ese tiempo para que el rango cambie. |
| `evBaja` | `03` |  | Evento batería baja (dos dígitos hexa, 00-FF). La mayoría de las plataformas los convierte a decimal (00-FF → 0-255: 31 → 49); Wialon en tramas ER los muestra tal cual. |
| `evOk` | `14` |  | Evento batería OK. La mayoría de las plataformas los convierte a decimal (00-FF → 0-255: 31 → 49); Wialon en tramas ER los muestra tal cual. |
| `evDesc` | `04` |  | Evento batería desconectada. La mayoría de las plataformas los convierte a decimal (00-FF → 0-255: 31 → 49); Wialon en tramas ER los muestra tal cual. |

**Resolved example**

```
>SAR0306+0000+0500050<
>SAR0406+0500+1000050<
>SAR0506+1100+9999050<
>SRL08E;TRG=AR04+;ACC={GER03H;@LOG}<
>SRL09E;TRG=AR05+;ACC={GER14H;@LOG}<
>SRL10E;TRG=AR03+;ACC={GER04H;@LOG}<
```

**Template** (placeholders `@type:name`, parameters `{name}`)

```
>SAR@ar:desc06+0000+{descMax}{debDecimas}<
>SAR@ar:baja06+{descMax}+{bajaMax}{debDecimas}<
>SAR@ar:ok06+{okMin}+9999{debDecimas}<
>SRL@rl:bajaE;TRG=@ar:baja+;ACC={GER{evBaja}H;@LOG}<
>SRL@rl:okE;TRG=@ar:ok+;ACC={GER{evOk}H;@LOG}<
>SRL@rl:descE;TRG=@ar:desc+;ACC={GER{evDesc}H;@LOG}<
```

**Notes**

- Los rangos van DISJUNTOS a propósito (desconectada 0-5 V, baja 5-10 V, OK ≥ 11 V): con rangos anidados como los de fábrica (baja = 0-10 V contiene a desconectada) una caída a 3 V dispara 'baja' y 'desconectada' en dos pasadas distintas del módulo, que evalúa un índice por vez. Con rangos disjuntos cada tensión está en un solo rango y entra un solo evento. La banda 10-11 V sin rango es la histéresis entre baja y OK.
- En modo BÁSICO no hace falta: el módulo Batería del configurador usa AR00-02 para esto. La receta es para modo avanzado; si la config viene del básico, esos tres AR ya están ocupados y la receta toma los siguientes libres.

## Alcoholímetro: buzzer de habilitación al encender ignición

<a id="alcoholimetro-buzzer-habilitacion"></a>

**id:** `alcoholimetro-buzzer-habilitacion`

Al encender ignición (IN07) el buzzer empieza a pitar (100 ms cada 5 s) pidiendo el soplo. Cuando llega una lectura nueva del alcoholímetro (capturada con CXXB), un UV tipo F la compara contra el máximo: si está por debajo, apaga el buzzer y emite el evento OK; si está por encima (o igual), emite el evento de falla y deja sonando el buzzer. La lectura se evalúa 1 s después del flanco de captura (XB+) mediante un TD one-shot, para que el UV tenga tiempo de refrescarse (UV_Service corre cada 45 ms y es otra task que el motor de reglas).

- **Requires hardware:** alcoholimetro con salida serie ASCII (envia 'alcohol=<valor>\r\n'), buzzer en salida XP, ignicion cableada a IN07 (`?` = optional)
- **Uses modules:** cxxb, td
- **Footprint:** 5 rules, 1 timers, 1 uvs, 1 cxxb
- **Example validated on:** spider, smart
- **Modes:** advanced

| Parameter | Default | Format | Meaning |
|---|---|---|---|
| `umbral` | `0.500` |  | Lectura maxima permitida, en la unidad del alcoholimetro (ej. g/L). Es el campo max del UV tipo F: se considera OK si 0.000 <= lectura < umbral. |
| `eventoOk` | `41` |  | Numero de evento (2 digitos hex) para 'soplo por debajo del maximo'. GU0 = Wialon IPS. |
| `eventoFalla` | `42` |  | Numero de evento (2 digitos hex) para 'soplo por encima del maximo'. La mayoría de las plataformas los convierte a decimal (00-FF → 0-255: 31 → 49); Wialon lo hace en CQ/EQ y en BQ/ER/HQ/HV/CV los muestra tal cual. |
| `ignicion` | `IN07` |  | Señal de ignición. IN07 = entrada digital (debounce ~100 ms). IGN tiene debounce default de 10 s, por eso se prefiere IN07 salvo que configuren los tiempos de IGN. |
| `buzzerXp` | `01` |  | Indice (2 digitos) de la salida XP del buzzer. |
| `delaySeg` | `00001` |  | Delay del TD (5 digitos, segundos) entre el flanco de captura y la evaluacion. 1 s es mas que suficiente para que el UV se refresque (45 ms). |
| `buzzerTon` | `0001` |  | Tiempo ON del pitido, en unidades de 100 ms (0001 = 100 ms). |
| `buzzerToff` | `0049` |  | Tiempo OFF entre pitidos, en unidades de 100 ms (0049 = 4.9 s -> periodo total 5 s). |
| `fuente` | `S` |  | Fuente del CXXB. 'S' = puerto serie. |
| `puerto` | `0` |  | idxSource del CXXB: 0 = serial de programacion, 1 = externo, 2 = interno. |
| `rxTimeout` | `050` |  | Timeout de la captura (campo ttt del SCXXB). OJO: esta en ms (no centesimas), maximo real 254. |
| `header` | `alcohol=` |  | Caracteres de inicio de captura del CXXB. |
| `footer` | `\r\n` |  | Caracteres de fin de captura del CXXB (\r\n = CR+LF; pasarelaDecodeString los decodifica). |

**Resolved example**

```
>SCXXB00S0000050,alcohol=,\r\n,A,0<
>SUV00FQXB00,5,6,0.000,0.500<
>SCXXP01000000010049<
>SRL61E;TRG=IN07+;ACC={SSSXP0111}<
>SRL62E;TRG=XB00+;ACC={STD10R00001}<
>SRL63E;TRG=TD10+;CND=UV00;ACC={STD10C}{SSSXP0100}{GU041H}<
>SRL64E;TRG=TD10+;CND=UV00!;ACC={STD10C}{GU042H}<
>SRL65E;TRG=IN07-;ACC={SSSXP0100}{STD10C}<
```

**Template** (placeholders `@type:name`, parameters `{name}`)

```
>SCXXB@xb:alcohol{fuente}{puerto}000{rxTimeout},{header},{footer},A,0<
>FQXB@xb:alcohol,5,6,0.000,{umbral}<
>SRL@rl:encenderE;TRG={ignicion}+;ACC={SSSXP{buzzerXp}11}<
>SRL@rl:armarE;TRG=XB@xb:alcohol+;ACC={STD@td:evaluarR{delaySeg}}<
>SRL@rl:okE;TRG=TD@td:evaluar+;CND=UV@uv:alcoholOk;ACC={STD@td:evaluarC}{SSSXP{buzzerXp}00}{GU0{eventoOk}H}<
>SRL@rl:fallaE;TRG=TD@td:evaluar+;CND=UV@uv:alcoholOk!;ACC={STD@td:evaluarC}{GU0{eventoFalla}H}<
>SRL@rl:apagarE;TRG={ignicion}-;ACC={SSSXP{buzzerXp}00}{STD@td:evaluarC}<
```

**Notes**

- NEGACION: '!' va DESPUES del termino (UV00! = NOT UV00). '&!' es el operador NAND (NOT AND), NO 'AND con termino negado': 'XB00&!UV00' es NAND y queda activo hasta con XB00=0 (sin soplo), por eso se usa CND=UV00! en su lugar.
- GOTCHA BUFFER VACIO: el buffer XB vacio devuelve 'RXB00' y atof('RXB00') = 0.0, que cae del lado 'por debajo del maximo' (0.0 < umbral). Por eso el UV ya esta en 1 en reposo y NO se puede usar 'UV00+' como detector de soplo: el flanco no salta. El detector de 'soplo nuevo' es XB00+ (pulso de captura), no el UV.
- BUZZER BLINK: SCXXP{idx:2d}{tactive:4d}{ton:4d}{toff:4d} con tactive=0000 (blink estacionario, suena mientras value=1), ton=0001 (100 ms ON) y toff=0049 (4.9 s OFF) = pitido cada 5 s. Los tiempos de ton/toff estan en unidades de 100 ms (XP_Mod_Service_Index multiplica por 10 ticks de 10 ms).
- SLOTS RL: 00-99 decimal puro y A0-FF hex con offset; el hueco con letra 0A..9F NO existe (el firmware responde CMDERR sin cargar la regla). El binding del ejemplo usa 61-65 (decimal), valido en modo advanced.
- El UV tipo F no necesita la coma final (esa es solo para la rama con delimitador \XXc). La comparacion es min <= valor < max, o sea que un soplo exactamente en el umbral (ej. 0.500) NO se considera OK (cae en falla). Si se quiere incluir el umbral como OK, bajar el max una centesima o subir el umbral.
- Si el alcoholimetro reporta '0.000' para un soplo limpio, ese valor es < umbral y se considera OK (correcto). Si en reposo emitiera '0.000' continuamente, el XB00+ dispararia con cada trama y apagaria el buzzer solo — pero esta receta asume que el sensor SOLO envia cuando hay soplo (confirmado con el caso real).

## Bajo consumo al quitar ignición

<a id="bajo-consumo-ign"></a>

**id:** `bajo-consumo-ign`

Al quitar IGN da una guarda para vaciar la cola viva (BU) y el log de descarga diferida (CXDL) antes de entrar a ULP (SKO++). Al despertar abre una ventana para reportar y reintentar, y si no, el timer la cierra. TD compartido entre la guarda y la ventana.

- **Requires hardware:** acc (`?` = optional)
- **Uses modules:** cxdl?
- **Footprint:** 6 rules, 1 timers, 1 uvs
- **Example validated on:** zero, spider, smart
- **Modes:** advanced

| Parameter | Default | Format | Meaning |
|---|---|---|---|
| `guardaSeg` | `60` | `5d` | Guarda tras IGN off para vaciar colas (seg) |
| `ventanaSeg` | `300` | `5d` | Ventana despierto tras salir de ULP (seg) |
| `ulpMin` | `15` | `m+3d` | Duración del ULP (min). SKO++ = m<3d>+wakeflags |
| `ulpWake` | `110000` |  | Flags SKO++: acc,io,cel,gps,int,ext. 110000 = wake por ACC+IO |

**Resolved example**

```
>SUV00UQDL,\201,0,5,00001,99999<
>SRLD0E;TRG=IGN-;ACC={STD10R00060}<
>SRLD1E;TRG=IGN+;ACC={STD10C}<
>SRLD2E;TRG=BUUV00|-<
>SRLD3E;TRG=TD10RLD2|+;CND=IGN!;ACC={STD10C}{SKO++m015110000}<
>SRLD4E;TRG=KO04+;ACC={STD10R00300}<
>SRLD5E;TRG=CQ00+;ACC={GU078H}<
```

**Template** (placeholders `@type:name`, parameters `{name}`)

```
>UQDL,\201,0,5,00001,99999<
>SRL@rl:guardaE;TRG=IGN-;ACC={STD@td:sharedR{guardaSeg}}<
>SRL@rl:cancelGuardaE;TRG=IGN+;ACC={STD@td:sharedC}<
>SRL@rl:pulsoE;TRG=BU@uv:logPend|-<
>SRL@rl:dormirE;TRG=TD@td:shared@rl:pulso|+;CND=IGN!;ACC={STD@td:sharedC}{SKO++{ulpMin}{ulpWake}}<
>SRL@rl:ventanaE;TRG=KO04+;ACC={STD@td:sharedR{ventanaSeg}}<
>SRL@rl:reporteE;TRG=CQ00+;ACC={GU078H}<
```

**Notes**

- GPS arranca en frío cada wake (flag gps=0); medí el TTFF real para que CQ00 llegue dentro de la ventana. Igual el TD+ es el techo duro que cierra la ventana.
- Variante sin CXDL: si no usás descarga diferida, eliminá @uv:logPend y poné @rl:pulso con TRG=BU- (solo cola viva).
- El slot conversor (@rl:pulso) debe quedar con índice MENOR que el consumidor (@rl:dormir) para que el pulso se vea en el mismo barrido del motor.

## Botón de pánico (pulsador mantenido N décimas)

<a id="boton-panico-pulsador-mantenido"></a>

**id:** `boton-panico-pulsador-mantenido`

Reporta un evento de pánico cuando se aprieta un pulsador conectado a una entrada digital, exigiendo que se lo mantenga apretado un tiempo mínimo (default 1 segundo) para que un roce no dispare la alarma. El tiempo mínimo lo hace el antirrebote nativo de la entrada (SIT), no una regla: la señal IN sólo cambia cuando el nuevo estado se sostiene ese tiempo. Un antirrebote + una regla. Sirve para cualquier pulsador que deba mantenerse apretado (pánico, pedido de auxilio, llamada).

- **Requires hardware:** entrada-digital (`?` = optional)
- **Uses modules:** none
- **Footprint:** 1 rules
- **Example validated on:** zero, spider, smart
- **Modes:** basic, advanced

| Parameter | Default | Format | Meaning |
|---|---|---|---|
| `entrada` | `00` |  | Índice (2 dígitos) de la entrada digital donde va el pulsador. |
| `flanco` | `-` |  | Flanco que corresponde a apretar: '-' si el pulsador lleva la entrada a masa (lo habitual: TRG=IN00-), '+' si la lleva a positivo. |
| `tonDecimas` | `10` | `4d` | Décimas de segundo que la entrada tiene que sostener el estado 1 para darlo por válido (10 = 1 s). Con flanco '+', es el tiempo mínimo apretado. |
| `toffDecimas` | `10` | `4d` | Décimas de segundo que la entrada tiene que sostener el estado 0 para darlo por válido (10 = 1 s). Con flanco '-', es el tiempo mínimo apretado. |
| `evento` | `10` |  | Evento de pánico (dos dígitos hexa, 00-FF). La mayoría de las plataformas los convierte a decimal (00-FF → 0-255: 31 → 49); Wialon en tramas ER los muestra tal cual. |

**Resolved example**

```
>SIT0000100010<
>SRL13E;TRG=IN00-;ACC={GER10H;@LOG}<
```

**Template** (placeholders `@type:name`, parameters `{name}`)

```
>SIT{entrada}{tonDecimas}{toffDecimas}<
>SRL@rl:panicoE;TRG=IN{entrada}{flanco};ACC={GER{evento}H;@LOG}<
```

**Notes**

- Cuál de los dos tiempos es el 'mantener apretado' depende del cableado: pulsador a masa (flanco '-') → el estado apretado es 0 → manda toff; pulsador a positivo (flanco '+') → manda ton. El otro tiempo es el mínimo para dar por soltado.
- El evento sale con GER (reporte extendido). Si la plataforma espera otro tipo de reporte, cambiar GER por GCQ o GBQ en la regla; el número de evento es el mismo.
- Sin @LOG el reporte va a la cola viva y se pierde si no hay cobertura en ese momento; con @LOG queda en el log diferido en flash. Para un pánico conviene @LOG.
- Para una entrada que se abre y se cierra (puerta, tapa, desenganche) con evento en cada sentido, usar evento-entrada-digital. Para alarmar si queda abierta mucho tiempo, puerta-abierta-demasiado-tiempo.

## Clave del AP WiFi que cambia sola todos los dias

<a id="clave-wifi-diaria-por-fecha"></a>

**id:** `clave-wifi-diaria-por-fecha`

El equipo se cambia solo la clave de su Access Point una vez por dia, componiendola como <prefijo secreto> + <fecha ddmmyy> tomada del RTC. Incluye el automatismo de puesta en hora: si el equipo no tiene hora valida, reintenta por WiFi o celular hasta conseguirla (mismo patron que sincronizar-hora-red.json, reutilizando el MISMO UV). El SSID y la IP del AP NO se tocan: se leen con QAPX y se reescriben igual. Verificado contra firmware 1.09.24 (mecanismo SU + UC; en 1.09.25+ se simplifica con SDA).

- **Requires hardware:** none (`?` = optional)
- **Uses modules:** wifi, celular?
- **Footprint:** 6 rules, 1 timers, 1 uvs, 2 ucs, 4 trs
- **Example validated on:** zero, spider, smart
- **Modes:** advanced

| Parameter | Default | Format | Meaning |
|---|---|---|---|
| `prefijo` | `RIN` | `str` | Parte secreta de la clave. ES UN SECRETO POR CLIENTE: no dejarlo escrito en el repo ni en la receta, va en la config del cliente (Configuraciones/). La seguridad vive ACA, no en la fecha, que es publica. Minimo 2 caracteres: prefijo + 6 de fecha debe dar >= 8 (WPA2). |
| `horaSync` | `000200` | `hhmmss` | Hora del SRTC AUTO. Temprano, porque puede bloquear el motor de reglas hasta ~1 min. |
| `horaSet` | `000500` | `hhmmss` | Hora en que se compone y aplica la clave nueva. |
| `horaOff` | `000600` | `hhmmss` | Hora en que se baja el WiFi (para que el AP tome la clave nueva). |
| `horaOn` | `000700` | `hhmmss` | Hora en que se vuelve a levantar el WiFi. |
| `autodestiny` | `0` | `int` | 3er campo de SCXWIF. LEER >QCXWIF< del equipo destino y respetar lo que ya tiene. |
| `retrySeg` | `60` | `5d` | Intervalo de reintento de puesta en hora al arrancar sin reloj (seg). Los 5 digitos de STD son segundos: el firmware hace atol(buff+3)*10 y el timer corre a 100 ms. |

**Resolved example**

```
>SUV00UQCP,5,6,10100,320000<
>SUC05 $SAPX| QAPX,\2C0,4,30| $,RIN| QCP,5,6|QAPX,\2C2,0,15|QAPX,\2C3,0,15|QAPX,\2C4,0,15<
>SUC06 $CLAVE:| QCP,5,6<
>STD15D0006000000<
>STR00000200<
>STR01000500<
>STR02000600<
>STR03000700<
>SRL20E;TRG=PWR+;CND=UV00!;ACC={STD15R}<
>SRL21E;TRG=TD15+;CND=WIFCEL|UV00!&;ACC={SRTC AUTO}<
>SRL22E;TRG=TR00+;ACC={SRTC AUTO}<
>SRL23E;TRG=TR01+;CND=UV00;ACC={SU5}{GU600H;@GP}<
>SRL24E;TRG=TR02+;CND=UV00;ACC={SCXWIF0,1,0}<
>SRL25E;TRG=TR03+;ACC={SCXWIF1,1,0}<
```

**Template** (placeholders `@type:name`, parameters `{name}`)

```
>UQCP,5,6,10100,320000<
>SUC@uc:setClave $SAPX| QAPX,\2C0,4,30| $,{prefijo}| QCP,5,6|QAPX,\2C2,0,15|QAPX,\2C3,0,15|QAPX,\2C4,0,15<
>SUC@uc:avisoClave $CLAVE:| QCP,5,6<
>STD@td:retryRtcD{retrySeg}00000<
>STR@tr:sync{horaSync}<
>STR@tr:set{horaSet}<
>STR@tr:off{horaOff}<
>STR@tr:on{horaOn}<
>SRL@rl:bootSinHoraE;TRG=PWR+;CND=@uv:fechaOk!;ACC={STD@td:retryRtcR}<
>SRL@rl:reintentaHoraE;TRG=TD@td:retryRtc+;CND=WIFCEL|@uv:fechaOk!&;ACC={SRTC AUTO}<
>SRL@rl:syncE;TRG=TR@tr:sync+;ACC={SRTC AUTO}<
>SRL@rl:aplicaE;TRG=TR@tr:set+;CND=@uv:fechaOk;ACC={SU@uc:setClave}{GU@uc:avisoClave00H;@GP}<
>SRL@rl:wifiOffE;TRG=TR@tr:off+;CND=@uv:fechaOk;ACC={SCXWIF0,1,{autodestiny}}<
>SRL@rl:wifiOnE;TRG=TR@tr:on+;ACC={SCXWIF1,1,{autodestiny}}<
```

**Notes**

- Validado en el pipeline del configurador: 14 comandos, skipped [], unhandled [], y en modo avanzado el re-export devuelve todo (el timer sale como STD15D0006000000, con la distancia explicita). La regla con dos bloques ACC={SU5}{GU600H;@GP} se importa como dos acciones raw y se re-exporta igual.
- El UV de la fecha cumple DOS roles: guarda de la clave, y senal de 'ya tengo hora' para el automatismo de puesta en hora. Por eso esta receta NO se aplica junto con sincronizar-hora-red.json: se pisarian (ese patron ya esta incorporado aca, con el mismo UV).
- Los dos caminos de puesta en hora son complementarios: el del timer arranca solo al bootear SIN hora y deja de actuar en cuanto la consigue (la CND lo corta); el de las {horaSync} corre todos los dias aunque ya tenga hora, para corregir la deriva del RTC. Si el equipo pierde la hora en marcha, lo levanta el diario.
- QCP no genera trafico: QueryMsg llama AddRpt con que=false -> printmsg escribe directo en rsp, no encola ni transmite.
- QCQ,5,6 da la misma fecha con el mismo offset (RPT_CP y RPT_CQ comparten el bloque de printmsg). La receta sincronizar-hora-red.json ya usa ese patron para validar el RTC.
- La clave DEBE quedar en 8 caracteres o mas: WPA2 lo exige y wifiSetSsid() no valida el minimo (esp_wifi_set_config falla y el AP no levanta). Con 6 de fecha, el prefijo necesita 2 o mas.
- Reubicacion: @uc:setClave esta limitado a 00..0F porque SU<n> toma un solo digito hex. @uc:avisoClave tambien (GU<n>00H).
- En 1.09.25+ conviene SDA: la regla queda ACC={SDA <la misma plantilla>} y se ahorra el UC y el SU. Ademas 1.09.25 arregla dos bugs del camino SU/UC (overflow de heap en SU+QUC, OOB en el split de plantillas entre comillas).
- El 3er campo de SCXWIF (autodestiny) hay que leerlo del equipo con >QCXWIF< y respetarlo: la receta no lo puede adivinar.

## Corte remoto seguro: sólo detenido y autorizado por flag

<a id="corte-condicionado-velocidad"></a>

**id:** `corte-condicionado-velocidad`

Corte de motor seguro: la plataforma AUTORIZA el corte poniendo un flag en 1, y el equipo lo ejecuta recién cuando el vehículo está por debajo de una velocidad (default 10 km/h), evaluando cada N segundos (default 5). Si la plataforma baja el flag, se restituye el motor. Reporta cuando la salida de corte cambia. Es lo que Rinho instala de fábrica para el corte remoto (~20 configs). Un timer + un umbral + cuatro reglas.

- **Requires hardware:** rele-corte-xp (`?` = optional)
- **Uses modules:** td, vl
- **Footprint:** 4 rules, 1 timers, 1 vl
- **Example validated on:** zero, spider, smart
- **Modes:** basic, advanced

| Parameter | Default | Format | Meaning |
|---|---|---|---|
| `flagAutoriza` | `16` |  | Flag U (2 dígitos) que autoriza el corte. La plataforma manda >SSSU1611< para cortar y >SSSU1600< para restituir. |
| `salidaCorte` | `00` |  | Índice (2 dígitos) de la salida XP del relé de corte. |
| `velMax` | `10` | `4d` | Velocidad (km/h) por debajo de la cual se permite cortar. |
| `evalSeg` | `5` | `5d` | Cada cuántos segundos se evalúa. La mayoría de las plataformas los convierte a decimal (00-FF → 0-255: 31 → 49); Wialon en tramas ER los muestra tal cual. |
| `evCorteOn` | `83` |  | Evento salida de corte activada (dos dígitos hexa, 00-FF). La mayoría de las plataformas los convierte a decimal (00-FF → 0-255: 31 → 49); Wialon en tramas ER los muestra tal cual. |
| `evCorteOff` | `84` |  | Evento salida de corte liberada. La mayoría de las plataformas los convierte a decimal (00-FF → 0-255: 31 → 49); Wialon en tramas ER los muestra tal cual. |

**Resolved example**

```
>STD02E0000500000<
>SVL02E00100<
>SRL50E;TRG=TD02+;CND=VL02!U16&XP00!&;ACC={SSSXP0011}<
>SRL51E;TRG=TD02+;CND=U16!XP00&;ACC={SSSXP0000}<
>SRL38E;TRG=XP00+;ACC={GER83H;@LOG}<
>SRL39E;TRG=XP00-;ACC={GER84H;@LOG}<
```

**Template** (placeholders `@type:name`, parameters `{name}`)

```
>STD@td:evalE{evalSeg}00000<
>SVL@vl:corteE{velMax}0<
>SRL@rl:activarE;TRG=TD@td:eval+;CND=@vl:corte!U{flagAutoriza}&XP{salidaCorte}!&;ACC={SSSXP{salidaCorte}11}<
>SRL@rl:desactivarE;TRG=TD@td:eval+;CND=U{flagAutoriza}!XP{salidaCorte}&;ACC={SSSXP{salidaCorte}00}<
>SRL@rl:corteOnE;TRG=XP{salidaCorte}+;ACC={GER{evCorteOn}H;@LOG}<
>SRL@rl:corteOffE;TRG=XP{salidaCorte}-;ACC={GER{evCorteOff}H;@LOG}<
```

**Notes**

- Operación: cortar = >SSSU1611< (el equipo corta cuando el vehículo se detiene, a lo sumo evalSeg segundos después); restituir = >SSSU1600<. El flag persiste el reinicio, así que un vehículo con corte autorizado sigue cortado tras un reboot.
- CUIDADO con el flag: en otras configuraciones de Rinho U16 significa 'es de día' (velocidad día/noche por horario). Si la config ya usa U16 para otra cosa, cambiá flagAutoriza.
- Alternativa nativa: el módulo Corte Gradual (SGC) del equipo corta de a pulsos y con condición de velocidad propia; esta receta es la variante con timer y flag, que no depende de ese módulo y reporta el estado de la salida.
- Sin el dígito de filtrada en el SVL el firmware lee 0; acá se escribe explícito (0) porque se quiere la velocidad instantánea. Si el GPS da velocidad espuria detenido, subir velMax o usar 1 (filtrada).

## Corte de motor con preaviso de buzzer

<a id="corte-motor-preaviso-buzzer"></a>

**id:** `corte-motor-preaviso-buzzer`

El comando de corte del operador no corta directo: enciende el buzzer, y recién después de N segundos (default 120 = 2 min) de preaviso sonoro se activa la salida de corte de motor. Mientras suena el buzzer el corte se puede abortar apagándolo. El delay se hace con un TD one-shot armado al vuelo desde la regla (R con tiempo explícito, sin configuración E previa): el timer queda dormido hasta que se enciende el buzzer, y la regla de corte lo frena con C para que no re-cicle.

- **Requires hardware:** buzzer-xp, rele-corte-xp (`?` = optional)
- **Uses modules:** td
- **Footprint:** 3 rules, 1 timers
- **Example validated on:** spider, smart
- **Modes:** basic, advanced

| Parameter | Default | Format | Meaning |
|---|---|---|---|
| `salidaBuzzer` | `01` |  | Índice (2 dígitos) de la salida XP del buzzer. El comando de corte del operador es encender esta salida. |
| `salidaCorte` | `00` |  | Índice (2 dígitos) de la salida XP que corta el motor (relé). |
| `preavisoSeg` | `00120` |  | Duración del preaviso en segundos, 5 dígitos con ceros a la izquierda (00120 = 2 min). Va como tiempo del STD<td>R. |

**Resolved example**

```
>SRL68E;TRG=XP01+;ACC={STD02R00120}<
>SRL69E;TRG=TD02+;CND=XP01;ACC={STD02C}{SSSXP0011}{SSSXP0100}<
>SRL70E;TRG=XP01-;ACC={STD02C}<
```

**Template** (placeholders `@type:name`, parameters `{name}`)

```
>SRL@rl:armarE;TRG=XP{salidaBuzzer}+;ACC={STD@td:preavisoR{preavisoSeg}}<
>SRL@rl:cortarE;TRG=TD@td:preaviso+;CND=XP{salidaBuzzer};ACC={STD@td:preavisoC}{SSSXP{salidaCorte}11}{SSSXP{salidaBuzzer}00}<
>SRL@rl:abortarE;TRG=XP{salidaBuzzer}-;ACC={STD@td:preavisoC}<
```

**Notes**

- Operación (con el binding del ejemplo): comando de corte = >SSSXP0111< (enciende buzzer y arranca la cuenta); cancelar durante el preaviso = >SSSXP0100<; restituir el motor después del corte = >SSSXP0000<.
- Formato de salidas: SSSXP{idx:2d}{estado:2d} con estado 00|11 (o el corto SSXP{idx:1d}{estado:1d}). NO existe >SXP..< como comando suelto. Las señales XP{idx} sirven como TRG (flancos +/-/*) y como CND (nivel).
- Como R no persiste, si el equipo se reinicia en medio del preaviso se pierde la cuenta: el buzzer arranca apagado tras el boot y el corte no se ejecuta. Se resuelve remandando el comando de corte.
- Para dejar evidencia del corte en el log diferido (persiste sin cobertura), agregar un reporte con @LOG al ACC de @rl:cortar, p.ej. {GCQxxL;@LOG}.
- En modo básico las reglas custom van de RL50 en adelante; el binding del ejemplo (68/69/70) ya cumple.

## Flag 'en alguna zona' para usar como condición

<a id="dentro-de-alguna-zona-flag"></a>

**id:** `dentro-de-alguna-zona-flag`

Sintetiza en un flag U la señal 'el vehículo está dentro de ALGUNA de estas regiones', para usarla como condición en otras reglas (buzzer, corte, reportes distintos). Una regla sin acción evalúa cada segundo la OR de las regiones; mientras da verdadero rearma un timer corto y pone el flag en 1; cuando deja de dar, el timer vence y el flag vuelve a 0 (histéresis de N segundos). Reporta entrada y salida. Es el patrón más repetido de las configuraciones reales de Rinho (~12 configs). Un timer + cuatro reglas.

- **Requires hardware:** none (`?` = optional)
- **Uses modules:** td, rg
- **Footprint:** 5 rules, 1 timers, 1 flags
- **Example validated on:** zero, spider, smart
- **Modes:** basic, advanced

| Parameter | Default | Format | Meaning |
|---|---|---|---|
| `zonas` | `RG00RG01\|` |  | Condición en RPN con la OR de las regiones: RG00RG01\| para dos, RG00RG01\|RG02\| para tres, hasta diez por regla (RG00RG01\|RG02\|…RG09\|). Las regiones se definen aparte con su geometría. |
| `ventanaSeg` | `10` | `5d` | Segundos sin estar en ninguna zona antes de bajar el flag (histéresis). |
| `evEntra` | `20` |  | Evento entrada a zona (dos dígitos hexa, 00-FF). La mayoría de las plataformas los convierte a decimal (00-FF → 0-255: 31 → 49); Wialon en tramas ER los muestra tal cual. |
| `evSale` | `21` |  | Evento salida de zona. La mayoría de las plataformas los convierte a decimal (00-FF → 0-255: 31 → 49); Wialon en tramas ER los muestra tal cual. |

**Resolved example**

```
>STD06D0001000000<
>SRL80E;TRG=TT+;CND=RG00RG01|<
>SRL84E;TRG=RL80+;ACC={STD06R}{SSSU0611}<
>SRL85E;TRG=TD06+;ACC={STD06C}{SSSU0600}<
>SRL86E;TRG=U06+;ACC={GER20H;@LOG}<
>SRL87E;TRG=U06-;ACC={GER21H;@LOG}<
```

**Template** (placeholders `@type:name`, parameters `{name}`)

```
>STD@td:refrescoD{ventanaSeg}00000<
>SRL@rl:adentroE;TRG=TT+;CND={zonas}<
>SRL@rl:marcarE;TRG=@rl:adentro+;ACC={STD@td:refrescoR}{SSSU@u:enZona11}<
>SRL@rl:bajarE;TRG=TD@td:refresco+;ACC={STD@td:refrescoC}{SSSU@u:enZona00}<
>SRL@rl:entraE;TRG=@u:enZona+;ACC={GER{evEntra}H;@LOG}<
>SRL@rl:saleE;TRG=@u:enZona-;ACC={GER{evSale}H;@LOG}<
```

**Notes**

- TT+ es el tick de un segundo del motor de reglas: la regla sin acción se evalúa cada segundo y su señal RL<n> es un pulso cuando la CND es verdadera. El flag no titila en el borde de una zona gracias a la ventana: sale recién ventanaSeg segundos después del último segundo adentro.
- Uso típico como condición: CND=U<flag> (adentro) o CND=U<flag>! (afuera) en las reglas de velocidad, buzzer o corte. Ver velocidad-por-zona-buzzer para límites distintos por zona.
- El flag U lo elige la receta entre los libres mirando el script; un flag que otra regla use sólo como {SSSUxx11} en un ACC puede no contarse como usado. Si la config ya usa flags a mano, revisar que no choquen.
- Los 5 dígitos del STD son SEGUNDOS. El timer se declara con D y las reglas lo arrancan con R sin tiempo (usa ventanaSeg) y lo frenan con C, obligatorio en la regla de TD+ porque los TD se auto-recargan.

## Evento por entrada digital (puerta, tapa, desenganche)

<a id="evento-entrada-digital"></a>

**id:** `evento-entrada-digital`

Reporta un evento cuando una entrada digital se activa y otro cuando se desactiva: apertura y cierre de puerta, tapa abierta y cerrada, desenganche y enganche de un acoplado, o cualquier sensor de dos estados. Incluye el antirrebote de la entrada (SIT) para que un contacto que vibra no genere una ráfaga de eventos. Un antirrebote + dos reglas.

- **Requires hardware:** entrada-digital (`?` = optional)
- **Uses modules:** none
- **Footprint:** 2 rules
- **Example validated on:** smart
- **Modes:** basic, advanced

| Parameter | Default | Format | Meaning |
|---|---|---|---|
| `entrada` | `03` |  | Índice (2 dígitos) de la entrada digital del sensor. |
| `evActiva` | `08` |  | Evento cuando la entrada pasa a 1 (puerta abierta, tapa abierta, desenganche). Dos dígitos hexa, 00-FF. La mayoría de las plataformas los convierte a decimal (00-FF → 0-255: 31 → 49); Wialon en tramas ER los muestra tal cual. |
| `evInactiva` | `09` |  | Evento cuando la entrada vuelve a 0 (puerta cerrada, tapa cerrada, enganche). La mayoría de las plataformas los convierte a decimal (00-FF → 0-255: 31 → 49); Wialon en tramas ER los muestra tal cual. |
| `filtroDecimas` | `5` | `4d` | Antirrebote en décimas de segundo, el mismo en los dos sentidos (5 = 0,5 s): el estado tiene que sostenerse ese tiempo para que la señal cambie. |

**Resolved example**

```
>SIT0300050005<
>SRL06E;TRG=IN03+;ACC={GER08H;@LOG}<
>SRL07E;TRG=IN03-;ACC={GER09H;@LOG}<
```

**Template** (placeholders `@type:name`, parameters `{name}`)

```
>SIT{entrada}{filtroDecimas}{filtroDecimas}<
>SRL@rl:activaE;TRG=IN{entrada}+;ACC={GER{evActiva}H;@LOG}<
>SRL@rl:inactivaE;TRG=IN{entrada}-;ACC={GER{evInactiva}H;@LOG}<
```

**Notes**

- Qué es 'activa' depende del cableado: un sensor que cierra a masa al abrir la puerta pone la entrada en 0 al abrir, y entonces los eventos van al revés. Si al probar salen cruzados, intercambiar evActiva y evInactiva.
- La ignición es la entrada 07 en todos los modelos; para eventos de ignición no hace falta esta receta: el configurador ya trae las reglas de IGN+ e IGN-.
- El evento sale con GER (reporte extendido). Si la plataforma espera otro tipo, cambiar GER por GCQ o GBQ; el número de evento es el mismo. Con @LOG el reporte persiste en flash sin cobertura.
- Para un pulsador que tiene que mantenerse apretado (pánico), usar boton-panico-pulsador-mantenido. Para alarmar si la entrada queda activa más de N segundos (puerta que quedó abierta), puerta-abierta-demasiado-tiempo.

## Evento al entrar y salir de una zona del catálogo masivo (ZNA/ZNB)

<a id="evento-zona-masiva"></a>

**id:** `evento-zona-masiva`

Reporta un evento al entrar y otro al salir de una zona lógica del módulo de zonas (catálogo de hasta 4096 geocercas cargado con SZONES). Dos reglas por zona; CND=GPS evita la salida falsa cuando se pierde el fix. Repetir la receta por cada zona que tenga que avisar (zonas 00-99 de cada grupo).

- **Requires hardware:** none (`?` = optional)
- **Uses modules:** zones
- **Footprint:** 2 rules
- **Example validated on:** zero, spider, smart
- **Modes:** basic, advanced

| Parameter | Default | Format | Meaning |
|---|---|---|---|
| `zona` | `ZNA00` |  | Zona lógica, con su grupo: ZNA00..ZNA99 o ZNB00..ZNB99 (índice decimal). Las zonas 100 en adelante no se pueden usar en reglas. |
| `evEntra` | `20` |  | Evento al entrar (dos dígitos hexa, 00-FF). La mayoría de las plataformas los convierte a decimal (00-FF → 0-255: 31 → 49); Wialon en tramas ER los muestra tal cual. |
| `evSale` | `21` |  | Evento al salir. La mayoría de las plataformas los convierte a decimal (00-FF → 0-255: 31 → 49); Wialon en tramas ER los muestra tal cual. |

**Resolved example**

```
>SRL60E;TRG=ZNA00+;CND=GPS;ACC={GER20H;@LOG}<
>SRL61E;TRG=ZNA00-;CND=GPS;ACC={GER21H;@LOG}<
```

**Template** (placeholders `@type:name`, parameters `{name}`)

```
>SRL@rl:entraE;TRG={zona}+;CND=GPS;ACC={GER{evEntra}H;@LOG}<
>SRL@rl:saleE;TRG={zona}-;CND=GPS;ACC={GER{evSale}H;@LOG}<
```

**Notes**

- Requiere el catálogo cargado: SZFRAC + SZMODE1 (reinicia el equipo, exige el registro vacío) + SZONES"http://…/zones.bin", y firmware 1.09.21 o posterior. Sin catálogo la zona nunca se activa.
- La zona es una señal de nivel: + al entrar, - al salir, * cualquier cambio. La evaluación es cada 1 s.
- Para varias zonas con el mismo par de eventos, en vez de repetir la receta se pueden poner varios elementos del catálogo en la misma zona lógica. Repetir la receta sólo cuando cada zona tiene que avisar con un evento distinto.
- Si la acción es crítica (corte, sirena), dejar el CND=GPS también en la regla de entrada.

## Exceso de velocidad con buzzer (un solo límite)

<a id="exceso-velocidad-buzzer"></a>

**id:** `exceso-velocidad-buzzer`

Aviso de exceso de velocidad simple, sin zonas: al superar el límite suena el buzzer (salida digital) y se reporta un evento; al volver por debajo de un segundo umbral más bajo (histéresis) el buzzer se apaga y se reporta el fin del exceso. Es lo que Rinho instala de fábrica en las flotas con buzzer. Dos umbrales VL + dos reglas.

- **Requires hardware:** buzzer-xp (`?` = optional)
- **Uses modules:** vl
- **Footprint:** 2 rules, 2 vl
- **Example validated on:** spider, smart
- **Modes:** advanced

| Parameter | Default | Format | Meaning |
|---|---|---|---|
| `velOn` | `110` | `4d` | Límite de velocidad (km/h): al superarlo suena el buzzer y se reporta. |
| `velOff` | `100` | `4d` | Umbral de apagado (km/h), más bajo que velOn: histéresis para que el buzzer no titile al rondar el límite. |
| `buzzerXp` | `1` |  | Índice (1 dígito) de la salida XP del buzzer. Encender: SSSXP0<xp>11, apagar: SSSXP0<xp>00. |
| `evOn` | `53` |  | Evento de exceso (dos dígitos hexa, 00-FF). La mayoría de las plataformas los convierte a decimal (00-FF → 0-255: 31 → 49); Wialon en tramas ER los muestra tal cual. |
| `evOff` | `54` |  | Evento de fin del exceso. La mayoría de las plataformas los convierte a decimal (00-FF → 0-255: 31 → 49); Wialon en tramas ER los muestra tal cual. |

**Resolved example**

```
>SVL00E01101<
>SVL01E01001<
>SRL16E;TRG=VL00+;ACC={GER53H;@LOG}{SSSXP0111}<
>SRL17E;TRG=VL01-;CND=XP01;ACC={GER54H;@LOG}{SSSXP0100}<
```

**Template** (placeholders `@type:name`, parameters `{name}`)

```
>SVL@vl:onE{velOn}1<
>SVL@vl:offE{velOff}1<
>SRL@rl:onE;TRG=VL@vl:on+;ACC={GER{evOn}H;@LOG}{SSSXP0{buzzerXp}11}<
>SRL@rl:offE;TRG=VL@vl:off-;CND=XP0{buzzerXp};ACC={GER{evOff}H;@LOG}{SSSXP0{buzzerXp}00}<
```

**Notes**

- En modo BÁSICO no hace falta: el módulo Velocidad del configurador arma exactamente esto (VL de encendido y apagado + salida). La receta es para modo avanzado, donde todo se escribe a mano.
- Buzzer: la salida tiene que estar configurada aparte como salida con blink, p.ej. >SCXXP01000000010004< (XP01, 100 ms ON / 400 ms OFF); SSSXP sólo la enciende y apaga.
- Si quieren el aviso sólo cuando el exceso dura N segundos, usar la receta exceso-velocidad-sostenido: usa el tiempo de establecimiento del propio VL (tres dígitos en décimas después del dígito de filtrada, campo Sostenido en el configurador).
- Zonas: si el límite tiene que cambiar dentro de una región o un waypoint, usar velocidad-por-zona-buzzer.

## Exceso de velocidad sostenido N segundos

<a id="exceso-velocidad-sostenido"></a>

**id:** `exceso-velocidad-sostenido`

Alarma de exceso de velocidad que sólo dispara si el vehículo se mantiene por encima del límite durante N segundos (default 10): un pico de un instante no alarma. Lo hace el propio umbral VL con su tiempo de establecimiento: la señal sube recién cuando la velocidad lleva N segundos por encima del límite, y baja recién cuando lleva N segundos por debajo. Al subir reporta y enciende el buzzer; al bajar apaga y reporta el fin. Un umbral + dos reglas, sin timer.

- **Requires hardware:** buzzer-xp (`?` = optional)
- **Uses modules:** vl
- **Footprint:** 2 rules, 1 vl
- **Example validated on:** spider, smart
- **Modes:** basic, advanced

| Parameter | Default | Format | Meaning |
|---|---|---|---|
| `velMax` | `110` | `4d` | Límite de velocidad (km/h) que hay que sostener. |
| `decimas` | `100` | `3d` | Tiempo de establecimiento en décimas de segundo (100 = 10 s): cuánto tiene que sostenerse por encima para alarmar, y por debajo para dar el fin. |
| `buzzerXp` | `1` |  | Índice (1 dígito) de la salida XP del buzzer. Encender: SSSXP0<xp>11, apagar: SSSXP0<xp>00. |
| `evOn` | `53` |  | Evento de exceso sostenido (dos dígitos hexa, 00-FF). La mayoría de las plataformas los convierte a decimal (00-FF → 0-255: 31 → 49); Wialon en tramas ER los muestra tal cual. |
| `evOff` | `54` |  | Evento de fin del exceso. La mayoría de las plataformas los convierte a decimal (00-FF → 0-255: 31 → 49); Wialon en tramas ER los muestra tal cual. |

**Resolved example**

```
>SVL00E01101100<
>SRL50E;TRG=VL00+;ACC={GER53H;@LOG}{SSSXP0111}<
>SRL51E;TRG=VL00-;ACC={GER54H;@LOG}{SSSXP0100}<
```

**Template** (placeholders `@type:name`, parameters `{name}`)

```
>SVL@vl:limiteE{velMax}1{decimas}<
>SRL@rl:alarmaE;TRG=VL@vl:limite+;ACC={GER{evOn}H;@LOG}{SSSXP0{buzzerXp}11}<
>SRL@rl:finE;TRG=VL@vl:limite-;ACC={GER{evOff}H;@LOG}{SSSXP0{buzzerXp}00}<
```

**Notes**

- Si querés umbrales distintos para encender y apagar (110 para alarmar, 100 para dar el fin) usá exceso-velocidad-buzzer, que tiene dos VL con histéresis por velocidad; ésta tiene histéresis por tiempo.
- Buzzer: la salida tiene que estar configurada aparte como salida con blink, p.ej. >SCXXP01000000010004< (XP01, 100 ms ON / 400 ms OFF).
- Sin buzzer (sólo el evento): sacar los bloques {SSSXP…} de las dos reglas.

## Frenada y aceleración bruscas confirmadas por timer

<a id="frenada-acelerada-confirmada"></a>

**id:** `frenada-acelerada-confirmada`

Eventos de frenada brusca y aceleración brusca por delta de velocidad GPS (módulo DV), con confirmación: el evento se reporta sólo si la señal DV sigue activa después de N segundos (default 2). Un pico de GPS de un instante arma el timer y lo cancela antes de vencer, así que no reporta. Es el anti-falso-positivo que Rinho usa de fábrica en ~10 configuraciones. Dos DV + dos timers + siete reglas.

- **Requires hardware:** none (`?` = optional)
- **Uses modules:** dv, td
- **Footprint:** 7 rules, 2 dv, 2 timers
- **Example validated on:** zero, spider, smart
- **Modes:** advanced

| Parameter | Default | Format | Meaning |
|---|---|---|---|
| `deltaFrenada` | `15` | `3d` | Caída de velocidad en km/h en un segundo que cuenta como frenada brusca (15 = -15 km/h/s). |
| `deltaAcel` | `10` | `3d` | Aumento de velocidad en km/h en un segundo que cuenta como aceleración brusca. |
| `confSeg` | `2` | `5d` | Segundos que la señal DV tiene que sostenerse para reportar. |
| `evFrenada` | `31` |  | Evento frenada brusca (dos dígitos hexa, 00-FF). La mayoría de las plataformas los convierte a decimal (00-FF → 0-255: 31 → 49); Wialon en tramas ER los muestra tal cual. |
| `evAcel` | `30` |  | Evento aceleración brusca. La mayoría de las plataformas los convierte a decimal (00-FF → 0-255: 31 → 49); Wialon en tramas ER los muestra tal cual. |

**Resolved example**

```
>SDV00-015<
>SDV01+010<
>STD08D0000200000<
>STD09D0000200000<
>SRLC0E;TRG=IGN+;ACC={STD08C}{STD09C}<
>SRLC1E;TRG=DV00+;CND=IGN;ACC={STD08R}<
>SRLC2E;TRG=DV00-;ACC={STD08C}<
>SRL22E;TRG=TD08+;ACC={STD08C}{GER31H;@LOG}<
>SRLC3E;TRG=DV01+;CND=IGN;ACC={STD09R}<
>SRLC4E;TRG=DV01-;ACC={STD09C}<
>SRL23E;TRG=TD09+;ACC={STD09C}{GER30H;@LOG}<
```

**Template** (placeholders `@type:name`, parameters `{name}`)

```
>SDV@dv:frenada-{deltaFrenada}<
>SDV@dv:acelerada+{deltaAcel}<
>STD@td:confFrenD{confSeg}00000<
>STD@td:confAcelD{confSeg}00000<
>SRL@rl:resetE;TRG=IGN+;ACC={STD@td:confFrenC}{STD@td:confAcelC}<
>SRL@rl:frenIniE;TRG=@dv:frenada+;CND=IGN;ACC={STD@td:confFrenR}<
>SRL@rl:frenFinE;TRG=@dv:frenada-;ACC={STD@td:confFrenC}<
>SRL@rl:frenOkE;TRG=TD@td:confFren+;ACC={STD@td:confFrenC}{GER{evFrenada}H;@LOG}<
>SRL@rl:acelIniE;TRG=@dv:acelerada+;CND=IGN;ACC={STD@td:confAcelR}<
>SRL@rl:acelFinE;TRG=@dv:acelerada-;ACC={STD@td:confAcelC}<
>SRL@rl:acelOkE;TRG=TD@td:confAcel+;ACC={STD@td:confAcelC}{GER{evAcel}H;@LOG}<
```

**Notes**

- DV mide el delta de velocidad GPS entre dos muestras de un segundo: SDV<idx2><signo><delta3>. Los DV por defecto de fábrica (-010/+010) sin confirmación disparan con cualquier salto del GPS; por eso la guarda.
- En modo BÁSICO no hace falta: el módulo Conducción del configurador arma frenada y aceleración (sin la confirmación por timer). Para sumarle la confirmación, pasar a avanzado y aplicar esta receta.
- Si querés el comportamiento del chofer con score (contadores por viaje), ver score-conduccion-viaje: usa HM (maniobras con acelerómetro) en vez de DV.
- Los 5 dígitos del STD son SEGUNDOS. Los timers se declaran con D y las reglas los arrancan con R (sin tiempo: usan los confSeg de la declaración) y los frenan con C, obligatorio en la regla de TD+ porque los TD se auto-recargan.

## Horómetro por ignición

<a id="horometro-ignicion"></a>

**id:** `horometro-ignicion`

Cuenta horas de motor: arranca el conteo cuando aparece la ignición (y también si el equipo bootea ya con ignición presente, p.ej. tras un corte de alimentación) y lo detiene al quitarla. El total se acumula on-device, independiente del envío de reportes.

- **Requires hardware:** none (`?` = optional)
- **Uses modules:** none
- **Footprint:** 3 rules
- **Example validated on:** zero, spider, smart
- **Modes:** advanced

| Parameter | Default | Format | Meaning |
|---|---|---|---|
| `ignSig` | `IGN` |  | Señal de ignición: IGN lógica o una entrada física INxx según cableado |

**Resolved example**

```
>SRL31E;TRG=PWR+;CND=IGN;ACC={SHOE}<
>SRL32E;TRG=IGN+;ACC={SHOE}<
>SRL33E;TRG=IGN-;ACC={SHOD}<
```

**Template** (placeholders `@type:name`, parameters `{name}`)

```
>SRL@rl:bootRunningE;TRG=PWR+;CND={ignSig};ACC={SHOE}<
>SRL@rl:startE;TRG={ignSig}+;ACC={SHOE}<
>SRL@rl:stopE;TRG={ignSig}-;ACC={SHOD}<
```

**Notes**

- El total acumulado vive en el equipo y no depende de la cobertura; lo leés/reportás aparte (p.ej. en el reporte periódico).
- ignSig: si tu ignición es una entrada digital, usá INxx en los tres ítems (y evitá mezclar IGN lógica con INxx en otras reglas de la misma config). PWR es singleton compartido, no se reubica.
- Sin dependencia de orden entre las 3 reglas.

## Ignición por cable O por movimiento (velocidad GPS)

<a id="ignicion-cable-o-movimiento"></a>

**id:** `ignicion-cable-o-movimiento`

Ignición virtual con OR de fuentes: enciende con el cable físico O con velocidad GPS sostenida sobre un umbral; apaga tras quietud prolongada sin cable. Usa el modo 3 de CXIGN (ignición manual + libera IN07): las reglas manejan la ignición REAL del equipo vía SSSIGN11/00, así que reportes, viajes STP y toda regla con IGN la siguen — no es un flag paralelo. El cable queda visible como entrada IN07 para las reglas. La histéresis la da el propio ton/toff del módulo IGN (SSSIGN00 arranca el toff; si vuelve el movimiento antes de vencer, se cancela). Un TD periódico reconcilia el estado (cubre reinicios con estado pegado). Variante opcional con acelerómetro (señal AC) para arranque instantáneo sin fix GPS: ver notas.

- **Requires hardware:** gps-siempre-activo, acc? (`?` = optional)
- **Uses modules:** vl, td
- **Footprint:** 4 rules, 1 vl, 1 timers
- **Example validated on:** zero, spider, smart
- **Modes:** basic, advanced

| Parameter | Default | Format | Meaning |
|---|---|---|---|
| `velUmbral` | `0010` |  | Umbral de 'en movimiento' en km/h, 4 dígitos con ceros (0010 = 10 km/h). Bajar a 0006-0008 si preocupa tráfico ultra lento; el establecimiento + filtrada protegen del ruido GPS parado (1-3 km/h). |
| `velFiltrada` | `1` |  | 1 = usa la velocidad GPS filtrada (getGpsFiltrada) en vez de la cruda. Dejar en 1. |
| `velEstablecimiento` | `050` |  | Tiempo de establecimiento del VL en DÉCIMAS de segundo, 3 dígitos (050 = 5 s). Es simétrico: la velocidad debe sostenerse ese tiempo por encima del umbral para subir la señal y por debajo para bajarla. |
| `ton` | `30` |  | ton de SCXIGN en unidades de 100 ms (30 = 3 s): cuánto debe sostenerse el ON para confirmar ignición. |
| `toff` | `3000` |  | toff de SCXIGN en unidades de 100 ms (3000 = 5 min): gracia de quietud antes de confirmar OFF. Absorbe semáforos y tráfico lento. Con la variante acelerómetro puede bajar a 1200 (2 min). |
| `tickSeg` | `00060` |  | Período del TD de reconciliación en segundos, 5 dígitos (comando STD<idx>E<tickSeg>00000). El OFF efectivo total es ≤tick + toff de quietud. |

**Resolved example**

```
>SVL04E00101050<
>SCXIGN3,30,3000<
>STD03E0006000000<
>SRL71E;TRG=IN07+;ACC={SSSIGN11}<
>SRL72E;TRG=VL04+;ACC={SSSIGN11}<
>SRL73E;TRG=TD03+;CND=IN07!VL04!&IGN&;ACC={SSSIGN00}<
>SRL74E;TRG=TD03+;CND=IN07VL04|IGN!&;ACC={SSSIGN11}<
```

**Template** (placeholders `@type:name`, parameters `{name}`)

```
>SVL@vl:movimientoE{velUmbral}{velFiltrada}{velEstablecimiento}<
>SCXIGN3,{ton},{toff}<
>STD@td:tickE{tickSeg}00000<
>SRL@rl:onCableE;TRG=IN07+;ACC={SSSIGN11}<
>SRL@rl:onVelE;TRG=VL@vl:movimiento+;ACC={SSSIGN11}<
>SRL@rl:offReconE;TRG=TD@td:tick+;CND=IN07!VL@vl:movimiento!&IGN&;ACC={SSSIGN00}<
>SRL@rl:onReconE;TRG=TD@td:tick+;CND=IN07VL@vl:movimiento|IGN!&;ACC={SSSIGN11}<
```

**Notes**

- GOTCHA CRÍTICO — GPS siempre activo: si la config apaga o congela el GPS sin ignición (p.ej. SCXGPS1 de la receta parking-auto-posicion-fija, o modos de ahorro), VL nunca sube → sin cable la ignición no enciende JAMÁS (deadlock). Verificar la config GPS con QCX antes de aplicar. Esta receta es incompatible con parking-auto-posicion-fija tal cual.
- Cada SSSIGN escribe NVS (ignSaveState → ignSetNvsState): el estado sobrevive reinicios (ignInit lo restaura), y por eso hacen falta las reglas de reconciliación — tras un reboot con estado pegado no hay flancos que lo corrijan; el tick lo arregla en ≤1 período. Con uso vehicular normal el desgaste de flash es irrelevante.
- Convivencia: no pisa la receta corte-motor-preaviso-buzzer (bindings de ejemplo disjuntos: esta usa TD03/RL71-74, aquella TD02/RL68-70).

## Jamming validado con guarda y corte gradual

<a id="jamming-validado-corte"></a>

**id:** `jamming-validado-corte`

Reacción a la interferencia de celular (jamming) sin falsos positivos: al detectarse arranca una guarda de N segundos (default 60) y se reporta el inicio; si la interferencia desaparece antes se cancela y se reporta el fin; si al vencer sigue, queda validada: se reporta y se activa el corte gradual de motor. La rehabilitación la dispara una señal a elección (default: un chofer que inicia sesión con su llave, IC00+). Un timer + cuatro reglas.

- **Requires hardware:** rele-corte-xp (`?` = optional)
- **Uses modules:** td, gc
- **Footprint:** 4 rules, 1 timers
- **Example validated on:** zero, spider, smart
- **Modes:** basic, advanced

| Parameter | Default | Format | Meaning |
|---|---|---|---|
| `guardaSeg` | `60` | `5d` | Segundos de interferencia sostenida antes de darla por válida. |
| `senalRehab` | `IC00+` |  | Señal (con flanco) que rehabilita el motor: IC00+ = un chofer inició sesión; U10+ = flag que setea la plataforma con >SSSU1011<; IN01+ = pulsador. |
| `evInicio` | `45` |  | Evento inicio de interferencia (dos dígitos hexa, 00-FF). La mayoría de las plataformas los convierte a decimal (00-FF → 0-255: 31 → 49); Wialon en tramas ER los muestra tal cual. |
| `evFin` | `46` |  | Evento fin de la interferencia antes de validarse. La mayoría de las plataformas los convierte a decimal (00-FF → 0-255: 31 → 49); Wialon en tramas ER los muestra tal cual. |
| `evValidado` | `47` |  | Evento interferencia validada (se ejecuta el corte). La mayoría de las plataformas los convierte a decimal (00-FF → 0-255: 31 → 49); Wialon en tramas ER los muestra tal cual. |
| `evRehab` | `48` |  | Evento rehabilitación del motor. La mayoría de las plataformas los convierte a decimal (00-FF → 0-255: 31 → 49); Wialon en tramas ER los muestra tal cual. |

**Resolved example**

```
>STD02D0006000000<
>SRL22E;TRG=JMD+;ACC={GER45H;@LOG}{STD02R}<
>SRL23E;TRG=JMD-;ACC={STD02C}{GER46H;@LOG}<
>SRL24E;TRG=TD02+;ACC={STD02C}{GER47H;@LOG}{SGCA}<
>SRL25E;TRG=IC00+;ACC={SGCS}{GER48H;@LOG}<
```

**Template** (placeholders `@type:name`, parameters `{name}`)

```
>STD@td:guardaD{guardaSeg}00000<
>SRL@rl:inicioE;TRG=JMD+;ACC={GER{evInicio}H;@LOG}{STD@td:guardaR}<
>SRL@rl:finE;TRG=JMD-;ACC={STD@td:guardaC}{GER{evFin}H;@LOG}<
>SRL@rl:validadoE;TRG=TD@td:guarda+;ACC={STD@td:guardaC}{GER{evValidado}H;@LOG}{SGCA}<
>SRL@rl:rehabilitarE;TRG={senalRehab};ACC={SGCS}{GER{evRehab}H;@LOG}<
```

**Notes**

- El corte gradual (SGCA) requiere el módulo GC configurado y habilitado en el equipo: >SGCE,00,pppp,iiii,mmmm< (salida XP del relé, paso en ms, intervalo en s, máximo en s). Sin SGCE el SGCA no hace nada. En el configurador está en el módulo Corte Gradual.
- Todos los reportes van con @LOG a propósito: durante el jamming no hay red, y sin @LOG el reporte en vivo se pierde. Con @LOG queda en flash y sale cuando vuelve la cobertura.
- Los 5 dígitos del STD son SEGUNDOS. La guarda se declara con D y las reglas la arrancan con R sin tiempo (usa guardaSeg) y la frenan con C, obligatorio en la regla de TD+ porque los TD se auto-recargan.

## Parking automático con posición fija

<a id="parking-auto-posicion-fija"></a>

**id:** `parking-auto-posicion-fija`

Al quitar la ignición arranca la detección de parking y congela la posición GPS para que el vehículo estacionado no derive por ruido del receptor. Cuando el parking se confirma (quedó quieto dentro del radio el tiempo configurado) reporta el evento y vuelve el GPS a modo normal. Opcionalmente silencia el buzzer al estacionar.

- **Requires hardware:** xp? (`?` = optional)
- **Uses modules:** pk
- **Footprint:** 2 rules, 1 pk
- **Example validated on:** spider, smart
- **Modes:** advanced

| Parameter | Default | Format | Meaning |
|---|---|---|---|
| `ignSig` | `IN07` |  | Señal de ignición (IN07 física o IGN lógica) |
| `parkEvent` | `78` |  | Evento al confirmar parking (hex 00-FF) |
| `gpsParkMode` | `1` |  | Modo GPS al estacionar: 1 = no actualiza sin ignición (congela); 2 = no actualiza salvo ignición+velocidad |
| `gpsRunMode` | `0` |  | Modo GPS al confirmar parking: 0 = actualiza siempre (normal). Dejalo en 1 si querés mantener la posición congelada todo el estacionamiento |
| `buzzerXp` | `2` |  | Índice de la salida del buzzer a silenciar al estacionar (opcional). Omití el {SSSXP0..} si no usás buzzer |
| `radioMetros` | `50` | `4d` | Radio del parking en metros (4 dígitos): moverse más que esto rompe el parking. |
| `confirmSeg` | `15` | `4d` | Segundos quieto para confirmar el parking (4 dígitos): recién ahí dispara PK00+. |

**Resolved example**

```
>SPKE00500015<
>SRL34E;TRG=IN07-;ACC={SPKA}{SCXGPS1}{SSSXP0200}<
>SRL35E;TRG=PK00+;CND=IN07!;ACC={GER78H;@LOG}{SCXGPS0}<
```

**Template** (placeholders `@type:name`, parameters `{name}`)

```
>SPKE{radioMetros}{confirmSeg}<
>SRL@rl:enterParkE;TRG={ignSig}-;ACC={SPKA}{SCXGPS{gpsParkMode}}{SSSXP0{buzzerXp}00}<
>SRL@rl:parkConfirmedE;TRG=PK00+;CND={ignSig}!;ACC={GER{parkEvent}H;@LOG}{SCXGPS{gpsRunMode}}<
```

**Notes**

- OJO con el comando: es SCXGPS, no SCXPGS. SCXPGS no existe en el firmware → comando desconocido / no-op silencioso (no congela nada). Era un typo frecuente.
- Por qué el ejemplo vuelve a SCXGPS0 al confirmar: una vez detectado el parking, el ancla de posición la sostiene el propio parking; si preferís mantener la posición congelada todo el estacionamiento, poné gpsRunMode = 1.
- El {SSSXP0<xp>00} silencia el buzzer al estacionar; es opcional y se combina bien con velocidad-por-zona-buzzer. Si no tenés buzzer, quitá esa acción del @rl:enterPark.
- ignSig: IN07 física en el ejemplo; con IGN lógica el flanco de entrada sería IGN- (y no mezcles IN07 con IGN en la misma config).
- El reporte usa destino LOG (;@LOG) porque el parking ocurre justo al apagar, cuando puede no haber cobertura o venir un sleep: así el evento no se pierde.

## Posición fija clavada en cada arranque (activo inmóvil)

<a id="posicion-fija-al-arrancar"></a>

**id:** `posicion-fija-al-arrancar`

Para un equipo instalado en un lugar fijo (búnker, galpón, bomba, tablero) donde el GPS no ve el cielo o no importa: en cada arranque escribe la latitud y longitud del sitio, así todos los reportes salen con esa posición. Distinto de parking-auto-posicion-fija, que congela la posición al quitar el contacto. Una regla.

- **Requires hardware:** none (`?` = optional)
- **Uses modules:** none
- **Footprint:** 1 rules
- **Example validated on:** zero, spider, smart
- **Modes:** basic, advanced

| Parameter | Default | Format | Meaning |
|---|---|---|---|
| `lat` | `-34.60373` |  | Latitud del sitio en grados decimales, con signo (sur negativo). |
| `lon` | `-58.38157` |  | Longitud del sitio en grados decimales, con signo (oeste negativo). |

**Resolved example**

```
>SRL14E;TRG=PWR+;ACC={SLAT-34.60373}{SLON-58.38157}<
```

**Template** (placeholders `@type:name`, parameters `{name}`)

```
>SRL@rl:posicionE;TRG=PWR+;ACC={SLAT{lat}}{SLON{lon}}<
```

**Notes**

- Si el equipo tiene GPS con vista al cielo, cada fix nuevo pisa la posición fijada; para que quede clavada también hay que poner el GPS en modo fijo (>SCXGPS1<, ver parking-auto-posicion-fija) o dejarlo apagado. Con GPS sin señal la posición fijada es la que se reporta.
- Los reportes salen con la posición del sitio pero con la hora del equipo; si el equipo no tiene GPS ni red, la hora puede no ser válida (ver sincronizar-hora-red).

## Puerta abierta demasiado tiempo (alarma con sirena)

<a id="puerta-abierta-demasiado-tiempo"></a>

**id:** `puerta-abierta-demasiado-tiempo`

Alarma cuando una entrada digital queda activa más de N segundos (default 10): puerta o portón que quedó abierto, tapa que no cerraron. Al activarse la entrada arranca un timer de guarda y se reporta la apertura; si se desactiva antes se cancela y se reporta el cierre; si vence con la entrada todavía activa, enciende una salida (sirena o buzzer) y reporta la alarma. Se rearma sola tras un reinicio con la puerta abierta. Un timer + cinco reglas.

- **Requires hardware:** entrada-digital, salida-xp (`?` = optional)
- **Uses modules:** td
- **Footprint:** 5 rules, 1 timers
- **Example validated on:** spider, smart
- **Modes:** basic, advanced

| Parameter | Default | Format | Meaning |
|---|---|---|---|
| `entrada` | `00` |  | Índice (2 dígitos) de la entrada digital del sensor de puerta. Activa = 1 = abierta. |
| `segundos` | `10` | `5d` | Cuántos segundos puede quedar abierta antes de alarmar. |
| `salidaAlarma` | `02` |  | Índice (2 dígitos) de la salida XP de la sirena o buzzer. Se enciende al alarmar y se apaga al cerrar la puerta. |
| `evAbre` | `10` |  | Evento de apertura (dos dígitos hexa, 00-FF). La mayoría de las plataformas los convierte a decimal (00-FF → 0-255: 31 → 49); Wialon en tramas ER los muestra tal cual. |
| `evCierra` | `11` |  | Evento de cierre. La mayoría de las plataformas los convierte a decimal (00-FF → 0-255: 31 → 49); Wialon en tramas ER los muestra tal cual. |
| `evAlarma` | `40` |  | Evento de puerta abierta demasiado tiempo. La mayoría de las plataformas los convierte a decimal (00-FF → 0-255: 31 → 49); Wialon en tramas ER los muestra tal cual. |

**Resolved example**

```
>STD02D0001000000<
>SRL05E;TRG=IN00+;ACC={STD02R00010}{GER10H;@LOG}<
>SRL06E;TRG=IN00-;ACC={STD02C}{SSSXP0200}{GER11H;@LOG}<
>SRL07E;TRG=TD02+;CND=IN00;ACC={STD02C}{SSSXP0211}{GER40H;@LOG}<
>SRL08E;TRG=PWR+;CND=IN00;ACC={STD02R00010}{GER10H;@LOG}<
>SRL09E;TRG=TD02+;CND=IN00!;ACC={STD02C}<
```

**Template** (placeholders `@type:name`, parameters `{name}`)

```
>STD@td:guardaD{segundos}00000<
>SRL@rl:abreE;TRG=IN{entrada}+;ACC={STD@td:guardaR{segundos}}{GER{evAbre}H;@LOG}<
>SRL@rl:cierraE;TRG=IN{entrada}-;ACC={STD@td:guardaC}{SSSXP{salidaAlarma}00}{GER{evCierra}H;@LOG}<
>SRL@rl:alarmaE;TRG=TD@td:guarda+;CND=IN{entrada};ACC={STD@td:guardaC}{SSSXP{salidaAlarma}11}{GER{evAlarma}H;@LOG}<
>SRL@rl:reinicioE;TRG=PWR+;CND=IN{entrada};ACC={STD@td:guardaR{segundos}}{GER{evAbre}H;@LOG}<
>SRL@rl:cerradaE;TRG=TD@td:guarda+;CND=IN{entrada}!;ACC={STD@td:guardaC}<
```

**Notes**

- Los 5 dígitos del STD son SEGUNDOS. El timer se declara con D (no cuenta desde el boot) y las reglas lo arrancan con R con tiempo explícito y lo frenan con C. Sin el C en las reglas de TD+, el timer se auto-recarga y re-alarma cada N segundos para siempre.
- La salida tiene que estar configurada aparte como salida (SCXXP): p.ej. >SCXXP02900090000000< deja la sirena fija hasta que la apaguen; para un buzzer intermitente >SCXXP02000000010004< (100 ms ON / 400 ms OFF). SSSXP<idx2>11 enciende y SSSXP<idx2>00 apaga.
- Antirrebote: si el contacto de la puerta rebota, mandar además el SIT de la entrada (ver evento-entrada-digital), p.ej. >SIT0000030003< para 300 ms. La receta no lo incluye para no pisar un antirrebote ya ajustado.
- Qué es 'abierta' depende del cableado: si el sensor pone la entrada en 0 al abrir, los flancos y las CND van al revés (IN+ ↔ IN-, IN ↔ IN!).

## Ralentí (motor encendido y detenido más de N minutos)

<a id="ralenti"></a>

**id:** `ralenti`

Detecta ralentí: contacto dado y velocidad por debajo de un umbral (default 5 km/h) durante más de N segundos (default 300 = 5 min). Un flag U queda en 1 mientras dura el ralentí, y se reporta el inicio y el fin. Un timer + un umbral + cinco reglas.

- **Requires hardware:** none (`?` = optional)
- **Uses modules:** td, vl
- **Footprint:** 5 rules, 1 timers, 1 vl, 1 flags
- **Example validated on:** zero, spider, smart
- **Modes:** basic, advanced

| Parameter | Default | Format | Meaning |
|---|---|---|---|
| `velQuieto` | `5` | `4d` | Velocidad (km/h) por debajo de la cual se considera detenido. |
| `guardaSeg` | `300` | `5d` | Segundos detenido con contacto antes de declarar ralentí. |
| `evInicio` | `09` |  | Evento inicio de ralentí (dos dígitos hexa, 00-FF). La mayoría de las plataformas los convierte a decimal (00-FF → 0-255: 31 → 49); Wialon en tramas ER los muestra tal cual. |
| `evFin` | `10` |  | Evento fin de ralentí. La mayoría de las plataformas los convierte a decimal (00-FF → 0-255: 31 → 49); Wialon en tramas ER los muestra tal cual. |

**Resolved example**

```
>STD04D0030000000<
>SVL01E00051<
>SRL10E;TRG=IGNVL01!&+;ACC={STD04R}<
>SRL11E;TRG=IGNVL01!&-;ACC={STD04C}{SSSU0600}<
>SRL12E;TRG=TD04+;ACC={STD04C}{SSSU0611}<
>SRL14E;TRG=U06+;ACC={GER09H;@LOG}<
>SRL15E;TRG=U06-;ACC={GER10H;@LOG}<
```

**Template** (placeholders `@type:name`, parameters `{name}`)

```
>STD@td:guardaD{guardaSeg}00000<
>SVL@vl:quietoE{velQuieto}1<
>SRL@rl:armarE;TRG=IGN@vl:quieto!&+;ACC={STD@td:guardaR}<
>SRL@rl:desarmarE;TRG=IGN@vl:quieto!&-;ACC={STD@td:guardaC}{SSSU@u:ralenti00}<
>SRL@rl:ralentiE;TRG=TD@td:guarda+;ACC={STD@td:guardaC}{SSSU@u:ralenti11}<
>SRL@rl:inicioE;TRG=@u:ralenti+;ACC={GER{evInicio}H;@LOG}<
>SRL@rl:finE;TRG=@u:ralenti-;ACC={GER{evFin}H;@LOG}<
```

**Notes**

- Los TRG compuestos van en RPN: IGN VL! & significa 'contacto Y NO en movimiento', y el flanco final (+/-) es el del resultado.
- Si el equipo tiene ignición virtual o el contacto es por otra señal, cambiar IGN por la señal que corresponda en las dos reglas.
- Los 5 dígitos del STD son SEGUNDOS. La guarda se declara con D y las reglas la arrancan con R sin tiempo (usa guardaSeg) y la frenan con C, obligatorio en la regla de TD+ porque los TD se auto-recargan.

## Reinicio programado del equipo cada N horas

<a id="reinicio-programado"></a>

**id:** `reinicio-programado`

Reinicia el equipo cada N segundos de funcionamiento (default 28800 = 8 h) con un timer periódico y {SRESET}. Se usa en instalaciones donde el módem o el enlace se degradan con los días y un reinicio preventivo evita perder el equipo. Un timer + una regla.

- **Requires hardware:** none (`?` = optional)
- **Uses modules:** td
- **Footprint:** 1 rules, 1 timers
- **Example validated on:** zero, spider, smart
- **Modes:** basic, advanced

| Parameter | Default | Format | Meaning |
|---|---|---|---|
| `periodoSeg` | `28800` | `5d` | Segundos entre reinicios (28800 = 8 h; máximo 99999 = 27,7 h). Cuenta desde cada arranque. |

**Resolved example**

```
>STD22E2880000000<
>SRL90E;TRG=TD22+;ACC={SRESET}<
```

**Template** (placeholders `@type:name`, parameters `{name}`)

```
>STD@td:reinicioE{periodoSeg}00000<
>SRL@rl:reiniciarE;TRG=TD@td:reinicio+;ACC={SRESET}<
```

**Notes**

- Un reinicio corta lo que esté en curso: se pierde la sesión del chofer (hay que volver a pasar la llave), la cola de reportes en vivo que no salió y cualquier timer arrancado con R. Lo del log diferido (@LOG) se conserva. Conviene un período largo y, si la flota usa chofer, evaluar si vale la pena.
- Para que el reinicio no caiga en medio de un viaje, condicionarlo: CND=IGN! en la regla (reinicia sólo con el contacto quitado; si al vencer hay contacto, el timer periódico vuelve a intentarlo al siguiente período).
- El timer periódico E persiste en flash y cuenta desde el boot: los 5 dígitos del STD son SEGUNDOS, así que el tope es 99999 s (27,7 h). Para períodos más largos, un horario TR (a una hora fija) con la misma regla.

## Señal y reporte de corte gradual (GC) en curso

<a id="reporte-corte-gradual-gc"></a>

**id:** `reporte-corte-gradual-gc`

El módulo GC (corte gradual de combustible/motor) no tiene una señal TRG= nativa para 'estoy cortando ahora': solo se consulta con QGC, que devuelve status:disabled|ready|active. Esta receta sintetiza esa señal con un UV tipo A que compara el fragmento 'active' del status, y la usa para reportar a la plataforma el inicio y el fin del corte. La receta NO configura ni activa el módulo GC (eso ya lo tiene el cliente con su propia SGCE/SGCA) — solo lee su estado.

- **Requires hardware:** none (`?` = optional)
- **Uses modules:** gc (corte gradual ya habilitado con SGCE en el equipo destino)
- **Footprint:** 2 rules, 1 uvs
- **Example validated on:** zero, spider, smart
- **Modes:** basic, advanced

| Parameter | Default | Format | Meaning |
|---|---|---|---|
| `evInicio` | `59` |  | Evento (hex 00-FF) del reporte al arrancar el corte (mapear en plataforma, ej. 'Corte de combustible iniciado'). La mayoría de las plataformas (Wialon incluida en CQ) los convierte a decimal (00-FF → 0-255: 31 → 49). |
| `evFin` | `5A` |  | Evento (hex) del reporte al terminar/cancelarse el corte. La mayoría de las plataformas (Wialon incluida en CQ) los convierte a decimal (00-FF → 0-255: 31 → 49). |
| `destino` | `` |  | Sufijo de destino del reporte. Vacío = TX viva DST_ALL (inmediata, best-effort). ';@LOG' = agregar además copia en log diferido (persiste sin cobertura). |

**Resolved example**

```
>SUV00AQGC,\3B1,7,6,active,<
>SRL50E;TRG=UV00+;ACC={GCQ59L}<
>SRL51E;TRG=UV00-;ACC={GCQ5AL}<
```

**Template** (placeholders `@type:name`, parameters `{name}`)

```
>AQGC,\3B1,7,6,active,<
>SRL@rl:inicioE;TRG=UV@uv:corteActivo+;ACC={GCQ{evInicio}L{destino}}<
>SRL@rl:finE;TRG=UV@uv:corteActivo-;ACC={GCQ{evFin}L{destino}}<
```

**Notes**

- UV00 en el resolvedExample es solo el binding sugerido: verificar que el índice UV y los slots RL estén libres en la config destino antes de reubicar (en básico, reglas custom van desde RL50).

## Reporte en paralelo a una segunda plataforma (@BACK)

<a id="reporte-espejo-segunda-plataforma"></a>

**id:** `reporte-espejo-segunda-plataforma`

Manda una copia de los reportes de posición, cambio de rumbo e ignición al destino de RESPALDO (IP4/IP5) sin tocar lo que ya va al principal: cada regla reporta con ;@BACK. Sirve para migrar de plataforma con las dos en paralelo, o para darle datos a un segundo sistema (el propio, uno del cliente). Un timer + cuatro reglas.

- **Requires hardware:** none (`?` = optional)
- **Uses modules:** td
- **Footprint:** 4 rules, 1 timers
- **Example validated on:** zero, spider, smart
- **Modes:** basic, advanced

| Parameter | Default | Format | Meaning |
|---|---|---|---|
| `periodoSeg` | `30` | `5d` | Segundos entre posiciones al destino de respaldo (con contacto). |
| `metros` | `100` | `5d` | Metros recorridos que también disparan el timer (0 = sólo por tiempo). |
| `senalRumbo` | `DC00+` |  | Señal de cambio de rumbo ya configurada en la config (DC00+ es la de fábrica). |
| `evPos` | `00` |  | Evento de posición periódica (dos dígitos). La mayoría de las plataformas los convierte a decimal (00-FF → 0-255: 31 → 49); Wialon en tramas ER los muestra tal cual. |
| `evIgnOn` | `01` |  | Evento contacto dado. La mayoría de las plataformas los convierte a decimal (00-FF → 0-255: 31 → 49); Wialon en tramas ER los muestra tal cual. |
| `evIgnOff` | `02` |  | Evento contacto quitado. La mayoría de las plataformas los convierte a decimal (00-FF → 0-255: 31 → 49); Wialon en tramas ER los muestra tal cual. |

**Resolved example**

```
>STD10E0003000100<
>SRLF0E;TRG=TD10+;CND=IGN;ACC={GER00H;@BACK}<
>SRLF1E;TRG=DC00+;CND=IGN;ACC={GER00H;@BACK}<
>SRLF2E;TRG=IGN+;ACC={GER01H;@BACK}<
>SRLF3E;TRG=IGN-;ACC={GER02H;@BACK}<
```

**Template** (placeholders `@type:name`, parameters `{name}`)

```
>STD@td:espejoE{periodoSeg}{metros}<
>SRL@rl:periodicoE;TRG=TD@td:espejo+;CND=IGN;ACC={GER{evPos}H;@BACK}<
>SRL@rl:rumboE;TRG={senalRumbo};CND=IGN;ACC={GER{evPos}H;@BACK}<
>SRL@rl:ignOnE;TRG=IGN+;ACC={GER{evIgnOn}H;@BACK}<
>SRL@rl:ignOffE;TRG=IGN-;ACC={GER{evIgnOff}H;@BACK}<
```

**Notes**

- El destino de respaldo tiene que estar configurado: IP4/IP5 (>SIP4"host",puerto< y >SIP5…<), el protocolo del respaldo en SCXIPP (segunda letra: P = TAIP UDP, W = Wialon IPS, etc.) y el keep-alive hacia B si el respaldo lo necesita (>SKA60,B<). Sin IP4/IP5 los reportes con @BACK no van a ningún lado.
- El tipo de reporte (GER, GCQ, GBQ, GU<n>) tiene que ser el que espera la segunda plataforma; se cambia en las cuatro reglas. Para Wialon, un reporte de usuario con la trama Wialon.
- Variante 'reapuntar': si en vez de espejar se quiere que el respaldo pase a ser el principal, es otro procedimiento (SCXIPP, SIP0/SIP1, y >SCXDLE< para cancelar los envíos pendientes al cambiar de protocolo).
- Los 5 dígitos de tiempo y los 5 de metros del STD son fijos: STD<idx2>E<seg5><metros5>. Con metros 00000 el timer es sólo por tiempo.

## Reporte periódico del nivel de combustible de varillas BLE (sensor BS, Escort TD)

<a id="reporte-nivel-varillas-ble"></a>

**id:** `reporte-nivel-varillas-ble`

Manda cada N segundos (default 5 min) un reporte de usuario TAIP con la posición y, por cada varilla de combustible Bluetooth (tanque), el nivel crudo (fl1, fl2...) y la edad del dato en segundos (ag1, ag2...): ;AP=fl1:1:2997,ag1:1:3,fl2:1:3105,ag2:1:2. Cubre de 1 a 4 varillas (tanques) con un solo reporte. La edad deja ver en plataforma si la varilla dejó de transmitir (sube hasta 255) y un campo vacío dice que no se la escuchó desde el arranque, así un corte de Bluetooth no se confunde con un tanque vacío. Las varillas tienen que estar ya vinculadas en sus slots SBS con time=0. Para la alerta de robo usar robo-combustible-varilla-ble.

- **Requires hardware:** ble (`?` = optional)
- **Uses modules:** bs (varillas ya vinculadas, time=0 — ver notas), uc, td, cxdl? (solo si el destino es ;@LOG)
- **Footprint:** 1 rules, 1 timers, 1 ucs
- **Example validated on:** zero, spider, smart
- **Modes:** basic, advanced

| Parameter | Default | Format | Meaning |
|---|---|---|---|
| `varillas` | `0` | `2d` | Slots BS de las varillas, separados por coma, en el orden de los tanques: "0" una varilla, "0,1,2" tres tanques (hasta 4: 0 a 3). El primero sale como fl1/ag1, el segundo como fl2/ag2, etc. Verificar en qué slot está cada una con >QBS00<, >QBS01<... |
| `tipo` | `BQ` |  | Familia de la trama base: BQ = posición + batería interna (la más usada en TAIP), CQ = posición sola, HQ = + horómetro. Va en $R{tipo} y en Q{tipo}; el recorte de 5 vale para toda la familia. |
| `periodoSeg` | `300` | `5d` | Cada cuántos segundos sale el reporte (300 = 5 min; máximo 99999). Cuenta desde el arranque. |
| `evento` | `30` |  | Número de evento del reporte (dos dígitos hexa, 00-FF). Mapearlo en la plataforma. La mayoría de las plataformas los convierte a decimal (00-FF → 0-255: 31 → 49); Wialon lo hace en CQ/EQ y en BQ/ER/HQ/HV/CV los muestra tal cual. |
| `destino` | `;@LOG` |  | ';@LOG' = log diferido en flash: sin cobertura se guarda y sale al volver la señal (no se pierde la curva de nivel). Vacío = TX viva DST_ALL (best-effort). |

**Resolved example**

```
>SUC01 $RBQ| #| QBQ,5,100| $;AP=fl1:1:| QBS00.fuel_lvl| $,ag1:1:| QBS00.age<
>STD10E0030000000<
>SRL60E;TRG=TD10+;ACC={GU130L;@LOG}<
```

**Template** (placeholders `@type:name`, parameters `{name}`)

```
>SUC@uc:nivel $R{tipo}| #| Q{tipo},5,100| $;AP={varillas}<
>STD@td:periodoE{periodoSeg}00000<
>SRL@rl:reporteE;TRG=TD@td:periodo+;ACC={GU@uc:nivel{evento}L{destino}}<
```

**Notes**

- Ejemplo con tres tanques (varillas="0,1,2"): >SUC01 $RBQ| #| QBQ,5,100| $;AP=fl1:1:| QBS00.fuel_lvl| $,ag1:1:| QBS00.age| $,fl2:1:| QBS01.fuel_lvl| $,ag2:1:| QBS01.age| $,fl3:1:| QBS02.fuel_lvl| $,ag3:1:| QBS02.age< → la trama llega como >RBQ30<fecha, posición, velocidad, E/S, tensión, odómetro, GPS, módem, batería>;AP=fl1:1:2997,ag1:1:3,fl2:1:3105,ag2:1:2,fl3:1:1840,ag3:1:4;#002A;ID=601234;*38<. Con cuatro tanques el UC ocupa 210 de los 234 caracteres.
- Por qué QBS y no 'MBS(n;0)': ver el comentario del ítem UC. MBS sirve en un UV o una condición, pero en el reporte confunde 'no escuché la varilla' con 'tanque vacío con dato recién leído'.
- El reporte de usuario se ejecuta con UN dígito hexa ({GU130L}) y se define con dos (SUC01): la receta usa el primer UC libre, no el 00 fijo (Wialon lo usa).
- Si la instalación duerme (ULP/KO) el timer y el escaneo BLE se pausan: no hay reportes mientras duerme. Para reportar sólo en marcha, agregar CND=IGN a la regla.
- Complemento natural: robo-combustible-varilla-ble (alerta en el momento de una caída brusca, también con time=0 en la SBS). Las dos recetas conviven sin conflicto.

## Lector RFID por serie: reportar solo cuando cambia la tarjeta

<a id="rfid-serial-solo-cambios"></a>

**id:** `rfid-serial-solo-cambios`

Un lector RFID externo conectado al puerto serie reenvía la misma trama cada pocos segundos mientras la tarjeta sigue presente (típico: cada 3 s). El capturador CXXB deja la última trama en su buffer, pero NO compara con la captura anterior: pone status=3 en cada cierre de trama, así que la señal XBnn+ dispara también con las repetidas. Esta receta sintetiza el 'cambió la tarjeta' con un UV tipo X configurado con ventana 0 — que compara el valor contra la lectura inmediatamente anterior — y reporta el tag una sola vez por tarjeta, hasta que aparezca una distinta. La receta NO configura el lector: solo captura lo que este emite.

- **Requires hardware:** lector RFID con salida serie ASCII conectado a un puerto serie libre del equipo (`?` = optional)
- **Uses modules:** cxxb, cxdl? (solo si el destino es ;@LOG)
- **Footprint:** 1 rules, 1 uvs, 1 ucs, 1 cxxb
- **Example validated on:** zero, spider, smart
- **Modes:** basic, advanced

| Parameter | Default | Format | Meaning |
|---|---|---|---|
| `fuente` | `S` |  | Medio del que captura el CXXB. 'S' = puerto serie (el caso de esta receta). El firmware acepta además G (GPS), M (módem), B (bluetooth), D (debug) y C (CAN); '-' deshabilita el capturador. |
| `puerto` | `1` |  | idxSource del CXXB: 0 = serial de programación, 1 = externo, 2 = interno. Es el puerto donde está cableado el lector. Solo aplica con fuente 'S'. |
| `header` | `` |  | Caracteres de inicio de trama (hasta 8, escapables como \XX en hex). Vacío = arranca a capturar con el primer byte. Usar si el lector antepone algo que NO es parte del ID (ej. 'ID:') — todo lo que quede DENTRO de la captura entra al valor. |
| `footer` | `\0D\0A` |  | Caracteres de fin de trama (hasta 8, escapables). \0D\0A = CR+LF. \03 = ETX (lectores tipo RDM6300, que abren con \02). |
| `rxChars` | `000` |  | Cierra la captura al llegar a N caracteres (000 = no usar). Alternativa al footer para tramas de largo fijo sin delimitador. |
| `rxTimeout` | `000` |  | Tiempo máximo para completar UNA trama. OJO: está en MILISEGUNDOS, no en centésimas, y el máximo real es 254 (ver notes). 000 = deja los 250 ms por defecto, de sobra para una trama corta. |
| `largoTag` | `20` |  | Máximo de caracteres del tag a incluir en el reporte. Si el tag es más corto se manda entero; no hace falta ajustarlo salvo que quieras recortarlo a propósito. |
| `evento` | `00` |  | Número de evento del reporte (dos dígitos hexa, 00-FF). La mayoría de las plataformas los convierte a decimal (00-FF → 0-255: 31 → 49); Wialon lo hace en CQ/EQ y en BQ/ER/HQ/HV/CV los muestra tal cual. |
| `destino` | `;@LOG` |  | Destino del reporte. ';@LOG' = log diferido en flash (persiste sin cobertura). Vacío = TX viva DST_ALL (best-effort, se pierde sin cobertura). |

**Resolved example**

```
>SCXXB00S1000000,,\0D\0A,A,0<
>SUV00XEVAL MXB(0),0,20,0,*1<
>SUC00 QXB00,5,20<
>SRL50E;TRG=UV00+;ACC={GU000L;@LOG}<
```

**Template** (placeholders `@type:name`, parameters `{name}`)

```
>SCXXB@xb:lector{fuente}{puerto}{rxChars}{rxTimeout},{header},{footer},A,0<
>XEVAL MXB(@xb:lector),0,20,0,*1<
>SUC@uc:tag QXB@xb:lector,5,{largoTag}<
>SRL@rl:tagNuevoE;TRG=UV@uv:tagCambio+;ACC={GU@uc:tag{evento}L{destino}}<
```

**Notes**

- VARIANTE PARA TAGS HEXA O ALFANUMÉRICOS — lista fija de tarjetas, un UV tipo A por tag (comparación exacta por strcmp, sin depender de atof). UV es nivel sostenido, así que queda alto mientras esa tarjeta siga en el buffer y las repetidas no generan flanco. Techo 48 tarjetas. El mismo UC00 sirve para todas porque lee el tag del buffer: >SCXXB00S1000000,\02,\03,A,0< / >SUV00AQXB00,5,10,1A2B3C4D5E,< / >SUV01AQXB00,5,10,5E6F7A8B9C,< / >SUC00 QXB00,5,20< / >SRL50E;TRG=UV00+;ACC={GU000L;@LOG}< / >SRL51E;TRG=UV01+;ACC={GU000L;@LOG}<. El 5 saltea 'RXB00' y el 10 es el largo del tag; la coma final del UV es obligatoria.
- Los slots del resolvedExample son solo el binding sugerido: verificar que el índice XB, el UV, el UC y el slot RL estén libres en la config destino (en básico, las reglas custom van desde RL50). Si el equipo entra en bajo consumo (ULP/KO) el motor de reglas se pausa y el UV no evalúa hasta despertar.

## Alerta de robo de combustible por CAN/OBD (nivel ECU)

<a id="robo-combustible-can"></a>

**id:** `robo-combustible-can`

Detecta caídas bruscas del nivel de combustible leído por CAN/OBD (ECU02 = fuel_pct) con un UV tipo X: ventana fija que acumula el cambio NETO y evalúa al vencer. Robo con motor en marcha → alerta dentro de la ventana. Robo estacionado (motor apagado, la ECU no responde) → el valor queda RETENIDO en ecuTable y la alerta sale al primer vencimiento de ventana después de arrancar. Incluye un segundo UV de recarga (+N) como evento informativo que además ayuda a descartar falsos (caída+subida cercanas = pendiente/estacionamiento en subida). El oleaje que se recupera dentro de la ventana se cancela solo (neto ≈ 0).

- **Requires hardware:** can (`?` = optional)
- **Uses modules:** cxcan (protocolo configurado y ECU02 entregando fuel_pct), uv
- **Footprint:** 2 rules, 2 uvs
- **Example validated on:** spider, smart
- **Modes:** advanced

| Parameter | Default | Format | Meaning |
|---|---|---|---|
| `ecuIdx` | `02` |  | Índice ECU del nivel: 02 = fuel_pct (% del tanque, OBD-II y J1939). En camiones con litros custom puede convenir 04/20 (litros) — el umbral pasa a litros. |
| `umbralPct` | `10` |  | Caída (puntos de % del tanque) que dispara dentro de la ventana. 10 = 4,5 L en tanque de 45 L. Regla práctica: ≤ mitad del robo mínimo a asegurar (peor caso: robo repartido justo entre dos ventanas consecutivas). |
| `ventanaSeg` | `120` |  | Ventana del delta de caída en segundos. El robo en marcha se alerta al vencer; el estacionado, en la primera ventana tras arrancar. |
| `umbralRecargaPct` | `10` |  | Subida (puntos de %) que reporta el evento de recarga. |
| `ventanaRecargaSeg` | `300` |  | Ventana del delta de recarga (una carga en surtidor entra cómoda en 5 min). |
| `evRobo` | `45` |  | Número de evento del reporte de robo (mapear en plataforma como 'Robo de combustible'). La mayoría de las plataformas (Wialon incluida en CQ) los convierte a decimal (00-FF → 0-255: 31 → 49). La mayoría de las plataformas los convierte a decimal (00-FF → 0-255: 31 → 49); Wialon lo hace en CQ/EQ y en BQ/ER/HQ/HV/CV los muestra tal cual. |
| `evRecarga` | `46` |  | Número de evento del reporte de recarga. La mayoría de las plataformas (Wialon incluida en CQ) los convierte a decimal (00-FF → 0-255: 31 → 49). La mayoría de las plataformas los convierte a decimal (00-FF → 0-255: 31 → 49); Wialon lo hace en CQ/EQ y en BQ/ER/HQ/HV/CV los muestra tal cual. |
| `destino` | `` |  | Sufijo de destino del reporte. Vacío = TX viva DST_ALL (inmediata, best-effort). ';@LOG' = SOLO log diferido en flash (persiste pero NO llega en el momento). Para alerta antirrobo dejar vacío; si querés copia persistente, agregá una segunda acción con ;@LOG. |

**Resolved example**

```
>SUV01XQECU02,\2C1,0,6,120,-10,<
>SUV02XQECU02,\2C1,0,6,300,+10,<
>SRL50E;TRG=UV01+;CND=IGN;ACC={GCQ45H}<
>SRL51E;TRG=UV02+;ACC={GCQ46L}<
```

**Template** (placeholders `@type:name`, parameters `{name}`)

```
>XQECU{ecuIdx},\2C1,0,6,{ventanaSeg},-{umbralPct},<
>XQECU{ecuIdx},\2C1,0,6,{ventanaRecargaSeg},+{umbralRecargaPct},<
>SRL@rl:roboE;TRG=UV@uv:caida+;CND=IGN;ACC={GCQ{evRobo}H{destino}}<
>SRL@rl:recargaE;TRG=UV@uv:recarga+;ACC={GCQ{evRecarga}L{destino}}<
```

**Notes**

- PASO 0: requiere Spider o Smart (Zero no tiene CAN) con CXCAN andando. Verificar con >QECU02< en marcha que responde 'RECU02,<pct>'. Vacío o 'RCXCAN NA' → revisar CXCAN antes de seguir.
- El consumo normal jamás dispara: un auto quema <1 punto de % por ventana de 2 min. El valor OBD además viene filtrado por la ECU (menos oleaje que un aforador crudo).
- La alerta sale por TX viva (best-effort). ';@LOG' la mandaría SOLO al log diferido: persiste sin cobertura pero no es inmediata. Si el vehículo opera en zonas sin señal, duplicar: ACC={GCQ45H}{GCQ45L;@LOG}.
- Complemento recomendado: sumar el nivel al reporte periódico UC (campo fuel_pct:2:'ECU(2;0)') — la curva en plataforma es la evidencia del robo y el respaldo si la alerta local no dispara.
- Si el cliente necesita enterarse EN EL MOMENTO con el vehículo apagado (la ECU no responde estacionado), la respuesta es hardware: varilla BLE → receta robo-combustible-varilla-ble.

## Alerta de robo de combustible por varilla BLE (sensor BS)

<a id="robo-combustible-varilla-ble"></a>

**id:** `robo-combustible-varilla-ble`

Detecta caídas bruscas del nivel medido por una varilla Bluetooth (sensor BS, ej. Escort TD Fuel) con un UV tipo X sobre QBS<slot>.fuel_lvl. Como la varilla transmite SIEMPRE (independiente de la ignición), el robo estacionado con motor apagado se alerta EN EL MOMENTO — es la diferencia clave con la receta robo-combustible-can, que lo detecta recién al arrancar. Un UV de nivel sobre .age actúa de gate de frescura (CND): sin señal BLE no hay alertas falsas. Incluye UV de recarga (+N) informativo. El umbral va en las unidades CRUDAS de la varilla y se calibra con una lectura real.

- **Requires hardware:** ble (`?` = optional)
- **Uses modules:** bs (varilla ya vinculada en un slot BS, con time=0 — ver notas), uv
- **Footprint:** 2 rules, 3 uvs
- **Example validated on:** zero, spider, smart
- **Modes:** advanced

| Parameter | Default | Format | Meaning |
|---|---|---|---|
| `bsSlot` | `00` |  | Slot BS donde ya está vinculada la varilla (verificar con >QBS00<, >QBS01<...). La receta NO configura la SBS: referencia la existente. |
| `umbralUnidades` | `50` |  | Caída en unidades CRUDAS de la varilla que dispara. Calibrar: unidades/L = lectura de >QBS<slot>.fuel_lvl< ÷ litros presentes; umbral = litros a detectar × unidades/L. Regla práctica: ≤ mitad del robo mínimo a asegurar (peor caso repartido entre dos ventanas). |
| `ventanaSeg` | `300` |  | Ventana del delta de caída en segundos. 5 min cubre el sifón típico (2-5 L/min) con margen. |
| `maxEdadSeg` | `60` |  | Edad máxima del dato BLE (segundos) para considerarlo fresco. El campo age satura en 255. Debe ser MENOR que cualquier timeout que tenga la SBS (ideal: SBS con time=0). |
| `umbralRecargaUnidades` | `50` |  | Subida en unidades crudas que reporta el evento de recarga. |
| `ventanaRecargaSeg` | `600` |  | Ventana del delta de recarga (los tanques grandes tardan en cargarse). |
| `evRobo` | `45` |  | Número de evento del reporte de robo (mapear en plataforma). La mayoría de las plataformas (Wialon incluida en CQ) los convierte a decimal (00-FF → 0-255: 31 → 49). La mayoría de las plataformas los convierte a decimal (00-FF → 0-255: 31 → 49); Wialon lo hace en CQ/EQ y en BQ/ER/HQ/HV/CV los muestra tal cual. |
| `evRecarga` | `46` |  | Número de evento del reporte de recarga. La mayoría de las plataformas (Wialon incluida en CQ) los convierte a decimal (00-FF → 0-255: 31 → 49). La mayoría de las plataformas los convierte a decimal (00-FF → 0-255: 31 → 49); Wialon lo hace en CQ/EQ y en BQ/ER/HQ/HV/CV los muestra tal cual. |
| `destino` | `` |  | Sufijo de destino del reporte. Vacío = TX viva DST_ALL (inmediata, best-effort). ';@LOG' = SOLO log diferido (persiste pero no inmediato). |

**Resolved example**

```
>SUV01XQBS00.fuel_lvl,0,6,300,-50<
>SUV02UQBS00.age,0,3,0,60<
>SUV03XQBS00.fuel_lvl,0,6,600,+50<
>SRL50E;TRG=UV01+;CND=UV02;ACC={GCQ45H}<
>SRL51E;TRG=UV03+;ACC={GCQ46L}<
```

**Template** (placeholders `@type:name`, parameters `{name}`)

```
>XQBS{bsSlot}.fuel_lvl,0,6,{ventanaSeg},-{umbralUnidades}<
>UQBS{bsSlot}.age,0,3,0,{maxEdadSeg}<
>XQBS{bsSlot}.fuel_lvl,0,6,{ventanaRecargaSeg},+{umbralRecargaUnidades}<
>SRL@rl:roboE;TRG=UV@uv:caida+;CND=UV@uv:fresco;ACC={GCQ{evRobo}H{destino}}<
>SRL@rl:recargaE;TRG=UV@uv:recarga+;ACC={GCQ{evRecarga}L{destino}}<
```

**Notes**

- PASO 0: verificar la varilla con >QBS<slot>.fuel_lvl< (valor crudo) y >QBS<slot>.age< (segundos desde la última trama). Con eso mismo se calibra el umbral: anotar lectura y litros presentes.
- Prueba de campo: motor apagado, extraer del tanque algo más que el umbral calibrado → la alerta debe llegar dentro de la ventana (≤5 min con el default). Tapar/alejar la varilla → NO debe alertar (gate + retención). Cargar combustible → evento de recarga dentro de su ventana.
- La alerta sale por TX viva (best-effort). Para copia persistente sin cobertura: ACC={GCQ45H}{GCQ45L;@LOG}. Complemento: sumar MBS(<slot>;0) al reporte periódico UC para tener la curva de nivel en plataforma.
- Si el vehículo tiene CAN/OBD y no varilla, usar robo-combustible-can (misma lógica, detección estacionada diferida al arranque).

## Prender y apagar una salida mandando una palabra por SMS

<a id="salida-por-sms-palabra-clave"></a>

**id:** `salida-por-sms-palabra-clave`

Activar o desactivar una salida (corte, sirena, traba, buzzer) con un mensaje de texto que lleva una palabra, por ejemplo ACTIVA y DESACTIVAR, para que lo pueda mandar alguien que no va a escribir un comando. No existe un comando dedicado de 'códigos por SMS': se arma con el capturador CXXB escuchando al módem, un disparador UV tipo A por palabra (compara texto exacto) y una regla por palabra que mueve la salida. Alternativa sin configurar nada: el SMS puede llevar directamente el comando (>SSSXP0011< prende la salida 0), y ese camino SÍ respeta la lista blanca de números (SCXSMW + SSM); esta receta NO.

- **Requires hardware:** línea con SMS habilitado en el chip (`?` = optional)
- **Uses modules:** cxxb
- **Footprint:** 2 rules, 2 uvs, 1 cxxb
- **Example validated on:** zero, spider, smart
- **Modes:** basic, advanced

| Parameter | Default | Format | Meaning |
|---|---|---|---|
| `fuente` | `M` |  | Medio del que captura el CXXB. 'M' = módem, que es por donde llega el texto del SMS. No cambiar para esta receta. |
| `palabraOn` | `ACTIVA` |  | Texto del SMS que prende la salida. Se compara LETRA POR LETRA, mayúsculas incluidas. Sin comas ni espacios. |
| `largoOn` | `6` |  | Cantidad de letras de palabraOn. Tiene que ser EXACTA: es el largo del tramo que se compara (ACTIVA = 6). Si no coincide, nunca dispara. |
| `palabraOff` | `DESACTIVAR` |  | Texto del SMS que apaga la salida. Mismas reglas que palabraOn. |
| `largoOff` | `10` |  | Cantidad de letras de palabraOff (DESACTIVAR = 10). |
| `salida` | `00` |  | Salida a mover, dos dígitos. El Zero y el PI tienen una sola (00); Spider y Smart, 00 a 02. |
| `posicion` | `5` |  | Desde qué carácter de la respuesta de QXB se compara. 5 saltea el prefijo 'RXBnn'. ES LO QUE HAY QUE AJUSTAR EN BANCO: con fuente M el capturador recibe también lo que el módem dice alrededor del SMS (aviso de mensaje, número, fecha), así que la palabra puede no empezar justo ahí. |

**Resolved example**

```
>SCXXB01M0000000,,,A,0<
>SUV01AQXB01,5,6,ACTIVA,<
>SUV02AQXB01,5,10,DESACTIVAR,<
>SRL00E;TRG=UV01+;ACC={SSSXP0011}<
>SRL01E;TRG=UV02+;ACC={SSSXP0000}<
```

**Template** (placeholders `@type:name`, parameters `{name}`)

```
>SCXXB@xb:modem{fuente}0000000,,,A,0<
>AQXB@xb:modem,{posicion},{largoOn},{palabraOn},<
>AQXB@xb:modem,{posicion},{largoOff},{palabraOff},<
>SRL@rl:prendeE;TRG=UV@uv:palabraOn+;ACC={SSSXP{salida}11}<
>SRL@rl:apagaE;TRG=UV@uv:palabraOff+;ACC={SSSXP{salida}00}<
```

**Notes**

- SEGURIDAD: el capturador ve lo que manda el módem ANTES del filtro de lista blanca (SCXSMW) y de la contraseña, que sólo protegen el camino del SMS-comando (modemSmsParse). Con esta receta, CUALQUIERA que mande la palabra dispara la regla. Si lo que se mueve es delicado (un corte de motor), recomendar el SMS con el comando >SSSXP0011< y lista blanca, o por lo menos una palabra que no se adivine.
- La palabra se compara exacta: 'activa' en minúsculas o 'ACTIVA ' con un espacio al final NO disparan. Conviene avisarle a quien va a mandar los SMS.
- El disparador tipo A es un nivel sostenido: mientras lo último capturado siga siendo la palabra, el UV sigue activo y la regla no vuelve a disparar hasta que llegue otra cosa. Mandar dos veces seguidas ACTIVA dispara una sola vez; alternar ACTIVA / DESACTIVAR funciona.
- Cada UV se refresca por turnos (round-robin de 50 ms por disparador): con muchos UV cargados la reacción al SMS puede demorar unos segundos.

## Score de conducción por viaje (frenadas, aceleradas, excesos, over-rev)

<a id="score-conduccion-viaje"></a>

**id:** `score-conduccion-viaje`

Enriquece los viajes con el comportamiento del chofer para que el score device-side del módulo TRIP sea valioso. Cada frenada/aceleración brusca (HM), exceso de velocidad (VL) y over-rev de RPM (UV sobre el CAN) emite un evento geolocalizado en vivo y además incrementa el contador de viaje correspondiente (GC0-GC4). Al cerrar el viaje, STP,STOP emite el resumen (R1: max/prom, chofer, GC0-7) y el score (R10: rating + conteos). El over-rev usa un TD gateado por los flancos del UV para contar tiempo sobre redline sin rebote (histéresis gratis) — sin flag ni segundo UV. Incluye el esqueleto de viaje (tracking + inicio/fin).

- **Requires hardware:** can? (`?` = optional)
- **Uses modules:** trip, hm, vl, uv, td
- **Footprint:** 11 rules, 2 timers, 1 uvs, 2 hm, 1 vl
- **Example validated on:** zero, spider, smart
- **Modes:** advanced

| Parameter | Default | Format | Meaning |
|---|---|---|---|
| `destino` | `@BACK` |  | Destino de reportes/tramas de viaje (;@BACK backup, ;@LOG flash, ;@GP GPRS). El STP,STOP debe ir al mismo destino que ya recibís. |
| `accelMag` | `7` |  | Aceleración brusca: umbral en km/h·s (GPS). 1 km/h·s≈0.028g; 7≈0.2g. Campo 2 del SHM tipo 0. |
| `accelMinSpeed` | `5` |  | Velocidad mínima GPS (km/h) para habilitar la detección de aceleración. Campo 3 del SHM. |
| `brakeMag` | `9` |  | Frenada brusca: umbral en km/h·s (GPS). 9≈0.25g. Campo 2 del SHM tipo 1. |
| `brakeMinSpeed` | `10` |  | Velocidad mínima GPS (km/h) para habilitar la detección de frenada. |
| `hmWindow` | `1000` |  | Ventana de sostenimiento HM en ms. 1000 = dispara al 2º segundo válido (con muestreo GPS de 1 s). NO subir a 3000-4000: ahí no dispara (ver notas). |
| `hmCooldown` | `5` |  | Cooldown HM en segundos entre eventos consecutivos. |
| `velLimit` | `120` | `4d` | Umbral de exceso de velocidad en km/h. Se emite como SVL<idx>E<velLimit:4d>1 (filtrado=1). |
| `rpmLimit` | `4000` |  | Umbral de over-rev (RPM). Nafta ~4000-4500; DIÉSEL más bajo ~2800-3200. Es el campo min del UV. |
| `rpmTimeout` | `15` |  | Timeout del QECU en SEGUNDOS: si el CAN no da RPM válido en ese lapso, devuelve 0 (no dispara). NO es ms. |
| `overrevPeriod` | `10` | `5d` | Período del TD de over-rev en segundos = 'peso por tiempo'. Cada N s continuos sobre redline suma 1 a GC1. Se configura con STD<idx>D<overrevPeriod:5d> (D = deshabilitado + persiste). |
| `evTrack` | `00` |  | Evento de punto de tracking (hex 00-FF) La mayoría de las plataformas los convierte a decimal (00-FF → 0-255: 31 → 49); Wialon en tramas ER los muestra tal cual. |
| `evStart` | `01` |  | Evento de inicio de viaje (hex) La mayoría de las plataformas los convierte a decimal (00-FF → 0-255: 31 → 49); Wialon en tramas ER los muestra tal cual. |
| `evEnd` | `02` |  | Evento de fin de viaje (hex) La mayoría de las plataformas los convierte a decimal (00-FF → 0-255: 31 → 49); Wialon en tramas ER los muestra tal cual. |
| `evAccel` | `31` |  | Evento en vivo de aceleración brusca (hex, rango propio para no pisar 00/01/02) La mayoría de las plataformas los convierte a decimal (00-FF → 0-255: 31 → 49); Wialon en tramas ER los muestra tal cual. |
| `evBrake` | `30` |  | Evento en vivo de frenada brusca (hex) La mayoría de las plataformas los convierte a decimal (00-FF → 0-255: 31 → 49); Wialon en tramas ER los muestra tal cual. |
| `evSpeed` | `14` |  | Evento en vivo de exceso de velocidad (hex; 0x14 = 20 dec, default del configurador) La mayoría de las plataformas los convierte a decimal (00-FF → 0-255: 31 → 49); Wialon en tramas ER los muestra tal cual. |
| `trackSeg` | `30` | `5d` | Período del punto de tracking en segundos (5 dígitos). |
| `trackMetros` | `100` | `5d` | Distancia que también dispara el punto de tracking, en metros (5 dígitos, 00000 = sólo por tiempo). |

**Resolved example**

```
>STD10E0003000100<
>STD11D00010<
>SHM00E,0,7,5,0,1000,5<
>SHM01E,1,9,10,0,1000,5<
>SVL00E01201<
>SUV00UQECU00:15:0,\2C1,0,6,4000,65535,<
>SRLF0E;TRG=TD10+;CND=IGN;ACC={GER00H;@BACK}<
>SRLF1E;TRG=DC00+;CND=IGN;ACC={GER00H;@BACK}<
>SRLF2E;TRG=IGN+;ACC={GER01H;@BACK}{STP,START}<
>SRLF3E;TRG=IGN-;ACC={GER02H;@BACK}{STD10R}<
>SRLF4E;TRG=IGN-;ACC={STP,STOP;@BACK}<
>SRL60E;TRG=HM00+;ACC={GER31H;@BACK}{STP,GCI,2}<
>SRL61E;TRG=HM01+;ACC={GER30H;@BACK}{STP,GCI,3}<
>SRL62E;TRG=VL00+;ACC={GER14H;@BACK}{STP,GCI,0}<
>SRL63E;TRG=UV00+;ACC={STD11R}<
>SRL64E;TRG=UV00-;ACC={STD11C}<
>SRL65E;TRG=TD11+;ACC={STP,GCI,1}<
```

**Template** (placeholders `@type:name`, parameters `{name}`)

```
>STD@td:trackE{trackSeg}{trackMetros}<
>STD@td:overrevD{overrevPeriod}00000<
>SHM@hm:accelE,0,{accelMag},{accelMinSpeed},0,{hmWindow},{hmCooldown}<
>SHM@hm:brakeE,1,{brakeMag},{brakeMinSpeed},0,{hmWindow},{hmCooldown}<
>SVL@vl:speedE{velLimit}1<
>UQECU00:{rpmTimeout}:0,\2C1,0,6,{rpmLimit},65535,<
>SRL@rl:trackPerE;TRG=TD@td:track+;CND=IGN;ACC={GER{evTrack}H;{destino}}<
>SRL@rl:trackHeadE;TRG=DC00+;CND=IGN;ACC={GER{evTrack}H;{destino}}<
>SRL@rl:tripStartE;TRG=IGN+;ACC={GER{evStart}H;{destino}}{STP,START}<
>SRL@rl:tripEndEvtE;TRG=IGN-;ACC={GER{evEnd}H;{destino}}{STD@td:trackR}<
>SRL@rl:tripCloseE;TRG=IGN-;ACC={STP,STOP;{destino}}<
>SRL@rl:accelE;TRG=HM@hm:accel+;ACC={GER{evAccel}H;{destino}}{STP,GCI,2}<
>SRL@rl:brakeE;TRG=HM@hm:brake+;ACC={GER{evBrake}H;{destino}}{STP,GCI,3}<
>SRL@rl:speedE;TRG=VL@vl:speed+;ACC={GER{evSpeed}H;{destino}}{STP,GCI,0}<
>SRL@rl:rpmUpE;TRG=UV@uv:rpm+;ACC={STD@td:overrevR}<
>SRL@rl:rpmDownE;TRG=UV@uv:rpm-;ACC={STD@td:overrevC}<
>SRL@rl:rpmCountE;TRG=TD@td:overrev+;ACC={STP,GCI,1}<
```

**Notes**

- HM por default 'no disparan': los tipos GPS (0=accel, 1=freno) muestrean la velocidad 1 vez/seg y exigen delta sostenido en segundos consecutivos hasta juntar la ventana; con ventana 3000-4000 ms hace falta acelerar/frenar sostenido ~4-5 s (imposible en la práctica) y la histéresis SUMA al umbral. Por eso hysteresis=0 y ventana=1000 ms. Comando: SHM<idx>E,<tipo>,<mag km/h·s>,<velMín>,<hyst%>,<ventana ms>,<cooldown s>. Para picos cortos reales usar IMU (tipos 6/7, ventana 0, umbral ~350-400 mg) en equipos con acelerómetro calibrado.
- El evEnd/evStart/evTrack (00/01/02) y los eventos de conducción (30/31/14, y 33 si agregás over-rev en vivo) se mapean a texto en la plataforma. Reservá un rango propio para conducción para no pisar los de tracking.
- OJO: el original del cliente reseteaba STD00R en el fin de viaje; acá se reinicia el timer de tracking (STD10R = @td:track). Si tu STD00 es otro timer con propósito propio, ajustá el binding.
- Sin CAN, el over-rev (GC1) no cuenta: QECU devuelve 0 → UV en 0. El resto (frenadas/aceleradas/velocidad) funciona por GPS. RPM no tiene fallback GPS.

## Sincronizar hora por red

<a id="sincronizar-hora-red"></a>

**id:** `sincronizar-hora-red`

Al iniciar, verifica si el equipo ya tiene una hora valida. Si no la tiene, arranca un timer de reintento y, cuando haya WiFi o celular disponible, ejecuta SRTC AUTO cada minuto hasta que el RTC quede valido.

- **Requires hardware:** none (`?` = optional)
- **Uses modules:** wifi?, celular?
- **Footprint:** 2 rules, 1 timers, 1 uvs
- **Example validated on:** zero, spider, smart
- **Modes:** advanced

| Parameter | Default | Format | Meaning |
|---|---|---|---|
| `retrySeg` | `60` | `5d` | Intervalo entre verificaciones de hora (seg) |
| `rtcMin` | `1` | `int` | Valor minimo aceptado para el campo de hora/fecha consultado por QCQ |
| `rtcMax` | `1000000` | `int` | Valor maximo aceptado para el campo de hora/fecha consultado por QCQ |

**Resolved example**

```
>SUV00UQCQ,5,6,1,1000000<
>STD15D0006000000<
>SRL90E;TRG=PWR+;CND=UV00!;ACC={STD15R00060}<
>SRL91E;TRG=TD15+;CND=WIFCEL|UV00!&;ACC={SRTC AUTO}{STX RTC ;@TRM}<
```

**Template** (placeholders `@type:name`, parameters `{name}`)

```
>UQCQ,5,6,{rtcMin},{rtcMax}<
>STD@td:retryRtcD{retrySeg}00000<
>SRL@rl:startRtcSyncE;TRG=PWR+;CND=@uv:rtcOk!;ACC={STD@td:retryRtcR{retrySeg}}<
>SRL@rl:syncRtcE;TRG=TD@td:retryRtc+;CND=WIFCEL|@uv:rtcOk!&;ACC={SRTC AUTO}{STX RTC ;@TRM}<
```

**Notes**

- La condicion de RL91 se evalua izquierda a derecha, sin precedencia: WIFCEL|UV00!& equivale a (WIF OR CEL) AND NOT UV00.
- El timer queda deshabilitado en la plantilla y se reinicia desde RL90 solo si al boot el RTC no esta valido.
- El borrador original usaba STD15D00030 / STD15R00030. Para reintentar cada 30 segundos, cambiar retrySeg a 30.
- Si UV00 ya esta ocupado en la config destino, reubicar @uv:rtcOk y reemplazar todas sus referencias internas.
- Si TD15, RL90 o RL91 ya estan ocupados, reubicar @td:retryRtc, @rl:startRtcSync o @rl:syncRtc respectivamente.

## Límites de velocidad por zona con buzzer

<a id="velocidad-por-zona-buzzer"></a>

**id:** `velocidad-por-zona-buzzer`

Buzzer (salida digital) que avisa exceso de velocidad con tres límites según dónde esté el vehículo: un límite general fuera de zonas, uno más bajo dentro de una región (RG) y uno aún más bajo dentro de un waypoint (WP). Cada límite tiene histéresis ON/OFF para que el buzzer no titile, reporta un evento al cruzar y se silencia al volver por debajo. Todo gateado por ignición.

- **Requires hardware:** xp (`?` = optional)
- **Uses modules:** vl, wp, rg
- **Footprint:** 6 rules, 6 vl
- **Example validated on:** spider, smart
- **Modes:** advanced

| Parameter | Default | Format | Meaning |
|---|---|---|---|
| `velGeneral` | `110` | `4d` | Límite general fuera de zonas (km/h) — ON |
| `velGeneralOff` | `108` | `4d` | Apagado general (km/h): un poco por debajo del ON, para que el buzzer no titile al rondar el límite. |
| `velRegion` | `40` | `4d` | Límite dentro de la región RG (km/h) — ON |
| `velRegionOff` | `38` | `4d` | Apagado dentro de la región (km/h). |
| `velWaypoint` | `20` | `4d` | Límite dentro del waypoint WP (km/h) — ON |
| `velWaypointOff` | `18` | `4d` | Apagado dentro del waypoint (km/h). |
| `buzzerXp` | `2` |  | Índice de la salida del buzzer (0-9). Encender: SSSXP0<xp>11, apagar: SSSXP0<xp>00 |
| `zonaRegion` | `RG00` |  | Geocerca de la región (límite medio). Su geometría se define aparte |
| `zonaWaypoint` | `WP00` |  | Geocerca del waypoint (límite bajo). Su geometría se define aparte |
| `ignSig` | `IN07` |  | Señal de ignición usada en las CND (IN07 físico o IGN lógico) |
| `evGenOn` | `53` |  | Evento exceso general ON (hex 00-FF) La mayoría de las plataformas los convierte a decimal (00-FF → 0-255: 31 → 49); Wialon en tramas ER los muestra tal cual. |
| `evGenOff` | `54` |  | Evento general OFF (hex) La mayoría de las plataformas los convierte a decimal (00-FF → 0-255: 31 → 49); Wialon en tramas ER los muestra tal cual. |
| `evRegOn` | `55` |  | Evento región ON (hex) La mayoría de las plataformas los convierte a decimal (00-FF → 0-255: 31 → 49); Wialon en tramas ER los muestra tal cual. |
| `evRegOff` | `56` |  | Evento región OFF (hex) La mayoría de las plataformas los convierte a decimal (00-FF → 0-255: 31 → 49); Wialon en tramas ER los muestra tal cual. |
| `evWpOn` | `57` |  | Evento waypoint ON (hex) La mayoría de las plataformas los convierte a decimal (00-FF → 0-255: 31 → 49); Wialon en tramas ER los muestra tal cual. |
| `evWpOff` | `58` |  | Evento waypoint OFF (hex) La mayoría de las plataformas los convierte a decimal (00-FF → 0-255: 31 → 49); Wialon en tramas ER los muestra tal cual. |

**Resolved example**

```
>SVL00E01101<
>SVL01E01081<
>SVL02E00401<
>SVL03E00381<
>SVL04E00201<
>SVL05E00181<
>SCXXP02000000010004<
>SRL44E;TRG=VL00+;CND=WP00!RG00!&IN07&;ACC={GER53H;@LOG}{SSSXP0211}<
>SRL45E;TRG=VL01-;CND=WP00!RG00!&;ACC={GER54H;@LOG}{SSSXP0200}<
>SRL54E;TRG=VL02+;CND=RG00IN07&;ACC={GER55H;@LOG}{SSSXP0211}<
>SRL55E;TRG=VL03-;CND=RG00;ACC={GER56H;@LOG}{SSSXP0200}<
>SRL64E;TRG=VL04+;CND=WP00IN07&;ACC={GER57H;@LOG}{SSSXP0211}<
>SRL65E;TRG=VL05-;CND=WP00;ACC={GER58H;@LOG}{SSSXP0200}<
```

**Template** (placeholders `@type:name`, parameters `{name}`)

```
>SVL@vl:genOnE{velGeneral}1<
>SVL@vl:genOffE{velGeneralOff}1<
>SVL@vl:regOnE{velRegion}1<
>SVL@vl:regOffE{velRegionOff}1<
>SVL@vl:wpOnE{velWaypoint}1<
>SVL@vl:wpOffE{velWaypointOff}1<
>SRL@rl:genOnE;TRG=VL@vl:genOn+;CND={zonaWaypoint}!{zonaRegion}!&{ignSig}&;ACC={GER{evGenOn}H;@LOG}{SSSXP0{buzzerXp}11}<
>SRL@rl:genOffE;TRG=VL@vl:genOff-;CND={zonaWaypoint}!{zonaRegion}!&;ACC={GER{evGenOff}H;@LOG}{SSSXP0{buzzerXp}00}<
>SRL@rl:regOnE;TRG=VL@vl:regOn+;CND={zonaRegion}{ignSig}&;ACC={GER{evRegOn}H;@LOG}{SSSXP0{buzzerXp}11}<
>SRL@rl:regOffE;TRG=VL@vl:regOff-;CND={zonaRegion};ACC={GER{evRegOff}H;@LOG}{SSSXP0{buzzerXp}00}<
>SRL@rl:wpOnE;TRG=VL@vl:wpOn+;CND={zonaWaypoint}{ignSig}&;ACC={GER{evWpOn}H;@LOG}{SSSXP0{buzzerXp}11}<
>SRL@rl:wpOffE;TRG=VL@vl:wpOff-;CND={zonaWaypoint};ACC={GER{evWpOff}H;@LOG}{SSSXP0{buzzerXp}00}<
```

**Notes**

- VL es recurso de instancia (indexado 0-15) y se reubica igual que RL/TD: cada par ON/OFF a dos slots VL libres, y su regla apunta al mismo índice. Se agregó el placeholder @vl a la tabla del README.
- Histéresis: el umbral OFF va unos km/h por debajo del ON (110/108, 40/38, 20/18, parámetros *Off). Sin separación el buzzer titila al rondar el límite. En el firmware VL emite el flag de filtrado = 1: SVL<idx2>E<vel4>1.
- Zonas: WPxx/RGxx son geocercas que definís aparte con su geometría; la receta solo referencia el índice. El nivel general aplica fuera de ambas: CND = WPxx!RGxx!& (RPN: ¬WP ∧ ¬RG).
- ignSig: IN07 = ignición física en el ejemplo. Si usás la señal lógica IGN, reemplazá IN07 por IGN en TODAS las CND (no mezcles ambas en la misma config).
- Reportes con destino LOG (;@LOG) → el evento de exceso queda persistido en flash y se reenvía aunque no haya cobertura. Cambiá a BUF (sin sufijo) si lo querés solo en vivo. LOG es el destino por defecto del configurador.
- Sin dependencia de orden entre las 6 reglas (no hay reglas conversoras de pulso).
