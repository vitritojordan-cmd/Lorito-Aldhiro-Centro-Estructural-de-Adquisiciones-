# Entrega — Lorito Aldhiro 0.15.001

*Lorito Aldhiro. Powered by Carola Records Aldhira Capture CRAC*

| Artefacto | SHA-256 |
|---|---|
| [`lorito_aldhiro_v0_15_001_golden_0.15.tar.gz`](lorito_aldhiro_v0_15_001_golden_0.15.tar.gz) — árbol completo 0.15.001 (fuentes, pruebas, herramientas, documentación, legado) | `ad71c7119557c4d7cdbe17c596bc34c9224d167e44556d753c12af53c53d2175` |
| [`lorito_aldhiro_v0_15_001_runtime.tar.gz`](lorito_aldhiro_v0_15_001_runtime.tar.gz) — paquete de producción 0.15.001 (234 archivos, reproducible) | `1bb61411b68528b99039717cd444b25faf8a81700dcaad473495ec2dafec80ea` |

La versión sube a **0.15**, como se pidió, al terminar dos correcciones puntuales. La 0.14 quedó cerrada en 0.14.007: ver [`ENTREGA_LORITO_ALDHIRO_0_14.md`](ENTREGA_LORITO_ALDHIRO_0_14.md).

## 0.15.001 — Carretes que parpadean en el desborde y letra del cuentavueltas anterior

### Carretes del LCD cuando se desborda el cuentavueltas

- **Antes:** tambaleaban, con un desplazamiento vertical de ±2,5 px.
- **Ahora:** parpadean rápido y nunca dejan de girar.
  - El parpadeo es de 6 por segundo: 83 ms encendidos y 83 ms atenuados, con el mismo atenuado de una leyenda apagada del LCD.
  - El giro sigue la física de la cinta, igual que sin desborde.
  - El parpadeo empieza encendido al desbordarse y termina con el desborde. Solo ocurre grabando en la cinta desbordada; nunca en reproducción.

### Cuentavueltas analógico con la letra anterior

- **Diagnóstico:** la familia de letra no había cambiado (LLPixel). Cambió el tamaño:
  - el rodillo de hasta 0.12 dibujaba el dígito en negrita a casi todo el alto, con un 70 % de tinta;
  - los tambores de 0.13 lo dibujaban al 45 %, y por eso parecía otra letra, más chica y fina.
- **Ahora:** LLPixel en negrita, con el dígito al 70 % del tambor, centrado por su tinta, como antes.
  - Para que el dígito grande no se encime con sus vecinos, en reposo se ve un solo dígito, como en el rodillo anterior.
  - El color ámbar, el brillo y el giro del tambor no cambian.

### Un defecto de 0.14.007 encontrado en la validación

- **Leyenda TAU:** en una de tres corridas de la suite Qt quedó encendida el doble de lo que dura el pitido.
- **Causa:** bajo carga, el aviso de fin llegaba 250 ms tarde, y la leyenda solo se apagaba con ese aviso.
- **Arreglo:** el apagado se programa al encenderse, con la duración conocida del patrón.

### Pruebas

| Suite | Resultado |
|---|---|
| Completa (stubs) | 1258 OK |
| Completa con `-W error::ResourceWarning` | 1258 OK, 0 avisos |
| Qt real (29 módulos, 453 pruebas) | 3 de 3 corridas OK |
| Tarball extraído | manifiesto verificado, 1258 OK + 73 Qt OK |
| Escáner de referencias | 0 violaciones |
| Grafo de imports | 191 módulos, 0 ciclos |

Pruebas nuevas:

- parpadeo a 6 Hz con giro continuo;
- atenuado sin desplazamiento;
- letra al 70 % para los diez dígitos;
- un solo dígito en reposo;
- leyenda TAU con aviso de fin demorado.

Detalle en `gaps/GAP-100.md` y `saneamiento_0_15_001/TEST_REPORT_0.15.001.md`, dentro del tarball.

### Capturas ([`capturas_0_15_001/`](capturas_0_15_001/))

| Archivo | Qué muestra |
|---|---|
| `00_cuentavueltas_antes_despues.png` | Tambores 0.14.007 y 0.15.001: activo, rodando y en pausa. |
| `01_carretes_desborde_antes_despues.png` | Cuatro fases seguidas de los carretes desbordados: tambaleo (0.14.007) y parpadeo con giro (0.15.001). |
| `02_referencia_rodillo_0_12_vs_tambores_0_13.png` | La letra de referencia: el rodillo de 0.12 frente a los tambores de 0.13. |
