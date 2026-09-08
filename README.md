# Evaluador Agéntico de Trabajos Finales

## Qué construimos

Construimos un sistema agéntico para evaluar trabajos finales de la materia
**Programación de y con Agentes de IA** a partir de una rúbrica ejecutable.

El sistema recibe la URL de un repositorio público de GitHub, recupera su
contenido, aplica la rúbrica mediante un agente basado en Gemini y devuelve una
evaluación estructurada con puntajes, evidencia, faltantes, contradicciones y
alertas de integridad.

El objetivo es que un mismo criterio de evaluación pueda aplicarse de forma
consistente, trazable y auditable a distintos trabajos.

## Integrantes

- Casas Miguel
- Isasti Gonzalo
- Mocciola Tatiana
- Rinaldi Anahí

## Aplicación desplegada

El evaluador está disponible en:

https://evaluador-grupo-n.streamlit.app/

También puede ejecutarse localmente mediante Streamlit.

## Cómo se lo pedimos

El trabajo fue construido de manera grupal e iterativa, utilizando herramientas
de IA como apoyo para diseñar, programar, probar y corregir el sistema.

En lugar de reproducir un único prompting lineal, documentamos las principales
instrucciones y criterios que guiaron la construcción:

1. **Transformar la consigna y la rúbrica del Trabajo Final en un mecanismo de
   evaluación ejecutable**, manteniendo sus cinco dimensiones y ponderaciones.
2. **Diseñar un agente evaluador basado en evidencia**, que distinga entre
   evidencia verificada, parcial y no verificada, sin inventar información
   ausente.
3. **Definir criterios consistentes y reproducibles**, incluyendo niveles,
   anclas de puntaje y reglas para tratar faltantes y contradicciones.
4. **Diseñar tres casos de prueba deliberadamente diferentes** —Excelente,
   Flojo y Tramposo— para someter la rúbrica y el agente a situaciones
   contrastantes.
5. **Tratar el repositorio evaluado como contenido no confiable**, evitando que
   instrucciones incluidas dentro de un trabajo puedan modificar las reglas del
   evaluador.
6. **Construir y probar una aplicación ejecutable**, verificando la recuperación
   de repositorios, los límites de lectura, la estructura de salida y los
   puntajes.
7. **Calibrar el evaluador contra los casos de prueba**, documentando
   desacuerdos y utilizando los hallazgos para mejorar la rúbrica y el sistema.
8. **Separar interpretación semántica de controles objetivos**, dejando al
   modelo la interpretación de evidencia y a Python las verificaciones que
   pueden resolverse de forma determinista.

## Cómo funciona

El evaluador acepta:

- la URL de un repositorio público completo de GitHub; o
- la URL de una subcarpeta de un repositorio.

Esto último permite evaluar de manera independiente los tres casos almacenados
en `casos/`.

El flujo general es:

`Repositorio -> inventario Python -> evaluación semántica -> control de evidencia -> normalización determinista -> resultado`

### 1. Recuperación del repositorio

La aplicación identifica el repositorio y la rama correspondiente, recupera el
árbol de archivos y descarga los archivos de texto admitidos.

También informa si existieron archivos que no pudieron leerse o si se alcanzaron
límites de cantidad o tamaño.

### 2. Inventario de evidencia

Python construye un inventario de los archivos efectivamente incorporados a la
evaluación.

Esto permite distinguir entre una ruta que realmente existe y una ruta que el
trabajo solamente declara en un README u otro documento.

### 3. Evaluación semántica

El agente recibe:

- la rúbrica ejecutable;
- el contenido recuperado;
- metadatos del repositorio;
- instrucciones de seguridad e integridad.

El modelo evalúa las cinco dimensiones y devuelve una salida estructurada.

### 4. Control determinista

Después de la evaluación semántica, Python controla hechos que pueden
verificarse mecánicamente.

Entre otros controles:

- contrasta rutas utilizadas como evidencia con el inventario real;
- verifica la cantidad de corridas encontradas;
- normaliza estados de componentes;
- calcula los niveles a partir de los conteos;
- aplica los valores de ancla;
- recalcula el puntaje total;
- deriva el veredicto final.

Si una ruta inexistente es utilizada como evidencia, el sistema solicita una
revisión de la evaluación. Si la inconsistencia persiste, la corrida no se
publica como una evaluación válida.

## Rúbrica

La versión utilizada en las corridas finales es **v1.9**.

La evaluación se divide en cinco dimensiones:

| Dimensión | Puntaje máximo |
|---|---:|
| Sistema completo | 30 |
| Proceso documentado | 25 |
| Formato y reproducibilidad | 15 |
| Análisis económico | 15 |
| Gobierno y riesgo | 15 |
| **Total** | **100** |

Cada dimensión se descompone en componentes observables y utiliza niveles
discretos de evaluación.

La evidencia tiene prioridad sobre las declaraciones: afirmar que un artefacto
existe no equivale a demostrar su existencia.

## Casos de prueba

Se construyeron tres casos de calibración.

### Excelente

