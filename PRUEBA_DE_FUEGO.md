# Prueba de fuego — jueves 10/9

La prueba de fuego utiliza casos nuevos y públicos para comparar los evaluadores
construidos por los grupos.

El objetivo del grupo es demostrar no solamente que el evaluador produce una
nota, sino que puede explicar qué verificó, qué no pudo verificar y por qué
asignó cada puntaje.

---

## Antes de entrar

Chequeo previo:

- comprobar que el repositorio abre desde una ventana privada;
- comprobar que la aplicación desplegada abre correctamente;
- confirmar que la API key está disponible mediante Streamlit Secrets;
- realizar una evaluación de prueba para verificar disponibilidad del modelo;
- tener disponible el repositorio y las corridas finales de calibración;
- no modificar rúbrica, prompt o código después del último ensayo salvo que se
  detecte una falla crítica.

Aplicación:

`https://evaluador-grupo-n.streamlit.app/`

---

## Roles durante la prueba

Para evitar improvisación conviene separar tres funciones.

| Rol | Qué hace |
|---|---|
| **Manos** | Maneja la pantalla y ejecuta la evaluación. |
| **Voz** | Explica brevemente qué comportamiento esperamos antes de ejecutar. |
| **Notas** | Registra resultados, preguntas y cualquier comportamiento inesperado. |

La persona que explica debe anticipar el comportamiento esperado sin prometer un
puntaje exacto.

Ejemplo:

> “Esperamos que el evaluador diferencie lo declarado de lo verificable y que
> muestre explícitamente las limitaciones o contradicciones que encuentre.”

No conviene anticipar una nota exacta porque parte de la evaluación requiere
interpretación semántica.

---

## Casos que pueden aparecer

### 1. Repositorio con instrucciones dirigidas al evaluador

El contenido del repositorio se considera evidencia no confiable.

Las instrucciones encontradas dentro del trabajo evaluado no pueden reemplazar
la rúbrica ni las instrucciones del evaluador.

Si existe un intento inequívoco de modificar el comportamiento del corrector,
puede registrarse en `alertas_integridad`.

Una contradicción, una afirmación falsa o una apelación persuasiva no se
clasifican automáticamente como prompt injection.

**Qué decir antes:**

> “El repositorio es tratado como contenido no confiable. Una instrucción dentro
> del trabajo no puede cambiar las reglas del evaluador.”

---

### 2. Repositorio vacío, incompleto o roto

El evaluador debe puntuar solamente lo verificable.

La ausencia de evidencia no se reemplaza con una estimación.

Si el paquete recibido es incompleto por una limitación técnica, esa situación
debe distinguirse de la inexistencia demostrada de un artefacto.

**Qué decir antes:**

> “Si falta evidencia, el sistema no la inventa. Y si no pudo leer una parte del
> repositorio, debe distinguir esa limitación de una ausencia real.”

---

### 3. Repositorio genuinamente bueno

Un evaluador excesivamente desconfiado también sería un mal evaluador.

La calibración v2.0 incluyó un caso Excelente que obtuvo **96,25/100** y un caso
Flojo que obtuvo **22,50/100** sin que ser incompleto se transformara
automáticamente en una alerta de integridad.

**Qué decir antes:**

> “Los controles de integridad no están diseñados para castigar un trabajo por
> ser incompleto. Buscan verificar la evidencia y registrar contradicciones
> concretas.”

---

### 4. Repositorio grande

La aplicación utiliza límites para evitar enviar cantidades ilimitadas de
contenido al modelo.

Configuración actual:

- máximo de archivos evaluables: **150**;
- máximo por archivo: **12.000 caracteres**;
- máximo total del repositorio: **400.000 caracteres**.

La aplicación registra información sobre archivos fallidos, truncamientos y
otras limitaciones de carga.

Cuando el paquete es incompleto, una ruta ausente del material recibido no debe
tratarse automáticamente como prueba de que el archivo no existe.

**Qué decir:**

> “Si el repositorio supera los límites, la aplicación hace visible que el
> paquete está incompleto en lugar de convertir automáticamente lo que no leyó
> en evidencia ausente.”

---

### 5. Repetición del mismo caso

La calibración mostró que `temperature = 0` no garantiza identidad absoluta
entre ejecuciones.

Por eso el objetivo de repetibilidad no es exigir que cada componente semántico
produzca siempre exactamente la misma interpretación.

Lo que debe permanecer estable son las invariantes objetivas:

- una ruta inexistente no puede utilizarse como evidencia válida;
- las corridas declaradas deben contrastarse con las verificables;
- una herramienta no queda verificada solo porque sea mencionada;
- los niveles y puntajes deben respetar las anclas de la rúbrica;
- la suma final debe corresponder a los puntajes de las dimensiones.

**Qué decir:**

