# Entrega — Lorito Aldhiro 0.13.001

| Artefacto | SHA-256 |
|---|---|
| [`lorito_aldhiro_v0_13_005_golden_0.13.tar.gz`](lorito_aldhiro_v0_13_005_golden_0.13.tar.gz) | `a7f2488476fa621f21438270880d06850d63c74c9807da5b4cd81274e7cf7b4b` |
| [`lorito_aldhiro_v0_13_004_golden_0.13.tar.gz`](lorito_aldhiro_v0_13_004_golden_0.13.tar.gz) | `721211148630fee75e3a2c8ef95325af462ded5999e083aa0d5018cd8b773fc2` |
| [`lorito_aldhiro_v0_13_001_golden_0.13.tar.gz`](lorito_aldhiro_v0_13_001_golden_0.13.tar.gz) | `34209c74dde34a791220628d95c5ee20898c546e369962d5c5d5383e526c7f4d` |

La versión sube a **0.13** al cerrar la Orden LA-GOLDEN-VIEW-SIGSEGV-019. La 0.12 quedó concluida antes de empezar, como se pidió; está en [`ENTREGA_LORITO_ALDHIRO_0_12.md`](ENTREGA_LORITO_ALDHIRO_0_12.md).

## 0.13.005 — DITHER con BYPASS; carretes y corazón según el estado

### Menú DSP

- **APAGAR DITHER:** desaparece como interruptor del menú DSP.
- **Submenú DITHER:** queda con **TPDF (TRIANGULAR)** y **BYPASS**, que apaga el maquillador.
- **Posición del submenú:** como DSP está junto al borde derecho, su submenú ahora se abre a la izquierda y ya no tapa el menú.

### Carretes y corazón del LCD

| Estado | Carretes | Corazón |
|---|---|---|
| Reposo | apagados (atenuado de leyenda apagada) | apagado |
| Captura | encendidos | encendido |
| Pausa manual o automática (silencio) | **parpadean** entre encendido y atenuado (480 ms) | encendido |
| Reproducción y bobinado | encendidos | apagado (el corazón es solo de la captura) |

### Además

- **TAU:** cada ventana atiende solo los eventos de su propio servicio. Antes, con varias ventanas en el mismo proceso, los pitidos de una podían mover la leyenda de otra.

### Pruebas

- **Suite completa:** 1057/1057, con 0 ResourceWarning.
- **Qt real:** 258/258.
- **Tarball:** verificado contra su manifiesto y con la suite completa sobre lo extraído.

### Capturas ([`capturas_0_13_005/`](capturas_0_13_005/))

| Archivo | Qué muestra |
|---|---|
| `00_estados_carretes_corazon.png` | Los seis estados en una sola lámina |
| `01_menu_dsp_dither_bypass_2x.png` | DSP → DITHER con TPDF y BYPASS |

## 0.13.004 — Frecuencias de 44,1 a 192 kHz, motor Dither TPDF y TAU

Tres revisiones dentro de la línea 0.13: [095] frecuencia, [096] dither (LA-DTH-OI-001) y [097] TAU (LA-TAU-OI-001). El detalle técnico, la investigación y la matriz están en `FRECUENCIA_DITHER_TAU_GOLDEN.md` dentro del tarball.

### Frecuencia de muestreo

- **Menú MUESTRA:** 44100, 48000, 88200, 96000, 176400 y 192000 Hz.
- **Verificación con el dispositivo:** se hace antes de aplicar la frecuencia y antes de grabar. Si no la admite, muestra "Fnc No Disp" y no cambia nada.
- **Frecuencia del stream:** se comprueba que el stream abra a la frecuencia pedida.
- **Margen ante cortes de disco:** el colchón se mide en segundos. A 192 kHz pasa de 5,5 s / 2,7 s a 20 s / 10 s; a ≤ 48 kHz no cambia nada.
- **Contador del escritor de disco:** había una carrera, anterior a esta entrega, que dejaba el contador de la sesión un lote corto. El audio estaba completo en el disco. Quedó corregida y tiene su prueba.

### Motor Dither (DSP → DITHER)

- **Cuándo actúa:** TPDF de ±1 LSB, solo cuando la exportación **reduce la profundidad de bits** (WAV 16 o FLAC 16 desde el RAW de 24 bits). Es la última etapa antes de cuantizar. En los demás casos no añade nada.
- **Registro:** la semilla queda anotada en el manifiesto, así que cada exportación se puede reproducir.
- **Medidas:** 850× tiempo real; ruido de −96,3 dBFS (0,5 LSB RMS).
- **Menú:** TPDF y APAGAR. Se retiraron RECTANGULAR y SHAPED, que no hacían nada y la orden deja para más adelante.

### TAU

- **Pitidos:** 1 al activar y 2 al desactivar, solo cuando la acción ocurrió de verdad:
  - REC, PAUSA, PLAY, ◀◀, ▶▶, STOP y Space;
  - ALC, MPX, FOCUS y HX PRO;
  - DITHER.
- **Leyenda TAU del LCD:** se enciende mientras suena y no parpadea entre los dos pitidos. Se limpia siempre: al terminar, ante un fallo o al cerrar la ventana.
- **Valores provisionales** (la orden pide ajustarlos de oído): tono de 80 ms a 1500 Hz y −18 dBFS, pausa de 95 ms, rampas sin clic.

### Pruebas

- **Suite completa:** 1053/1053, sin fallos y con 0 ResourceWarning.
- **Qt real:** 254/254 (19 módulos).
- **Tarball:** verificado contra su manifiesto, con la suite completa corrida sobre lo extraído.

### Pendientes

- **Hardware:** este entorno no tiene dispositivo de audio, así que falta oír TAU y el dither en el hardware real y medir la latencia de la leyenda.
- **ALSA `hw` exclusivo:** con un dispositivo exclusivo, TAU puede no sonar mientras se reproduce. Falla de forma segura: sin leyenda.
- **MASTER a 16 bits:** falta decidir si una exportación a 16 bits SIN dither debe seguir encendiendo MASTER.

### Capturas ([`capturas_0_13_004/`](capturas_0_13_004/))

| Archivo | Qué muestra |
|---|---|
| `01_menu_muestra_2x.png`, `01b_menu_muestra_scroll_2x.png` | Las seis frecuencias |
| `02_menu_dsp_dither_2x.png` | DSP → DITHER |
| `03_lcd_tau_y_dither_2x.png` | TAU y DITHER encendidas |

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
