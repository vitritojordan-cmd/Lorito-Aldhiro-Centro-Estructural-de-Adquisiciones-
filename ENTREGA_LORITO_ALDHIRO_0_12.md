# Entrega — Lorito Aldhiro 0.12.001

| Artefacto | SHA-256 |
|---|---|
| [`lorito_aldhiro_v0_12_001_golden_0.12.tar.gz`](lorito_aldhiro_v0_12_001_golden_0.12.tar.gz) | `a71efa6c1b62885387892705ffd9c9b409b1de1e87dea8249e3be7f4f85a316b` |

La versión sube de la línea 0.11 a **0.12**, como se pidió, una vez cerrados todos los tratamientos. La línea anterior está en [`ENTREGA_LORITO_ALDHIRO_0_11.md`](ENTREGA_LORITO_ALDHIRO_0_11.md).

La línea siguiente está en [`ENTREGA_LORITO_ALDHIRO_0_13.md`](ENTREGA_LORITO_ALDHIRO_0_13.md).

## 0.12.001 — Panel fusionado, menús de 4 ítems, marco de madera, FF/REW Golden y normalización Golden

### Panel fusionado

- **Un solo vidrio** con el SVG de la usuaria.
- **Orden:** forma de onda → duración de cinta → log, separados solo por líneas finas.
- **Medidas:** cinta y log tienen la misma altura (37 px). El bloque mantiene sus 134 px.

### Barra de menús

- **Placa de aluminio flotante** con tornillos (SVG de la usuaria).
- **Botones:** 10 de 85 × 28 px, repartidos parejos de punta a punta. Ya no quedan pegados a la izquierda.
- **Persianas:**
  - muestran como máximo **4 opciones**;
  - si hay más, aparece una **barra de scroll metálica a la derecha**, que se usa con arrastre, clic en el riel, rueda o teclado;
  - los submenús abren otra persiana igual.

### Marco de madera

- El lienzo pasa de 1040×630 a **1072×674**.
- Hay **16 px de madera** alrededor de todo.
- Las **cuatro escuadras de latón** quedan a la vista, también las superiores.

### Orden LA-GOLDEN-TRANSPORT-018 — avance rápido y rebobinado

- **LCD:** muestra **"Ff Wd > X{n}"** y **"Rew < X{n}"**. El factor sube con cada pulsación: ×1, ×2, ×3, ×4 y vuelve a ×1.
- **Coordinador de transporte único:**
  - el cambio de sentido pasa por una parada confirmada;
  - se detiene exactamente en el inicio o en el fin de la cinta;
  - STOP espera la confirmación de parada;
  - la captura tiene prioridad;
  - el exportador espera a que la cinta se detenga;
  - la posición es en frames enteros, sin deriva.
- **Sonido:** no se oye nada durante el bobinado. Si se partió de PLAY, la reproducción se reanuda al parar.
- **Documento:** `TRANSPORTE_FFREW_GOLDEN.md` dentro del tarball, con la investigación citada.

### Orden LA-GOLDEN-NORM-ARCH-017 — normalización y archivo del legado

- **Medidor según ITU-R BS.1770-5 / EBU R128:**
  - LUFS con puertas;
  - true peak con sobremuestreo del Anexo 2;
  - pico de muestra y RMS.

  Un tono de referencia de −23 dBFS mide −23,0 LUFS.
- **Contrato Golden:**
  - objetivo, techo y ganancia máxima de +24 dB;
  - silencio sin ganancia;
  - NaN/Inf rechazados;
  - errores tipados;
  - estado ARCHIVAL_SAFE / DEGRADED / INVALID.
- **Limitador:** **se retiró el limitador oculto**. Solo se aplica cuando se elige, declarado como etapa.
- **Verificación de la salida:** se mide el archivo publicado.
- **Archivo del legado:** los motores de normalización en RAM pasaron a `legado/` con manifiesto SHA-256. Su equivalencia se sigue probando contra la copia archivada.
- **Documento:** `NORMALIZACION_ARCHIVO_GOLDEN.md` dentro del tarball.

### Verificación

| Corrida | Resultado |
|---|---|
| Plano | 969/969 OK (49 skips) |
| `-W error::ResourceWarning` | 969/969 OK, 0 avisos |
| PySide6 real | 170/170 OK |
| Copia extraída del tarball | Manifiesto SHA-256 OK y 969/969 OK |

### Capturas y detalle

- **Capturas:** en [`capturas_0_12_001/`](capturas_0_12_001/).
- **Detalle técnico:** `gaps/GAP-090.md`.
- **Informe de pruebas:** `saneamiento_0_12_001/TEST_REPORT_0.12.001.md`.

### Pendiente

- **Orden LA-GOLDEN-VIEW-SIGSEGV-019:** visor de onda, protección SIGSEGV y cuentavueltas. Irá en **0.13**.
- **Orden LA-GOLDEN-CORE-020:** modernización del núcleo. Irá en **0.14**.
