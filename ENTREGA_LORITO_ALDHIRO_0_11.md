# Entrega — Lorito Aldhiro 0.11.001 → 0.11.007 (Órdenes Golden) → 0.11.008 (pieles SVG) → 0.11.009 (LCD en cuadrícula) → 0.11.010 (LCD: overlays de CRAC) → 0.11.011 (LCD para Lorito: carretes que giran) → 0.11.012 (Medidor de Intensidad de Señal LCD)

| Artefacto | SHA-256 |
|---|---|
| `lorito_aldhiro_v0_11_001_release_candidate_0.11.tar.gz` (partida, sin cambios) | `a18f4a89f56999acd180805fad8140061facec77f6b6fe2f144c53eeae31ce48` |
| `lorito_aldhiro_v0_11_007_golden_0.11.tar.gz` (cinco Órdenes Golden) | `e48910c1e77905486a502e097f7e8acc2ac57b27db7ffb21f527e0e762d88fa4` |
| `lorito_aldhiro_v0_11_008_golden_0.11.tar.gz` (+ pieles SVG) | `b527c158e2a663122989c34ef7a3cc6268d83c0cbb79ae6e79ffa429f1e89eb0` |
| `lorito_aldhiro_v0_11_009_golden_0.11.tar.gz` (+ LCD Display en cuadrícula angular) | `48d2958e1499f83ea7c8d8bd1f43de16a9ac729ba69d0a2ef3656fbfc90e8b10` |
| `lorito_aldhiro_v0_11_010_golden_0.11.tar.gz` (+ leyendas centradas, celdas PREVI/TAU/COSANI/EXPORT, overlays del VFD de CRAC, carretes, fuente alarm clock) | `3f3bddb6e4149079a1d119c5d71fd9008e9b18e9b748d15e0e4473e7bffda319` |
| `lorito_aldhiro_v0_11_011_golden_0.11.tar.gz` (+ overlays solo de Lorito, letra fija, carretes vectoriales que giran, corazón, hubs retirados) | `eb3bdc6e50b92d0173298a86d156841c899bb731ca3625e01e4c4c6c54a26ad7` |
| `lorito_aldhiro_v0_11_012_golden_0.11.tar.gz` (**vigente**: + Medidor de Intensidad de Señal LCD en lugar de la onda bamboleante) | `98d68dbbaa8c1dc5257a443b5a25d8c8eeeeea77f85880a32e7ac0cbac1a1d79` |

El tarball nuevo es un snapshot íntegro del árbol, con raíz plana como el de
partida y sin bytecode. Aplica las cinco órdenes adjuntas, todas con NO-GO
inicial. Cada motor viejo se sustituye por el que describe su orden:

| Orden | Motor nuevo | Documento principal |
|---|---|---|
| Captura (CAP-001…012) | ring SPSC sin pérdida silenciosa, FrameLedger, FSM de sesión, cierre en fases con fsync | `CAPTURE_ARCHITECTURE_GOLDEN.md` |
| Exportación (EX-01…14) | `ExportService` como única ruta, finalización durable y verificada; ruta por array eliminada | `EXPORT_CONTRACT.md` |
| DSP PREVI/NR | contrato único, reconfiguración transaccional, invariancia de bloque, banco de fidelidad | `DSP_CHAIN_SPEC.md`, `NR_FIDELITY_BENCH.md` |
| Instrumentación/ITL | medidor dBFS por frames, envolventes por canal, reloj de cinta desde frames, bitácora forense | `ITL_TELEMETRY_GOLDEN.md` |
| Scrubs/Velocista/Seek (SMV-01…14) | Transport Core como autoridad única, transacciones con generación, cursor absoluto, transición por operación | `SCRUBS_TRANSPORT_ARCHITECTURE_GOLDEN.md` |

**Verificación**: 743/743 pruebas, 0 fallos, en plano y con
`-W error::ResourceWarning`. En la partida eran 598.

**Estado por hallazgo y puertas pendientes**: `ORDENES_GOLDEN_0_11_ESTADO.md`,
dentro del tarball. Ninguna orden se declara GOLDEN: faltan soak, loopback de
hardware, comparación con CRAC 0.36, escucha ciega, referencias Dolby/dbx y
firmas.

