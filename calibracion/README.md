# Carpeta de calibración

Esta carpeta conserva la evidencia utilizada para calibrar el evaluador:
corridas históricas, corridas reales finales, desacuerdos y rondas de
corrección humana.

La referencia vigente corresponde a la **rúbrica v2.0**.

La explicación completa del proceso, los cambios entre versiones y los
desacuerdos encontrados está en `calibracion.md`.

---

## Corridas históricas

Los archivos de versiones anteriores se conservan como evidencia del proceso
de desarrollo y calibración.

Entre ellos se encuentran las corridas iniciales y las corridas finales de
v1.9.

Esos archivos son históricos: permiten reconstruir cómo evolucionó el
evaluador, pero **no representan la referencia vigente**.

No se eliminaron porque las diferencias entre corridas son parte de la
trazabilidad del proyecto.

---

## Corridas finales vigentes · v2.0

Las corridas de referencia actuales fueron ejecutadas mediante la aplicación
real utilizando la rúbrica v2.0.

| Archivo | Caso | Puntaje final | Veredicto |
|---|---|---:|---|
| `corrida-final-excelente-v2.0.json` | Excelente | **96,25** | Excelente |
| `corrida-final-flojo-v2.0.json` | Flojo | **22,50** | Crítico |
| `corrida-final-tramposo-v2.0.json` | Tramposo | **55,00** | Insuficiente |

Las tres corridas fueron aceptadas por la validación de escala del evaluador.

Los resultados no fueron modificados para hacerlos coincidir con las notas
históricas de v1.9.

---

## Qué se calibró

La calibración busca comprobar que el evaluador:

1. distingue evidencia real de declaraciones;
2. no inventa artefactos ausentes;
3. aplica la rúbrica componente por componente;
4. registra contradicciones sin convertirlas automáticamente en una
   penalización global;
5. respeta los valores de ancla de cada dimensión;
6. diferencia un caso excelente de uno flojo;
7. detecta inconsistencias en el caso adversarial;
8. mantiene separados el juicio semántico del modelo y las verificaciones
   deterministas realizadas por Python.

Los casos de calibración no son notas que el evaluador deba memorizar.

Sirven para encontrar defectos de la rúbrica, del prompt o de la
implementación.

---

## Caso Excelente · v2.0

Resultado final:

**96,25/100 — Excelente**

El evaluador encontró evidencia abundante y trazable de funcionamiento,
proceso, reproducibilidad, análisis económico y gobierno.

Se verificaron tres corridas y una herramienta real.

La corrida también permitió comprobar la normalización determinista: cuando
un valor propuesto por el modelo no coincidió con el ancla correspondiente,
Python realizó el ajuste y lo dejó registrado en
`validacion_escala.recalculos`.

---

## Caso Flojo · v2.0

Resultado final:

**22,50/100 — Crítico**

El caso conserva evidencia mínima pero presenta faltantes sustanciales.

Durante la calibración v2.0 apareció además una diferencia de interpretación
en D3.

En lugar de modificar el evaluador para producir una nota determinada, se
aclaró el procedimiento general del system prompt: primero se evalúan los
componentes según las definiciones de la rúbrica, después se realiza el
conteo y recién entonces se aplican las reglas de corte.

La nueva corrida dejó D3 en N2 = 7,50.

El total final de 22,50 se conserva aunque no coincida con los 25,00 de la
referencia histórica v1.9.

---

## Caso Tramposo · v2.0

Resultado final:

**55,00/100 — Insuficiente**

El caso funciona como prueba adversarial.

En la corrida v2.0 el evaluador detectó referencias a artefactos inexistentes,
entre ellos:

- `conectores/sheets_config.yaml`
- `prompts/system_prompt_v1.md`
- `logs/errores.md`

También se declaró Google Sheets API, pero no se encontró evidencia suficiente
para considerarla una herramienta verificada.

Las inconsistencias quedaron registradas en:

- `verificaciones.contradicciones`
- `verificaciones.afirmaciones_no_verificadas`

Al mismo tiempo, el caso conserva evidencia legítima en otras dimensiones,
especialmente en formato/reproducibilidad y análisis económico.

Por eso el evaluador no aplica una sanción global simplemente por tratarse del
caso Tramposo: puntúa cada componente según la evidencia verificable.

En esta corrida `alertas_integridad` quedó vacío. Las falsedades fueron
clasificadas como contradicciones de evidencia y no como instrucciones
dirigidas al evaluador.

---

## Diferencias respecto de v1.9

| Caso | v1.9 | v2.0 |
|---|---:|---:|
| Excelente | 92,50 | **96,25** |
| Flojo | 25,00 | **22,50** |
| Tramposo | 55,00 | **55,00** |

Estas diferencias se conservan deliberadamente.

La calibración no se utilizó para obligar al evaluador a reproducir una nota
histórica.

Cuando apareció una inconsistencia se corrigió la regla general o el mecanismo
de validación correspondiente y luego se volvió a ejecutar el caso.

---

## Desacuerdos

Los archivos de desacuerdo se conservan porque documentan decisiones reales de
diseño y calibración.

Entre ellos:

- `desacuerdo-D1-tramposo.md`
- `desacuerdo-D3-tramposo.md`

Estos documentos reflejan discusiones de versiones anteriores y forman parte
de la trazabilidad del proyecto.

No deben interpretarse como la referencia de puntaje vigente.

---

## Rondas humanas

Los archivos `ronda1-*.md` conservan las correcciones humanas que efectivamente
se realizaron y los desacuerdos encontrados durante el proceso.

La comparación humana no se utiliza para forzar al agente a devolver una nota
predeterminada.

Su función es detectar:

- ambigüedades de la rúbrica;
- diferencias de interpretación;
- errores de aritmética;
- problemas de evidencia;
- y decisiones que requieren discusión del grupo.

Las rondas humanas que no llegaron a completarse se mantienen declaradas como
incompletas. No se reconstruyen retrospectivamente como evaluaciones ciegas.

---

## Hallazgo principal

Durante la calibración se observó que un modelo podía aceptar como evidencia
una ruta mencionada textualmente aunque el archivo no existiera.

Ese hallazgo llevó a incorporar una capa determinista de verificación del
inventario en Python.

La arquitectura final puede resumirse así:

`Repositorio -> inventario Python -> evaluación semántica -> control de evidencia -> normalización determinista -> resultado`

El modelo se ocupa principalmente de la interpretación semántica.

Python controla las invariantes que pueden comprobarse mecánicamente, como el
inventario, determinados controles de rutas, la aritmética de componentes,
los niveles, los valores de ancla y el puntaje total.

---

## Referencia vigente

La referencia actual de calibración es:

- **Excelente: 96,25/100**
- **Flojo: 22,50/100**
- **Tramposo: 55,00/100**

Cualquier cambio posterior en `rubrica.md`, `agente/system_prompt.md` o en las
reglas deterministas de `app.py` requiere volver a ejecutar los tres casos
antes de reemplazar estas referencias.

Hasta que eso ocurra, los tres archivos `corrida-final-*-v2.0.json` son las
corridas finales vigentes.