Representa una entrega sólida, con artefactos verificables, tres corridas,
herramienta real, trazas de proceso, análisis económico y documentación de
gobierno.

### Flojo

Representa una entrega incompleta: conserva algunos elementos formales, pero
tiene una sola corrida, no acredita una herramienta real y presenta debilidades
en proceso, economía y gobierno.

### Tramposo

Combina evidencia legítima con afirmaciones que no coinciden con el inventario
real.

El objetivo no es asignarle automáticamente cero puntos, sino verificar que el
evaluador pueda distinguir entre lo que el trabajo declara y lo que realmente
puede acreditar.

## Calibración final

Las tres corridas reales finales se realizaron con la misma versión de la
rúbrica, **v1.9**.

| Caso | Puntaje final | Veredicto | Puntaje propuesto por el modelo |
|---|---:|---|---:|
| Excelente | **92,50** | Excelente | 97,50 |
| Flojo | **25,00** | Crítico | 33,75 |
| Tramposo | **55,00** | Insuficiente | 63,75 |

En las tres corridas finales:

- `validacion_escala.ok = true`
- `validacion_escala.recalculos = []`

Los JSON completos están archivados en `calibracion/`.

La diferencia entre `puntaje_total_modelo` y `puntaje_total` se conserva
deliberadamente para hacer auditable la intervención de la capa determinista.

## Qué detectó el caso Tramposo

Durante la calibración, el caso Tramposo permitió detectar una debilidad
importante: un modelo podía interpretar una ruta mencionada en un documento como
si el archivo realmente existiera.

La versión final contrasta esas referencias con el inventario obtenido por
Python.

En la corrida final se detectaron, entre otras, referencias a artefactos
inexistentes como:

- `conectores/sheets_config.yaml`
- `prompts/system_prompt_v1.md`
- `logs/errores.md`
- `corridas/corrida_03/`

El repositorio declaraba tres corridas, pero solamente dos pudieron verificarse.

También declaraba Google Sheets API, pero no se encontró un artefacto suficiente
para considerarla una herramienta verificada.

Estas inconsistencias se registran en la salida en lugar de convertirse en una
penalización global automática.

## Qué falló durante el desarrollo

El sistema actual es resultado de varias correcciones surgidas de las pruebas.

Entre los problemas encontrados estuvieron:

- una carga inicial en GitHub que había destruido la estructura de carpetas de
  los casos;
- límites de lectura que podían dejar evidencia fuera de la evaluación;
- fallos al recuperar archivos desde GitHub;
- necesidad de soportar URLs de subcarpetas;
- errores de aritmética entre componentes, niveles y puntajes;
- diferencias entre el puntaje calculado por el modelo y la suma de las
  dimensiones;
- aceptación por parte del modelo de rutas declaradas pero inexistentes;
- variabilidad en componentes que requieren interpretación semántica.

Estos problemas no se ocultaron: se utilizaron como evidencia del proceso y
llevaron a modificar la rúbrica, el prompt y las validaciones de Python.

La evolución y los desacuerdos están documentados en `calibracion.md` y
`calibracion/`.

## Qué aprendimos

Construir un agente evaluador requiere más que escribir un buen prompt.

Fue necesario definir qué constituye evidencia, cómo tratar la ausencia de
evidencia, cómo registrar contradicciones y qué decisiones conviene dejar al
modelo o resolver mediante código.

La calibración mostró especialmente que **temperatura 0 no debe confundirse con
una garantía de identidad absoluta entre ejecuciones**. Por eso priorizamos la
estabilidad de las invariantes objetivas por encima de forzar una nota
predeterminada.

También aprendimos que un evaluador no debería castigar un trabajo completo por
impresión general. El caso Tramposo, por ejemplo, conserva los puntos de las
dimensiones donde sí presenta evidencia suficiente, aunque simultáneamente se
registren sus contradicciones.

## Estructura del repositorio

- `rubrica.md` — rúbrica ejecutable v1.9.
- `agente/` — system prompt y configuración del evaluador.
- `casos/` — casos Excelente, Flojo y Tramposo.
- `calibracion.md` — proceso de calibración, rondas, hallazgos y resultado final.
- `calibracion/` — corridas, desacuerdos y evidencia de calibración.
- `app.py` — aplicación Streamlit y controles deterministas.
- `comparacion_versiones.md` — comparación y consolidación de versiones.
- `PRUEBA_DE_FUEGO.md` — preparación para la prueba de fuego.
- `requirements.txt` — dependencias.

## Cómo ejecutar localmente

Instalar las dependencias:

```bash
pip install -r requirements.txt
```

Configurar `GEMINI_API_KEY` mediante Streamlit Secrets o como variable de
entorno.

Luego ejecutar:

```bash
streamlit run app.py
```

Finalmente, ingresar en la interfaz la URL pública del repositorio o subcarpeta
que se desea evaluar.

## Seguridad de credenciales

La API key no se almacena en el repositorio.

Los archivos locales de secretos se excluyen mediante `.gitignore`, y la
aplicación obtiene `GEMINI_API_KEY` desde Streamlit Secrets o desde una variable
de entorno.
