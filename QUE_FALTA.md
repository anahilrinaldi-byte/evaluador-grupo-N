# Cierre de traspaso

Este documento comenzó como una lista de pendientes durante el desarrollo del
evaluador.

Al cierre de la versión presentada, los pendientes principales fueron resueltos.
Se conserva este archivo como evidencia del proceso y de cómo evolucionó el
proyecto.

---

## Estado final

| Tema | Estado |
|---|---|
| Rúbrica ejecutable | ✅ Completo |
| Agente corrector | ✅ Completo |
| Entrada de repositorios GitHub | ✅ Completo |
| Casos Excelente, Flojo y Tramposo | ✅ Completo |
| Corridas reales del evaluador | ✅ Completo |
| Calibración | ✅ Completo |
| Control determinista de evidencia | ✅ Completo |
| Validación de puntajes | ✅ Completo |
| README principal | ✅ Actualizado |
| Prueba de fuego | ✅ Actualizada |

---

## 1. Corridas reales — completado

El evaluador fue ejecutado sobre los tres casos de calibración utilizando la
aplicación real.

Resultados finales con la rúbrica v1.9:

| Caso | Puntaje final | Veredicto |
|---|---:|---|
| Excelente | **92,50** | Excelente |
| Flojo | **25,00** | Crítico |
| Tramposo | **55,00** | Insuficiente |

Los JSON completos de las corridas finales se conservan en `calibracion/`.

En las tres corridas:

- `validacion_escala.ok = true`;
- `validacion_escala.recalculos = []`.

Los puntajes históricos utilizados durante etapas anteriores de calibración no
son objetivos que la versión final deba reproducir.

---

## 2. Calibración — completada

Durante el desarrollo se compararon distintas interpretaciones humanas y
automáticas de la rúbrica.

Los desacuerdos encontrados sirvieron para modificar y precisar la rúbrica, el
system prompt y los controles del evaluador.

La calibración completa está documentada en:

- `calibracion.md`;
- `calibracion/README.md`;
- archivos `ronda1-*`;
- archivos `desacuerdo-*`;
- corridas históricas y finales.

La versión final de la rúbrica utilizada para las corridas de referencia es
**v1.9**.

---

## 3. Principal hallazgo de calibración

El caso Tramposo permitió detectar una falla importante: durante el desarrollo,
el modelo podía aceptar como evidencia una ruta mencionada en un documento
aunque el artefacto no existiera realmente en el repositorio.

El problema no se resolvió intentando forzar una nota determinada.

Se incorporó una capa determinista en Python para contrastar las referencias de
evidencia con el inventario real del repositorio.

La arquitectura final puede resumirse así:

`Repositorio -> inventario Python -> evaluación semántica -> control de evidencia -> normalización determinista -> resultado`

Si se detecta una referencia inexistente, el sistema solicita una revisión
semántica de los componentes afectados. Si la salida insiste en utilizar una
ruta inexistente como evidencia, la corrida se rechaza.

---

## 4. Separación entre modelo y controles deterministas

La versión final no delega toda la corrección al LLM.

### Modelo

Se utiliza para interpretar evidencia, aplicar criterios semánticos y explicar
la evaluación.

### Python

Se utiliza para controles objetivos, entre ellos:

- inventario de archivos;
- verificación de referencias de evidencia;
- cantidad de corridas verificables;
- normalización de estados;
- aplicación de anclas;
- reglas de corte;
- cálculo del total;
- asignación del veredicto.

Esta separación surgió de las fallas observadas durante la calibración.

---

## 5. Casos de prueba

### Excelente

Caso diseñado con evidencia amplia del sistema, proceso, corridas, economía y
gobierno.

Resultado final: **92,50 — Excelente**.

### Flojo

Caso incompleto que permite comprobar que ausencia de evidencia no significa
automáticamente fraude o manipulación.

Resultado final: **25,00 — Crítico**.

### Tramposo

Caso que contiene declaraciones que no coinciden con el inventario real y, al
mismo tiempo, evidencia legítima en otras dimensiones.

Resultado final: **55,00 — Insuficiente**.

La nota no se fuerza por la etiqueta del caso: cada componente se puntúa según
la evidencia verificable.

---

## 6. Corridas históricas

Durante el desarrollo existieron resultados distintos para los casos de
calibración, especialmente para Tramposo.

Esos resultados se conservan porque muestran la evolución del sistema.

No deben interpretarse como notas objetivo de la versión final.

Los cambios entre corridas llevaron a:

- precisar la rúbrica;
- mejorar el system prompt;
- distinguir declaración de evidencia;
- agregar controles deterministas;
- evitar falsos positivos en detección de rutas;
- separar contradicciones de alertas de integridad;
- validar aritmética y niveles fuera del modelo.

---

## 7. Estado para la prueba de fuego

Antes de la prueba pública no se prevén cambios de arquitectura.

La prioridad es mantener estable la versión calibrada y realizar solamente
correcciones ante fallas críticas.

La guía específica está en:

`PRUEBA_DE_FUEGO.md`

La aplicación desplegada está disponible en:

`https://evaluador-grupo-n.streamlit.app/`

---

# Criterio de cierre

El proyecto deja de considerarse una demostración de prompt cuando incorpora
controles externos al modelo y puede rechazar o corregir resultados que no
cumplen sus invariantes.

La versión final combina:

**rúbrica ejecutable + recuperación de evidencia + interpretación semántica +
verificación determinista + salida estructurada + calibración documentada.**

A partir de este punto, cualquier cambio en la rúbrica, el prompt o los controles
debe volver a probarse contra los tres casos antes de incorporarse a la versión
presentada.