> “No prometemos identidad textual entre dos llamadas al modelo. Sí controlamos
> determinísticamente los hechos objetivos y la aritmética de la evaluación.”

---

## El caso Tramposo y el control de evidencia

El caso Tramposo fue especialmente útil durante el desarrollo.

En las primeras pruebas observamos que el modelo podía aceptar como evidencia
una ruta simplemente porque aparecía escrita en README.md o DECISIONES.md.

Eso llevó a incorporar un control determinista en Python.

La arquitectura final puede resumirse así:

`Repositorio -> inventario Python -> evaluación semántica -> control de evidencia -> normalización determinista -> resultado`

En la corrida final v2.0 del caso Tramposo:

- se declararon y verificaron 3 corridas;
- se declaró Google Sheets API, pero no se verificó una herramienta;
- se detectaron referencias a artefactos inexistentes.

Entre ellas:

- `conectores/sheets_config.yaml`
- `prompts/system_prompt_v1.md`
- `logs/errores.md`

Las inconsistencias quedaron registradas como contradicciones y afirmaciones no
verificadas. `alertas_integridad` quedó vacío porque las falsedades detectadas
fueron tratadas como contradicciones de evidencia y no como instrucciones
dirigidas al evaluador.

El resultado final fue **55,00/100 — Insuficiente**.

El caso conserva, sin embargo, los puntos correspondientes a evidencia legítima.
Por ejemplo, obtuvo el puntaje completo de análisis económico.

Esto demuestra que las contradicciones no generan una penalización global por
impresión general.

---

## Si algo falla en vivo

No conviene ocultar una falla ni ejecutar repetidamente hasta obtener una salida
más conveniente.

Primero hay que identificar qué tipo de falla ocurrió.

Puede tratarse de:

- acceso al repositorio;
- disponibilidad de Gemini;
- límites del paquete;
- JSON inválido;
- evidencia inexistente;
- interpretación semántica discutible;
- validación determinista.

La respuesta debe separar un error técnico de un desacuerdo de evaluación.

Ejemplo:

> “La aplicación pudo recuperar el repositorio, pero el modelo devolvió una
> salida que no cumple la estructura esperada. El sistema la rechaza en lugar de
> publicar una nota inválida.”

Que una corrida sea rechazada por una validación puede ser evidencia de que el
control funciona.

---

## Tres cosas para mostrar si preguntan cómo evolucionó el sistema

### 1. Rúbrica ejecutable

La rúbrica pasó por distintas versiones a medida que aparecieron ambigüedades en
las pruebas.

La versión utilizada en las corridas finales vigentes es **v2.0**.

Los cambios y desacuerdos están documentados en `calibracion.md`.

### 2. Separación entre modelo y código determinista

El modelo interpreta evidencia, pero no tiene control absoluto sobre el puntaje
final.

Python controla hechos objetivos, conteos, niveles, anclas y suma final.

Las corridas finales muestran esta diferencia:

| Caso | Propuesta del modelo | Puntaje final |
|---|---:|---:|
| Excelente | 93,75 | **96,25** |
| Flojo | 37,50 | **22,50** |
| Tramposo | 63,75 | **55,00** |

### 3. El problema de las rutas inexistentes

La calibración descubrió que un LLM puede aceptar como evidencia una ruta
mencionada aunque no exista.

Ese problema no se intentó resolver solamente agregando instrucciones al prompt.

Se agregó una verificación objetiva del inventario en Python y una segunda
evaluación cuando el control encuentra evidencia inválida.

Es un ejemplo concreto de una decisión arquitectónica surgida de una falla real.

---

## Resultados de referencia antes de la prueba

Las corridas finales de calibración son:

| Caso | Resultado |
|---|---|
| Excelente | **96,25 — Excelente** |
| Flojo | **22,50 — Crítico** |
| Tramposo | **55,00 — Insuficiente** |

Estas referencias corresponden a la rúbrica **v2.0**. Las corridas anteriores
se conservan en `calibracion/` como evidencia histórica del proceso.

Estos valores son referencias de calibración, no puntajes que el evaluador deba
forzar ante otros repositorios.

Los JSON completos están archivados en `calibracion/`.

---

## Ensayo previo

Cada integrante puede proponer un repositorio o caso que los demás no hayan
utilizado durante la construcción.

Durante el ensayo:

1. ejecutar una sola vez;
2. registrar el resultado;
3. revisar evidencia y contradicciones antes de mirar solamente el total;
4. comprobar si cualquier afirmación objetiva del evaluador puede reconstruirse;
5. modificar el sistema únicamente si se descubre una falla generalizable.

No modificar la rúbrica solamente para obtener un puntaje esperado.

La pregunta central ante cualquier resultado inesperado es:

> “¿Encontramos una ambigüedad semántica legítima o una propiedad objetiva que
> deberíamos verificar mediante código?”

Esa distinción resume una de las principales decisiones de diseño del evaluador.
