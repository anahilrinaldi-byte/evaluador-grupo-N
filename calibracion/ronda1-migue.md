# Ronda 1 · corrección a ciegas — migue

**No mires las notas de los otros hasta que los cuatro archivos estén subidos.**
El desacuerdo es el entregable: si mirás antes, se pierde.

Puntuá los **tres casos** en las **cinco dimensiones**, con `rubrica.md` en la mano.

---

## Valores permitidos

V2 usa anclas discretas: **no existen valores intermedios.** Solo podés poner uno
de estos cinco por dimensión.

| Nivel | D1 (30) | D2 (25) | D3 (15) | D4 (15) | D5 (15) |
|---|---|---|---|---|---|
| N4 | 30 | 25 | 15 | 15 | 15 |
| N3 | 22,5 | 18,75 | 11,25 | 11,25 | 11,25 |
| N2 | 15 | 12,5 | 7,5 | 7,5 | 7,5 |
| N1 | 7,5 | 6,25 | 3,75 | 3,75 | 3,75 |
| N0 | 0 | 0 | 0 | 0 | 0 |

**Cómo se llega al nivel:** contá los cuatro componentes de la dimensión
—verificado 1, parcial 0,5, no verificado 0—, sumá y truncá hacia abajo.
3,5 trunca a 3, o sea N3.

---

## Caso EXCELENTE — `casos/excelente`

| Dim | Componentes (V / P / NV) | Conteo | Nivel | Puntaje | Por qué |
|---|---|---|---|---|---|
| D1 Sistema | V / V / V / V | 4 | N4 | 30 | Contrato completo, herramienta real, output estructurado y supervisión verificables. |
| D2 Proceso | V / V / V / V | 4 | N4 | 25 | Iteraciones, falla con artefacto, decisiones y cambios de alcance con traza. |
| D3 Formato | V / V / V / V | 4 | N4 | 15 | Estructura completa, tres corridas verificables, reconstruibilidad e instrucciones suficientes. |
| D4 Económico | V / V / P / V | 3,5 | N3 | 11,25 | La proyección económica no satisface completamente todos los horizontes exigidos. |
| D5 Gobierno | V / V / P / V | 3,5 | N3 | 11,25 | La supervisión está definida, pero el componente no alcanza toda la especificidad exigida. |
| **Total** | | | | **92,50** | |

## Caso FLOJO — `casos/flojo`

| Dim | Componentes (V / P / NV) | Conteo | Nivel | Puntaje | Por qué |
|---|---|---|---|---|---|
| D1 Sistema | P / NV / NV / NV | 0,5 | N1 | 7,5 | Solo el contrato alcanza parcialidad; no se verifica herramienta, output estructurado ni gancho de supervisión. |
| D2 Proceso | NV / NV / NV / NV | 0 | N0 | 0 | El proceso no queda demostrado con trazas suficientes. |
| D3 Formato | P / P / NV / NV | 1 | N1 | 3,75 | La estructura y la cantidad de corridas aportan evidencia parcial, pero no permiten acreditar reconstrucción ni ejecución suficiente. |
| D4 Económico | NV / V / NV / NV | 1 | N1 | 3,75 | Hay cálculo de costo por corrida, pero faltan proyección y justificación del modelo; el consumo no queda verificable. |
| D5 Gobierno | V / P / P / NV | 2 | N2 | 7,5 | El perímetro está verificado y riesgos/supervisión son parciales; la responsabilidad no alcanza la condición completa. |
| **Total** | | | | **22,50** | |

## Caso TRAMPOSO — `casos/tramposo`

| Dim | Componentes (V / P / NV) | Conteo | Nivel | Puntaje | Por qué |
|---|---|---|---|---|---|
| D1 Sistema | NV / NV / P / P | 1 | N0 | 0 | El conteo inicial es N1; R1/R2/R6 penalizan las múltiples rutas y artefactos declarados pero inexistentes. |
| D2 Proceso | NV / NV / V / V | 2 | N0 | 0 | N2 por conteo; R10 clasifica el relato sin traza como retroactivo y R11 aplica una reducción adicional por las discrepancias. |
| D3 Formato | V / NV / NV / NV | 1 | N1 | 3,75 | La estructura obligatoria existe; R15 descarta las corridas sin par entrada–salida y R14 opera como techo. |
| D4 Económico | P / V / V / V | 3,5 | N3 | 11,25 | La tabla de tokens no tiene el artefacto original de medición; costo, proyección y elección de modelo sí son verificables. |
| D5 Gobierno | V / NV / P / P | 2 | N2 | 7,5 | Perímetro verificado; riesgos genéricos no alcanzan; supervisión y responsabilidad son parciales. |
| **Total** | | | | **22,50** | |

---

## Dónde dudé

- **Excelente, D4:** el alcance de la proyección económica exigida y su carácter parcial.
- **Excelente, D5:** cuánto detalle adicional debía exigirse al mecanismo de supervisión.
- **Flojo, D3:** cómo representar la evidencia parcial de la estructura y de la única corrida sin sobreinterpretarla.
- **Tramposo, D1:** si las discrepancias declaradas/inexistentes debían producir una reducción adicional; la revisión de R2/R6 confirmó que sí.
- **Tramposo, D3:** si las dos carpetas existentes podían contar como corridas; R15 obliga a descartarlas por falta del par entrada–salida.
- **Tramposo, D4:** la tabla de tokens es coherente, pero P9 impide tratarla como evidencia original sin el artefacto de medición.

## Contradicciones y alertas que encontré

Solo para el tramposo, en principio. Si le encontrás contradicciones al flojo,
anotalo igual: sería una señal de que la rúbrica induce falsos positivos.

- El README declara tres corridas, pero `corrida_03/` no existe.
- Declara `conectores/sheets_config.yaml`, pero no existe.
- Declara `prompts/system_prompt_v1.md`, pero no existe.
- Declara `logs/errores.md`, pero no existe.
- Declara una salida JSON estructurada, pero las corridas disponibles contienen `reporte.md`.
- Declara medición real de tokens, pero no conserva el artefacto original de esa medición.
- `DECISIONES.md` relata iteraciones y un error de cálculo sin conservar los artefactos que permitirían reconstruirlos.
- Los riesgos de D5 están formulados genéricamente y no establecen consecuencias/procedimientos concretos para las fallas.
