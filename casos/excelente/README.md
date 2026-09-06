# Agente de Rentabilidad y Cobranzas — BD CURSOS

**Trabajo Final · Programación de y con Agentes de IA · MBA UCEMA 2026 2T**

---

## 1 · El problema

Una empresa de formación corporativa dicta cursos de ofimática en España,
Italia y Francia. La base histórica tiene **362 cursos dictados entre 2019 y
2021**: cliente, profesor, comercial, importes de cada parte y estado de
cobro.

El reporte de cierre se arma a mano en una planilla dinámica. Toma unas dos
horas por mes y el resultado no es comparable entre períodos, porque cada
persona lo arma con criterios distintos. En la práctica, **nadie sabía cuánto
había sin cobrar**: la primera corrida del agente lo cuantificó en 88.700
euros sobre 103 cursos, el 29,2% de la facturación histórica.

## 2 · Qué hace el agente

Lee la base y devuelve un reporte estructurado con margen por país, margen
por tipo de curso, ranking de comerciales y cartera impaga con detalle por
cliente, más alertas de negocio y un resumen ejecutivo.

Acepta filtros por país o por año, de modo que el mismo agente sirve para el
cierre general, el cierre por mercado y el cierre anual.

## 3 · Cómo está armado

| Pieza | Dónde |
|---|---|
| Contrato (system + user prompt) | `prompts/system_prompt.md`, `prompts/user_prompt.md` |
| Versiones anteriores del contrato | `prompts/versiones/` |
| Herramienta de lectura | `herramienta/config_lector.md` |
| Logs de ejecución y de error | `logs/` |
| Corridas | `corridas/corrida_01`, `_02`, `_03` |
| Historia de construcción | `DECISIONES.md` |
| Análisis económico | `ANALISIS_ECONOMICO.md` |
| Gobierno y riesgo | `GOBIERNO.md` |

**Herramienta real:** el agente no recibe los datos pegados en el prompt. Lee
`datos/bd_cursos.xlsx` mediante el lector configurado en
`herramienta/config_lector.md`, que valida las quince columnas antes de
entregar los registros. Cada lectura deja log: ver
`logs/2026-08-25_lectura_ok.log`.

## 4 · Corridas

Tres corridas con entradas distintas, para verificar que la estructura del
output se mantiene al cambiar el conjunto de datos:

| Corrida | Filtro | Registros | Facturación | Margen | Impago |
|---|---|---|---|---|---|
| 01 | Sin filtro | 362 | 303.905,00 | 171.955,35 (56,6%) | 88.700,00 |
| 02 | País = España | 149 | 118.810,00 | 66.296,40 (55,8%) | 17.815,00 |
| 03 | Año = 2021 | 120 | 101.310,00 | 58.388,10 (57,6%) | 37.040,00 |

Cada carpeta contiene la entrada exacta (`entrada.md`) y la salida sin
editar tal como la devolvió el modelo (`salida.json`).

**Hallazgo de negocio:** la morosidad de 2021 (36,6%) está siete puntos por
encima del promedio histórico. La cobranza empeoró y nadie lo había medido.

## 5 · Cómo volver a correrlo

1. Descargar la planilla actualizada y guardarla como `datos/bd_cursos.xlsx`,
   hoja `CURSOS`.
2. Cargar `prompts/system_prompt.md` como system prompt.
3. Modelo: GPT-4o mini. Temperatura: 0.
4. Enviar `prompts/user_prompt.md` completando filtro, período y fecha.
5. Verificar el log de lectura: `registros=362` y `registros_excluidos=0`.
6. Guardar la salida cruda en `corridas/corrida_NN/salida.json`.

## 6 · Supervisión

Nivel **L2**: el agente ejecuta y propone, una persona revisa antes de que el
reporte tenga efecto. El agente se detiene después de producir el JSON; el
reporte no se distribuye hasta que la revisión esté hecha. Detalle completo
—qué se revisa, quién firma, qué puede vetar— en `GOBIERNO.md`.

## 7 · Costo

USD 0,004 por corrida en el peor caso. Proyección anual sobre 96 corridas:
USD 0,39. Modelo elegido con el criterio del curso, comparado contra la
alternativa descartada. Ver `ANALISIS_ECONOMICO.md`.

## 8 · Qué falló y qué quedó afuera

La primera versión del contrato calculaba mal el margen: omitía la comisión
del comercial y sobreestimaba el resultado en 28.041,65 euros. El error
textual y el diagnóstico están en `logs/2026-08-22_error_margen.log`, y la
corrección en `DECISIONES.md`.

Quedaron afuera, con su motivo declarado en `DECISIONES.md`: la conexión a
Google Sheets, la alerta automática por umbral de morosidad, y el horizonte
semanal del análisis económico.
