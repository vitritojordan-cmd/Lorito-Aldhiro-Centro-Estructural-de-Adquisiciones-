# Entrega — Lorito Aldhiro 0.14.001

*Lorito Aldhiro. Powered by Carola Records Aldhira Capture CRAC*

| Artefacto | SHA-256 |
|---|---|
| [`lorito_aldhiro_v0_14_001_golden_0.14.tar.gz`](lorito_aldhiro_v0_14_001_golden_0.14.tar.gz) — árbol completo (fuentes, pruebas, herramientas, documentación, legado) | `b569e68d90ce7aad2062d75fa0ba92ff6019cb5c3ef465c50a371feef9c0bbec` |
| [`lorito_aldhiro_v0_14_001_runtime.tar.gz`](lorito_aldhiro_v0_14_001_runtime.tar.gz) — paquete de producción (solo lo que la aplicación necesita para arrancar) | `88767d403704addb30b406a665bdc1af2207da9cabb6f2787ea9afb390934d25` |

La versión sube a **0.14** con la Orden LA-GOLDEN-CORE-020 (modernización integral del núcleo). La 0.13 quedó cerrada en 0.13.009: ver [`ENTREGA_LORITO_ALDHIRO_0_13.md`](ENTREGA_LORITO_ALDHIRO_0_13.md).

## 0.14.001 — Núcleo autónomo; CRAC como referencia histórica

### Lo que pediste

- **CRAC como referencia histórica.** Ningún módulo, archivo, log, hilo, mensaje ni recurso de Lorito nombra a CRAC. La historia se conserva entera en `legado/procedencia/`:
  - las 1.418 referencias que había en 0.13.009, cada una con su texto original, archivo, línea y SHA-256;
  - el changelog de CRAC anterior a la bifurcación;
  - el catálogo VFD de CRAC y de qué línea salió cada overlay del LCD;
  - el código reemplazado.
- **La atribución, tal cual:** "Lorito Aldhiro. Powered by Carola Records Aldhira Capture CRAC". Vive en una sola constante del código. Aparece en el README, en el archivo histórico y en `python3 lorito_bootstrap.py --version`.
- **AECCT2, solo en Media Anim:** es el título de la computadora animada, en la constante `MEDIA_ANIM_COMPUTER_CAPTION`. Una prueba vigila que no aparezca en ningún otro lado: logs, rutas, configuración, metadatos, nombres.
- **Investigación en internet:** no hay registro público de CRAC ni de "Aldhira Capture"; es software interno de Carola Records. La investigación sí sirvió para la parte técnica de la orden (detalle y fuentes en `MODERNIZACION_NUCLEO_GOLDEN.md` §2).

### Lo que cambia para quien usa Lorito

| Antes | Ahora |
|---|---|
| Exportación `aecct2_<fecha>.wav` + `…_aldhira_spec_crac.txt` | `lorito_<fecha>.wav` + `…_lorito_spec.txt` |
| "Aldhira de Especificación de CRAC" / "C.R.A.C Tape Counter" | "Aldhira de Especificación de Lorito" / "Lorito Tape Counter" |
| Títulos "ADVERTENCIA CRAC", "CRAC Save", "CIERRE ABRUPTO DE CRAC"… | "ADVERTENCIA LORITO", "LORITO SAVE", "CIERRE ABRUPTO DE LORITO"… |
| Log interno `aecct2_internal.log` | `lorito_internal.log` (el viejo queda donde está) |

- **Migración al arrancar:** los archivos de control de una sesión anterior (latido y manifiesto de recuperación de crash) pasan solos a los nombres nuevos, con respaldo. Una sesión que se cortó con 0.13 se recupera igual con 0.14.
- **Lo que no se toca:** capturas RAW, temporales, exportaciones ya hechas y sus especificaciones.

### Lo que se encontró y se corrigió

- **Dependencia faltante:** `scipy` no figuraba en `requirements.txt`, aunque lo usan los DSP PREVI. En una instalación limpia no habrían funcionado.
- **Carga desde el monolito:** la física de cinta intentaba cargar un motor del monolito de CRAC y, al no hallarlo, seguía con la suya sin avisar. Se retiró ese intento. La salida es idéntica a 0.13.009 (verificado con una traza de 1.406 muestras).
- **Ciclo de imports oculto:** había uno entre el LCD y los paneles SVG, y se rompió.
- **Capas invertidas:** un módulo del núcleo importaba la interfaz y pasó a la capa de interfaz.
- **Prueba muerta:** la prueba de los 20 perfiles de Lucha Enloquecida nunca se ejecutaba, porque dependía del árbol de CRAC. Ahora se ejecuta contra una referencia propia.

### Lo que queda pendiente de la orden

- Desacoplar la interfaz de los internos de los motores, mediante contratos de servicio.
- Retirar los singletons globales.
- Hacer una sesión larga con hardware de audio real.

Por eso la orden queda **certificada en parcial**. Detalle en `MODERNIZACION_NUCLEO_GOLDEN.md` §1 y §9.

### Pruebas

- **Suite completa:** 1107 pruebas OK (96 omitidas: necesitan Qt real o hardware), con 0 ResourceWarning.
- **Qt real:** 308/308. Incluye el arranque desde el paquete de producción con una carpeta de usuario vacía.
- **Nuevas:** 41 pruebas de la orden (`tests/test_core_independencia.py`): escáner, alcance de AECCT2, atribución, archivo histórico, migración, arquitectura, regresión de transporte y paquete.
- **Tarball:** verificado contra su manifiesto, con la suite completa corrida sobre lo extraído.
- **Paquete de producción:** reproducible (dos construcciones dan el mismo SHA-256), sin legado ni historia, 0 referencias prohibidas.
