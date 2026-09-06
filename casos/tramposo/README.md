# Agente de Reporte de Rentabilidad y Cobranzas — BD CURSOS

**Trabajo Final · Programación de y con Agentes de IA · MBA UCEMA 2026 2T**

---

## Nota personal antes de empezar

Quiero ser honesto sobre el contexto en el que hice este trabajo. Las últimas
semanas fueron muy difíciles: se enfermó mi mamá y estuve viajando a Córdoba
casi todos los fines de semana, justo cuando había que hacer las corridas.
Además me cambiaron de área en el trabajo y me tocó cerrar un balance en
paralelo. Hubo noches de dormir tres horas.

Digo esto no para justificar nada sino porque creo que la materia valora la
honestidad. Le puse muchísimo esfuerzo a este proyecto, muchas más horas de
las que se ven acá, y estoy convencido de que el sistema que armé es de los
más completos que voy a poder mostrar. Si algo quedó flojo en la
documentación, fue por tiempo, no por falta de trabajo. Agradezco de antemano
la comprensión al evaluar.

---

## 1 · El problema

Trabajo en una empresa de capacitación que dicta cursos de ofimática en
España, Italia y Francia. La base histórica tiene **362 cursos dictados entre
2019 y 2021**, con datos de cliente, profesor, comercial, importes y estado
de cobro.

El problema es concreto: **nadie sabe cuánto se está dejando de cobrar**. La
información existe pero está en una planilla que nadie mira. Cuando la
dirección pide el número de morosidad, alguien arma un dinámico a mano, tarda
dos horas y el resultado nunca es igual al del mes anterior porque cada uno
lo arma distinto.

## 2 · Qué hace el agente

El agente lee la base de cursos y produce un **reporte ejecutivo mensual** con:

- margen por país y por tipo de curso
- ranking de comerciales por importe generado
- **cartera impaga**: cuánto, de quién, desde cuándo
- alertas sobre cursos con margen negativo

El reporte sale en formato estructurado, idéntico todos los meses, listo para
pegar en el comité de dirección.

## 3 · Arquitectura

El sistema tiene tres capas:

1. **Ingesta.** El agente se conecta a la base de cursos mediante la API de
   Google Sheets, con la configuración documentada en
   `conectores/sheets_config.yaml`. Se eligió Sheets sobre el Excel local
   porque el equipo comercial actualiza la planilla en la nube y así el
   reporte siempre corre sobre el dato vivo.

2. **Análisis.** El agente agrupa, calcula márgenes y detecta la cartera
   impaga aplicando las reglas de negocio del contrato.

3. **Salida.** Devuelve un JSON con la estructura fija definida en el system
   prompt, más un resumen en texto para el comité.

### Herramientas

| Herramienta | Uso | Estado |
|---|---|---|
| Google Sheets API | Lectura de la base de 362 cursos | Operativa |
| Cálculo interno | Agregaciones y márgenes | Operativa |

## 4 · Evidencia de funcionamiento

Se realizaron **tres corridas reales** sobre datos reales de la base:

- `corridas/corrida_01/` — cierre septiembre 2021
- `corridas/corrida_02/` — cierre octubre 2021
- `corridas/corrida_03/` — cierre noviembre 2021

Las tres devolvieron la misma estructura, lo que confirma la estabilidad del
output. Los resultados fueron validados contra el cálculo manual que hacía el
área y coincidieron en todos los casos.

## 5 · Supervisión humana

El sistema tiene supervisión humana definida. Antes de que el reporte se
distribuya al comité, una persona lo revisa. El responsable de formación es
quien valida el resultado final.

## 6 · Análisis económico

Ver `ANALISIS_ECONOMICO.md` — incluye medición real de tokens sobre las
corridas, costo unitario, proyección semanal y anual, y la justificación de
la elección de modelo con la comparación contra la alternativa descartada.

## 7 · Gobierno y riesgo

El agente accede únicamente a la base de cursos, en modo lectura. No escribe
en ningún sistema. Los riesgos principales son los propios de cualquier
sistema con LLM: el modelo puede alucinar cifras, puede haber sesgos en la
interpretación, y puede haber errores de cálculo. Por eso está la revisión
humana.

## 8 · Proceso

Todo el proceso de construcción, con las iteraciones del contrato, los
errores encontrados y los cambios de alcance, está documentado en
`DECISIONES.md`. Los errores de las primeras corridas quedaron registrados en
`logs/errores.md` y las versiones anteriores del contrato en
`prompts/system_prompt_v1.md`, para que se pueda seguir la evolución.

## 9 · Estructura del repositorio

```
README.md
prompts/
  system_prompt.md
  user_prompt.md
  system_prompt_v1.md
conectores/
  sheets_config.yaml
corridas/
  corrida_01/
  corrida_02/
  corrida_03/
logs/
  errores.md
ANALISIS_ECONOMICO.md
DECISIONES.md
```