Detalle técnico: `CHANGELOG.md` [079]–[084] y `gaps/GAP-076.md`…`GAP-081.md`.

## 0.11.008 — Pieles SVG del lienzo

- **Retirado el fondo de madera pintado con Qt.** Fallaba en cada
  repintado: importaba `QPointF` desde `QtGui`, cuando vive en `QtCore`.
  Se sustituye por completo por el SVG de madera.
- Los SVG se adaptan al tamaño real de cada widget y a las leyendas de
  Lorito:

| SVG | Componente de Lorito |
|---|---|
| Metaloric Panel (Vitreous Panel en CRAC) | visor de señal + panel de estado + duración de cinta |
| LCD Display (VFD en CRAC) | contómetro ámbar, LED y botones de transporte |
| Panascrub | los 3 scrubs (Escaneo, Torcedura, Velocista) |
| Vúmetros | REC VU y PLAY VU, con escala dBFS |

- **Corregido un defecto del Transport Core**: los comandos aplicados con
  el ring lleno no llegaban a CONFIRMED.
- **Verificación**: 756/756 pruebas, en plano y con
  `-W error::ResourceWarning`.
- **Capturas**: en [`capturas_0_11_008/`](capturas_0_11_008/). Empezar
  por `00_comparativa_antes_despues.png`. Detalle técnico:
  `gaps/GAP-082.md`, dentro del tarball.

## 0.11.009 — LCD Display en cuadrícula angular

- Todas las celdas del LCD Display son ahora celdas de cuadrícula con
  esquinas rectas. Cada leyenda pequeña ocupa el ángulo superior derecho
  de su celda.
- El **contómetro principal** se ancla a la esquina inferior izquierda de
  su celda, con un padding separador de 10 px × 6 px medido sobre la tinta
  real del texto, de modo que no toca los bordes. El texto superpuesto
  sigue la misma regla y ya no desborda.
- La **celda de la onda bamboleante** se elevó verticalmente: pasó de 1/4
  a 1/2 del bloque superior.
- El **Media Anim** se redujo al 80 % de su celda.
- **Prueba SMV-T02 corregida**: su señal de impulsos de una muestra era
  inválida a ratio 1,6 y la prueba fallaba de forma intermitente. Ahora usa
  ráfagas de banda limitada. El motor no cambia.
- **Verificación**: 760/760 pruebas, en plano y con
  `-W error::ResourceWarning`. Las 4 pruebas nuevas de la cuadrícula se
  ejecutan con PySide6 real.
- **Capturas**: en [`capturas_0_11_009/`](capturas_0_11_009/). Empezar
  por `00_comparativa_lcd_antes_despues.png`. Detalle técnico:
  `gaps/GAP-083.md`, dentro del tarball.

## 0.11.010 — LCD Display: overlays del VFD de CRAC, carretes y fuente alarm clock

- **Leyendas centradas**: todas las leyendas de las celdas del LCD quedan
  centradas en su celda, en horizontal y en vertical.
- **Celdas nuevas**: PREVI y TAU arriba, COSANI y EXPORT abajo. Se
  encienden mientras haya un overlay de su familia en pantalla.
- **Overlays de CRAC**: los 213 overlays del VFD display de CRAC 0.37.512
  quedan registrados en el LCD, cada uno en su familia (PREVI 88,
  COSANI 76, TAU 37, EXPORT 4 y sistema 8).
  - Incluye los de SIPA, desborde de cuentavueltas, ITL y función no
    disponible que CRAC muestra en su VFD.
  - No se registran los mensajes internos ni los que van a CAROLAVENTOS
    ("TU CINTA ES MUY LARGA... UPS!!", "ITL — FIN DE CINTA",
    "AUTO-PAUSA — SILENCIO", "FUNCION NO DISPONIBLE"), ni "FnSide B": al
    final del Lado B va "FnTape Aldh".
- **Carretes de cassette**: el carrete y su pareja más pequeña, inclinados
  en horizontal, en una celda invisible del ángulo superior derecho de la
  megacelda del contómetro. No tocan ninguna leyenda.
- **Fuente del contómetro**: pasa a ser alarm clock, la del VFD de CRAC.
  Con la fuente anterior, la "V" de VerLcha se veía cortada.
