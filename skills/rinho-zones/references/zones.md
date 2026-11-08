# Zones reference

Sources: public docs "Zonas — Catálogo masivo de geocercas"
(https://docs.rinho.com.ar/guias/zonas) and "Comando ZONES"
(https://docs.rinho.com.ar/comandos/configuracion-consulta-procesos-y-calculos-referentes-al-gps/comando-zones).
When in doubt, those pages win.

## Numbering

Every zone has a flat **id 0..511**:

| id | Zone | Name in rules (`TRG=`, `CND=`, `>QSS..<`) | Read with |
|---|---|---|---|
| 0..99 | ZNA 0..99 | `ZNA00`..`ZNA99` (two decimal digits) | rules, `>QSSZNAnn<`, `EVAL ZNA(n)` |
| 100..255 | ZNA 100..255 | none | `EVAL ZNA(n)` only |
| 256..355 | ZNB 0..99 | `ZNB00`..`ZNB99` | rules, `>QSSZNBnn<`, `EVAL ZNB(n)` |
| 356..511 | ZNB 100..255 | none | `EVAL ZNB(n)` only |

The index is **decimal**: id 42 is `ZNA42`, id 298 is `ZNB42`. Never write hex (`ZNA2A` is wrong).

## Limits

| | |
|---|---|
| Shapes per file | 4096 |
| Logical zones | 512 (ZNA 0-255, ZNB 0-255) |
| Usable in rules | ZNA00-99 and ZNB00-99 |
| Evaluation | once per second |
| Firmware | 1.09.21 or later |

A shape can belong to several zones; a zone can contain many shapes (it is "inside" while the
device is inside any of them).

## KML format

One `<Placemark>` per shape. Zones and sizes go in `<ExtendedData>`:

```xml
<Placemark>
  <name>Depósito central</name>
  <ExtendedData>
    <Data name="type"><value>WP</value></Data>          <!-- WP | POLY | ROUTE, optional: inferred -->
    <Data name="zones"><value>0,5,272</value></Data>     <!-- flat ids 0..511, comma separated -->
    <Data name="radius_m"><value>150</value></Data>      <!-- WP only, default 100 -->
  </ExtendedData>
  <Point><coordinates>-58.4173,-34.6118,0</coordinates></Point>   <!-- lon,lat,alt -->
</Placemark>
```

| Geometry | Type | Rules |
|---|---|---|
| `<Point>` | WP (circle) | one point, `radius_m` |
| `<LineString>` | ROUTE | 2+ points, `buffer_m` = side margin in meters (default 50) |
| `<Polygon><outerBoundaryIs>` | POLY | 3+ vertices, closed automatically. Holes are ignored |

If a Placemark has several geometries (Google Earth often adds a centroid point to a polygon),
the polygon wins over the line, and the line over the point.

KMZ (zipped KML): `rinho.mjs zones assign` and `zones list` read it directly.

The file format (`RZN1`, CRC32) is produced by `rinho.mjs zones build`, the same compiler Rinho uses
in its own tools.

## Device commands

| Command | What it does |
|---|---|
| `>SZFRAC<n><` | Memory reserved for zones: 2, 4, 8 or 16 (smaller = more space). Applies the next time zones are enabled |
| `>SZMODE1<` | Enables zones. Reboots the device if the mode changes. Requires the internal log to be empty (all records sent) |
| `>SZMODE1F<` | Same, forcing it with log pending (pending records are lost) |
| `>SZONES"http://..."<` | Downloads (plain HTTP GET, no https) and validates the zones file → `RZONES,START`, then `RZONES,OK,...,elem=..` or `RZONES,ERR,<reason>` |
| `>SZONESC<` | Cancels a download in progress |
| `>SZONESD<` | Deletes the zones file |
| `>QZONES<` | `RZONES,LOADED,...,elem=..` (shapes loaded), `RZONES,EMPTY` or `RZONES,DISABLED` (zones not enabled) |
| `>QZMODE<` | `RZMODE,<enabled>,<fraction>,<log sectors>,<zone sectors>` |
