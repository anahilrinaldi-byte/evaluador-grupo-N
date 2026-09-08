# Carpeta de calibración

Esta carpeta conserva la evidencia utilizada para calibrar el evaluador:
corridas históricas, corridas reales finales, desacuerdos y rondas de corrección
humana.

## Corridas históricas

Los archivos:

- `corrida-excelente.json`
- `corrida-flojo.json`
- `corrida-tramposo.json`

corresponden a etapas anteriores de calibración.

Se conservan como evidencia del proceso y no representan necesariamente el
resultado de la versión final del evaluador.

## Corridas reales finales

Las corridas finales fueron ejecutadas mediante la aplicación real con la
rúbrica v1.9.

| Archivo | Caso | Puntaje final | Veredicto |
|---|---|---:|---|
| `corrida-final-excelente-v1.9.json` | Excelente | **92,50** | Excelente |
| `corrida-final-flojo-v1.9.json` | Flojo | **25,00** | Crítico |
| `corrida-final-tramposo-v1.9.json` | Tramposo | **55,00** | Insuficiente |

En las tres corridas finales:

- `validacion_escala.ok = true`
- `validacion_escala.recalculos = []`

La aplicación conserva además `puntaje_total_modelo` para permitir comparar la
propuesta del LLM con el puntaje final derivado por la capa de validación.

## Qué se calibró

La calibración buscó comprobar que el evaluador:

1. distingue evidencia real de declaraciones;
2. no inventa artefactos ausentes;
3. aplica la rúbrica componente por componente;
4. registra contradicciones sin convertirlas en una penalización global;
5. respeta los valores de ancla de cada dimensión;
6. diferencia casos excelentes, flojos y con afirmaciones no verificables.

## Caso Excelente

El resultado final fue **92,50/100**.

El evaluador verificó:

- contrato completo;
- herramienta real;
- tres corridas;
- trazas de proceso;
- estructura reproducible;
- análisis económico;
- gobierno y supervisión.

Los faltantes fueron menores y se concentraron en proyección semanal y
contingencia de responsabilidad.

## Caso Flojo

El resultado final fue **25,00/100**.

El evaluador verificó solamente una corrida y no encontró evidencia suficiente
de herramienta real, proceso iterativo, análisis económico completo ni gobierno
robusto.

El caso sirve además como control de falsos positivos: ser incompleto no implica
automáticamente manipulación.

## Caso Tramposo

El resultado final fue **55,00/100**.

El evaluador verificó dos corridas aunque el repositorio declaraba tres y detectó
referencias a artefactos inexistentes, entre ellos:

- `conectores/sheets_config.yaml`
- `prompts/system_prompt_v1.md`
- `logs/errores.md`
- `corridas/corrida_03/`

También declaró Google Sheets API, pero no se encontró evidencia suficiente para
considerarla herramienta verificada.

Al mismo tiempo, el caso conserva evidencia válida en otras dimensiones,
especialmente análisis económico.

Por eso el evaluador no aplica una sanción global por “ser tramposo”: puntúa cada
componente según la evidencia disponible.

## Desacuerdos

Los archivos de desacuerdo se conservan porque documentan decisiones reales de
diseño y calibración.

Entre ellos:

- `desacuerdo-D1-tramposo.md`
- `desacuerdo-D3-tramposo.md`

Estos documentos reflejan discusiones de versiones anteriores y forman parte de
la trazabilidad del proyecto.

## Rondas humanas

Los archivos `ronda1-*.md` documentan correcciones humanas y desacuerdos entre
criterios.

La calibración humana no se utiliza para forzar al agente a devolver una nota
predeterminada. Su función es detectar ambigüedades en la rúbrica o diferencias
sistemáticas entre interpretación humana y evaluación automática.

## Hallazgo principal

Durante la calibración se observó que el modelo podía aceptar como evidencia una
ruta mencionada textualmente aunque el archivo no existiera.

Ese hallazgo llevó a incorporar una capa determinista de verificación del
inventario en Python.

La arquitectura final separa:

`evidencia objetiva -> Python`

de

`interpretación semántica -> modelo`

y luego vuelve a aplicar validaciones deterministas antes de publicar el
resultado.

La explicación completa del proceso está en `calibracion.md`.
