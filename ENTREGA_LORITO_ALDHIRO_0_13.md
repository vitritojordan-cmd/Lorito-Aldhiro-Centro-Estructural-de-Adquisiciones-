# Entrega — Lorito Aldhiro 0.13.001

| Artefacto | SHA-256 |
|---|---|
| [`lorito_aldhiro_v0_13_001_golden_0.13.tar.gz`](lorito_aldhiro_v0_13_001_golden_0.13.tar.gz) | `34209c74dde34a791220628d95c5ee20898c546e369962d5c5d5383e526c7f4d` |

La versión sube a **0.13** al cerrar la Orden LA-GOLDEN-VIEW-SIGSEGV-019. La 0.12 quedó concluida antes de empezar, como se pidió; está en [`ENTREGA_LORITO_ALDHIRO_0_12.md`](ENTREGA_LORITO_ALDHIRO_0_12.md).

## 0.13.001 — Celdas DSP/MASTER/C-90/OTROS, cuentavueltas de tambores ámbar y visor a prueba de SIGSEGV

### LCD: 10 celdas por fila

- **Arriba** se suman **DSP** y **MASTER**.
- **Abajo**, junto a C-60, van **C-90** y **OTROS**. Se enciende la celda del tipo de cinta elegido.
- **Reglas (de la usuaria):**
  - **DSP** solo con un DSP que no sea PREVI ni NR: Muscle EQ, Version-O-Tron, EQ Bias, MPX, Focus, HX Pro o ALC.
  - **MASTER** solo si la exportación es cruda: sin normalizar, sin cambio de velocidad y con códec sin pérdida (WAV, FLAC o WV). Lo confirma el resultado de la exportación.
  - **DITHER** solo con un dither elegido. Ningún motor aplica dither todavía: la celda refleja la elección.
  - **TAU** y **COSANI** siempre apagadas: son placeholders de 0.16.

### Cuentavueltas analógico (SVG de la usuaria)

- **Diseño:** 3 tambores ámbar con bisel, tornillos y vidrio. Conserva la física de giro de Lorito.
- **Montaje:** va sobre el panel de duración de cinta como control externo, con el criterio del Vitreous Subeventos de CRAC.
- **Acabado:** el bisel y el chasis son **idénticos encendido y apagado**; solo cambia el brillo de los tambores. Se quitó el zócalo que el panel pintaba detrás en captura y que dejaba una sombra a la izquierda.
- **Transporte:** sigue a la posición confirmada en PLAY y FF/REW, y se congela al detenerse. La captura tiene prioridad. La lectura se declara **estimada**, porque no hay sensor físico.

### Orden LA-GOLDEN-VIEW-SIGSEGV-019

Ver `VISOR_SIGSEGV_CUENTAVUELTAS_GOLDEN.md` dentro del tarball.

- **Auditoría V-01…V-10**, con archivo y línea.
- **La causa más grave:** la envolvente del visor usaba memoria proporcional a la duración: **494 MB con 20 min** y unos 1,5 GB por hora. Ahora usa **2,3 MB**, plano hasta 60 min, y es 15 veces más rápida.
- **Pipeline del visor:**
  - solicitud y snapshot inmutables y versionados;
  - worker con parada ordenada;
  - el slot verifica que el widget siga vivo;
  - las generaciones viejas se descartan.
- **Pintura transaccional:** ante un fallo, el visor muestra **VISOR NO DISPONIBLE** sin tocar la captura. El SHA-256 del RAW queda intacto.
- **Diagnóstico de fallos:** `faulthandler` y los excepthooks de hilos escriben en `crash_faulthandler.log`. Una prueba produce un SIGSEGV real y verifica el log.
- **Limitaciones declaradas:** ASan, UBSan y TSan no se pueden ejecutar con PySide6 y NumPy de la distribución. Tampoco se midió el P95 a 60 Hz en hardware de referencia; offscreen dio 6,2 ms.

### Pruebas

- **Suite completa:** 1005/1005, sin fallos y con 0 ResourceWarning.
- **Qt real:** 206/206.
- **Tarball:** verificado contra su manifiesto SHA-256, con la suite completa corrida sobre lo extraído.

### Capturas ([`capturas_0_13_001/`](capturas_0_13_001/))

| Archivo | Qué muestra |
|---|---|
| `00_comparativa_antes_despues.png` | 0.12.001 frente a 0.13.001 |
| `01_ventana_completa_2x.png` | Ventana completa |
| `02_lcd_celdas_dsp_master_c90_otros_2x.png` | Celdas nuevas |
| `03_cuentavueltas_transporte_2x.png` | Cuentavueltas en transporte |
| `04_cuentavueltas_captura_2x.png` | Cuentavueltas en captura, sin sombra |
| `05_visor_no_disponible_2x.png` | Visor degradado |

**Siguiente:** Orden LA-GOLDEN-CORE-020, que lleva a 0.14.