- **Verificación**: 781/781 pruebas, en plano y con
  `-W error::ResourceWarning`. Las 10 que necesitan PySide6 real se
  ejecutaron aparte con Qt 6.12.
- **Capturas**: en [`capturas_0_11_010/`](capturas_0_11_010/). Detalle
  técnico: `gaps/GAP-084.md`, dentro del tarball.

## 0.11.011 — LCD Display para Lorito: carretes que giran

- **Overlays solo de Lorito**: el registro del LCD pasa de 213 overlays
  del VFD de CRAC a 44. Quedan solo los de funciones que Lorito tiene:
  - cinta, scrubs, ITL, SIPA y desborde;
  - reducción de ruido, HX Pro, MPX y monitor;
  - sus tres DSP PREVI;
  - exportación y Fnc No Disp.
  La API usa nombres de Lorito (`showLcdOverlay`, tokens `LCD_*`).
- **Letra fija**: el contómetro, a 56 px, es la referencia y los overlays
  usan ese mismo tamaño. Los que no caben se desplazan como marquesina en
  vez de achicarse.
- **Carretes sin aliasing**: tu carrete se dibuja ahora como forma
  vectorial con antialiasing.
- **Corazón negro** a la altura de los carretes, centrado bajo DOLBY S.
- **Los carretes orquestan el movimiento**: la cinemática de los hubs de
  los paneles de log y de duración de cinta se migró a los carretes, y esos
  hubs se retiraron.
- **Verificación**: 796/796 pruebas, en plano y con
  `-W error::ResourceWarning`. Las 39 que necesitan PySide6 real pasan con
  Qt 6.12.
- **Capturas**: en [`capturas_0_11_011/`](capturas_0_11_011/). Detalle
  técnico: `gaps/GAP-085.md`, dentro del tarball.

## 0.11.012 — Medidor de Intensidad de Señal LCD

- **Reemplaza la onda bamboleante** de la celda inferior del LCD Display,
  que era decorativa, por una medida real.
  - Barras negras rectangulares, de ancho y separación uniformes, sobre una
    base común y un fondo amarillo intenso uniforme, sin reflejos.
  - Un grupo de barras por canal (L/R); la más nueva entra por la derecha.
- **Motor `SignalIntensityMeterEngine`:**
  - Escala en dBFS (de −60 a 0), con calibración AES17.
  - Balística PPM investigada: ataque al 90 % en 10 ms y caída suave de
    20 dB en 1,7 s, sin retención de pico.
  - El silencio digital es un estado propio; la saturación se marca con
    SAT.
- **Sin tocar el audio:**
  - El hilo de audio no dibuja, no espera a Qt, no toca disco y no toma
    locks.
  - Publica una instantánea inmutable (capacidad 1).
  - REC mide antes del DSP; PLAY mide la salida audible.
- **Estados:** REC, PLAY, SILENCIO, PAUSA, STOP, SIN DATOS y ESPERA. En
  PAUSA y STOP no se muestra ninguna lectura vieja, y al cambiar entre REC
  y PLAY no se arrastra telemetría.
- **Rendimiento** (informe en `saneamiento_0_11_012/INFORME_MEDIDOR_0.11.012.md`):
  - Motor: 39 µs por bloque de 512 frames (0,37 % del bloque).
  - Widget: 1,2 ms por pintado (~2,9 % de CPU a 24 Hz).
  - Memoria: +1 176 B en 30 min.
  - Latencia visual de ≈ 50 ms en el peor caso.
  - 0 frames perdidos y 0 underruns.
- **Corrección preexistente:** el writer de disco contaba en un ledger
  viejo si se llamaba `set_output()` antes de `start()`. Era el fallo
  intermitente de la batería; ya está corregido y tiene prueba de
  regresión.
- **Verificación:**
  - 831/831 pruebas, en plano y con `-W error::ResourceWarning`.
  - Las 35 pruebas que necesitan Qt real pasan con PySide6 6.12.
  - El tarball extraído también pasa el manifiesto y la batería completa.
- **Capturas:** en [`capturas_0_11_012/`](capturas_0_11_012/). Detalle
  técnico en `gaps/GAP-086.md`, dentro del tarball.
