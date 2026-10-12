# Entrega — Órdenes Golden de ingeniería sobre Lorito Aldhiro 0.11.001 → 0.11.007

| Artefacto | SHA-256 |
|---|---|
| `lorito_aldhiro_v0_11_001_release_candidate_0.11.tar.gz` (partida, sin cambios) | `a18f4a89f56999acd180805fad8140061facec77f6b6fe2f144c53eeae31ce48` |
| `lorito_aldhiro_v0_11_007_golden_0.11.tar.gz` (resultado) | `e48910c1e77905486a502e097f7e8acc2ac57b27db7ffb21f527e0e762d88fa4` |

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
