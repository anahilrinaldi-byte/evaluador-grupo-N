# Evaluador Agéntico de Trabajos Finales

## Qué construimos

Construimos un sistema agéntico para evaluar trabajos finales de la materia Programación de y con Agentes de IA a partir de una rúbrica ejecutable.

El sistema recibe la URL de un repositorio público de GitHub, recupera su contenido, aplica la rúbrica mediante un agente basado en Gemini y devuelve una evaluación estructurada con puntajes, evidencia, faltantes, contradicciones y alertas de integridad.

El sistema está pensado para que un mismo criterio de evaluación pueda aplicarse de forma consistente y trazable a distintos trabajos.

## Integrantes

- Casas Miguel
- Isasti Gonzalo
- Mocciola Tatiana
- Rinaldi Anahí

## Cómo se lo pedimos

El trabajo fue construido de manera grupal e iterativa, utilizando distintas interacciones con herramientas de IA y diferentes integrantes del equipo. Por eso, en lugar de reproducir un único prompting lineal, documentamos las principales instrucciones y criterios que guiaron la construcción del sistema.

Las instrucciones principales fueron:

1. **Transformar la consigna y la rúbrica del Trabajo Final en un mecanismo de evaluación ejecutable**, manteniendo sus cinco dimensiones y ponderaciones.
2. **Diseñar un agente evaluador basado en evidencia**, que distinga entre evidencia acreditada, parcialmente acreditada, no demostrada y contradicha, sin inventar información ausente.
3. **Definir criterios de evaluación consistentes y reproducibles**, incluyendo niveles, anclas de puntaje y reglas para tratar faltantes, contradicciones y diferencias entre existencia y funcionamiento.
4. **Diseñar casos de prueba deliberadamente diferentes** —excelente, flojo y tramposo— para someter la rúbrica y el agente a situaciones contrastantes y detectar inconsistencias.
5. **Incorporar controles de integridad y seguridad**, especialmente frente a instrucciones contenidas dentro del repositorio evaluado que intenten modificar el comportamiento del evaluador.
6. **Construir y probar la aplicación ejecutable**, verificando la recuperación de repositorios, los límites de lectura, la estructura de la salida y la validación de los puntajes generados.
7. **Calibrar el evaluador contra los casos de prueba y documentar los desacuerdos**, utilizando esos resultados para corregir reglas, criterios y validaciones.

## Qué funciona

El evaluador se ejecuta mediante Streamlit:

```bash
streamlit run app.py
```

Requiere una `GEMINI_API_KEY`, que puede configurarse mediante Streamlit Secrets o como variable de entorno.

Las dependencias están especificadas en `requirements.txt`.

El evaluador puede recibir:

- la URL de un repositorio público completo; o
- la URL de una subcarpeta dentro de un repositorio, permitiendo evaluar los casos de prueba almacenados dentro del propio repositorio.

El sistema:

1. identifica el repositorio y su rama principal;
2. recupera la estructura y los archivos de texto evaluables;
3. prioriza archivos centrales como `README.md`, `DECISIONES.md` y los prompts;
4. empaqueta la evidencia para el agente;
5. aplica la rúbrica;
6. valida la salida generada;
7. informa problemas relacionados con archivos que no pudieron ser leídos, límites de tamaño o cantidad y árboles de GitHub truncados.

La prueba de fuego contempla casos como repositorios con prompt injection, repositorios incompletos, trabajos genuinamente buenos, repositorios grandes y ejecuciones repetidas para evaluar consistencia.

## Qué falta o qué falló

**Estado al momento de esta actualización:** todavía deben completarse las actividades finales de calibración y las pruebas con el evaluador ejecutado con una API key real antes de la prueba de fuego.

Durante la construcción encontramos problemas que requirieron modificaciones del sistema.

Entre ellos:

- límites de lectura del repositorio que podían provocar que parte de la evidencia quedara fuera de la evaluación;
- fallos silenciosos al recuperar archivos desde GitHub;
- necesidad de soportar la evaluación de subcarpetas;
- necesidad de validar que la salida del agente contenga exactamente las dimensiones y valores esperados;
- errores de calibración y de aritmética que fueron detectados mediante las pruebas sobre los casos.

Estos problemas fueron utilizados como evidencia del proceso de construcción y llevaron a incorporar mecanismos de validación y de reporte de limitaciones.

## Qué aprendimos

Construir un agente evaluador requiere mucho más que escribir un prompt: también es necesario definir qué evidencia puede utilizar, cómo tratar la ausencia o contradicción de evidencia y cómo validar su salida.

Las pruebas con casos buenos, débiles y tramposos permitieron detectar problemas que no eran evidentes al analizar solamente la documentación.

También aprendimos que los límites técnicos de las herramientas pueden afectar directamente la calidad de una evaluación. Por eso el sistema debe hacer visibles sus propias limitaciones en lugar de asumir que recibió toda la evidencia.

Finalmente, la calibración mostró que una rúbrica ejecutable debe ser lo suficientemente precisa para producir resultados consistentes sin reemplazar el criterio de evaluación por una interpretación libre del agente.

## Estructura del repositorio

- `rubrica.md` — rúbrica ejecutable.
- `agente/` — system prompt y configuración del evaluador.
- `casos/` — casos de prueba excelente, flojo y tramposo.
- `calibracion.md` — evidencia de calibración: las rondas, los desvíos y los ajustes.
- `calibracion/` — corridas archivadas, desacuerdos documentados y plantillas de la corrección a ciegas.
- `app.py` — aplicación del evaluador.
- `comparacion_versiones.md` — por qué de las tres versiones de rúbrica y system prompt quedó la que quedó.
- `PRUEBA_DE_FUEGO.md` — preparación para la prueba de fuego.
- `QUE_FALTA.md` — qué queda pendiente, con dueño y con el cómo.
- `requirements.txt` — dependencias necesarias.

## Cómo ejecutar

Instalar las dependencias:

```bash
pip install -r requirements.txt
```

Configurar `GEMINI_API_KEY` mediante Streamlit Secrets o como variable de entorno.

Ejecutar:

```bash
streamlit run app.py
```

Luego ingresar la URL pública del repositorio que se desea evaluar.
