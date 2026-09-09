# Calibración

Evidencia de que las notas del corrector coinciden con el criterio humano del
grupo sobre los tres casos: qué notas esperábamos, qué salió al aplicar la
rúbrica, dónde no coincidía y qué se ajustó.

**Rúbrica aplicada:** la V2 de Anahí. Desde el 7/9 es la base de `rubrica.md`,
tras la consolidación de las tres versiones — ver `comparacion_versiones.md`.
Es la única de las tres que tiene anclas de puntaje discretas y aritmética de
conteo de componentes.

---

## Ronda 0 — reparación estructural, previa a toda corrección

Antes de correr nada apareció un defecto que invalidaba los tres casos.

### Qué pasó

Los casos se subieron con «Add files via upload» desde la web de GitHub, que
**no conserva carpetas**. Todo el árbol quedó aplastado: `corridas/`,
`prompts/`, `logs/`, `prompts/versiones/` y `herramienta/` dejaron de existir, y
los archivos homónimos quedaron sueltos en la raíz de cada caso, renombrados por
GitHub como `entrada (1).md`, `salida (2).json`, `reporte (1).md`.

### Por qué invalidaba la prueba

**R6 se disparaba en los tres casos por igual.** La regla construye el
inventario de rutas afirmadas y lo contrasta con el árbol real. Con la
estructura aplastada, *todas* las rutas de *todos* los casos eran inexistentes:
el excelente citaba trece rutas y ninguna existía.

Consecuencias medidas sobre el caso excelente:

| Dimensión | Con la estructura rota | Reconstruida | Diferencia |
|---|---|---|---|
| D1 · Sistema | N3 · 22,50 — R6 activa R2 y baja un nivel | N4 · 30,00 | +7,50 |
| D3 · Formato | N2 · 7,50 — R13 cuenta los archivos como parciales y R14 topea porque las tres corridas no eran verificables | N4 · 15,00 | +7,50 |
| **Total** | **77,50** | **92,50** | **+15,00** |

Y la trampa **T1 del tramposo quedaba muerta**: su discriminador es citar cuatro
rutas que no existen, pero si en los tres casos no existe ninguna, el corrector
no puede separar la trampa deliberada del accidente de carga.

### Qué se hizo

Se reconstruyó el árbol de los tres casos con `git mv`, según la estructura que
cada caso declara en su propio README. Cada archivo se ubicó por lo que él mismo
dice ser: `entrada.md` dice «Corrida 01», `entrada (1).md` dice «Corrida 02», y
las salidas coinciden por filtro y fecha. La reconstrucción es determinista, no
interpretativa.

**Las ausencias deliberadas se preservaron.** Verificado archivo por archivo:

| Ruta | Estado | Por qué |
|---|---|---|
| `tramposo/conectores/sheets_config.yaml` | no existe | Trampa T1 |
| `tramposo/corridas/corrida_03/` | no existe | Trampa T1 y T3 |
| `tramposo/logs/errores.md` | no existe | Trampa T1 |
| `tramposo/prompts/system_prompt_v1.md` | no existe | Trampa T1 |
| `flojo/prompts/user_prompt.md` | no existe | Falta declarada del caso flojo |

---

## Ronda 1 — aplicación de la rúbrica

**Qué es esta corrida y qué no es.** Es la rúbrica `rubrica_V2.md` aplicada a
mano, componente por componente, sobre los tres casos reconstruidos. **No es**
una ejecución del agente configurado con la API key. Sirve para verificar que la
rúbrica es aplicable y que los casos son alcanzables; **no reemplaza** la corrida
real del corrector, que sigue pendiente.

### Resultado

| | Excelente | Flojo | Tramposo |
|---|---|---|---|
| D1 · Sistema | 30,00 | 7,50 | 7,50 |
| D2 · Proceso | 25,00 | 6,25 | 0,00 |
| D3 · Formato | 15,00 | 3,75 | 7,50 |
| D4 · Económico | 11,25 | 3,75 | 15,00 |
| D5 · Gobierno | 11,25 | 3,75 | 3,75 |
| **Total** | **92,50** | **25,00** | **33,75** |
| Nota objetivo declarada | 92,50 | 25,00 | 33,75 |
| **Desvío** | **0,00** | **0,00** | **0,00** |

### Qué se verificó de forma independiente

No se aceptó la derivación de `NOTAS_DE_DISENO.md`: se recontó.

**Las seis funciones del contrato (§1.2), contadas sobre los archivos:**

| Caso | Funciones cubiertas | Componente | Coincide con lo declarado |
|---|---|---|---|
| Excelente | 6 de 6 — identidad, alcance, insumos, reglas duras, casos borde, formato | verificado | sí |
| Tramposo | 5 de 6 — falta comportamiento ante ambigüedad o fallo | parcial | sí |
| Flojo | 4 de 6 — falta alcance y falta comportamiento ante fallo | parcial | sí |

**D3 recontada sobre el árbol reconstruido:** el excelente cumple los cuatro
componentes —estructura completa, tres corridas con entradas realmente distintas
(sin filtro, España, 2021), entrada y salida cruda con fecha en cada una, e
instrucciones de ejecución paso a paso— y llega a N4 sin necesitar interpretación.

D2, D4 y D5 se aceptaron según la derivación de las notas de diseño, tras
verificar que las reglas citadas se corresponden con el texto de la rúbrica. **No
es una verificación independiente y hay que rehacerla en la ronda 2.**

---

## Ronda 1 a ciegas — 7/9 · Gonzalo

Primera corrección hecha por una persona del grupo con criterio propio, y no una
aplicación de la rúbrica por el mismo agente que la escribió. Se hizo con un
agente distinto, a propósito: el que construyó la rúbrica ya había puntuado los
tres casos y no podía volver a mirarlos sin contaminar el resultado.

Notas completas en `calibracion/ronda1-gonzalo.md`.

### Qué corrigió

El **caso excelente**, las cinco dimensiones: **92,50**. Coincide con la corrida de
calibración a mano, pero el conteo de componentes no coincidía, y eso importa más
que el total (ver abajo).

El flojo y el tramposo quedan pendientes, y está declarado: son cuatro de las
cinco celdas genuinamente ciegas.

### Lo que la ronda le hizo a la rúbrica

Tres versiones en una sesión, **cada una por un defecto encontrado corrigiendo, no
por revisión de escritorio**. Ese es el resultado que la ronda tenía que producir.

| Versión | Qué cerró | Lo destapó |
|---|---|---|
| v1.2 · R6-bis | R6 no distinguía un insumo de datos externo de un artefacto que el entregable debe producir, y penalizaba a un trabajo por no versionar su fuente aunque el consumo estuviera acreditado | `datos/bd_cursos.xlsx`, citado doce veces en el excelente y ausente del árbol |
| v1.3 · R6-ter | R6 no decía si una afirmación general de práctica entra al inventario. Dos correctores podían diferir en 3,75 puntos sobre la misma evidencia | «Cada corrida deja un log en `logs/`», con log para una de tres corridas |
| v1.4 · alcance por defecto | Tres reglas —R6, R17 y R20— no enunciaban sobre qué se aplican. Quedaban veinticuatro escritas con el mismo criterio | El USD 0,0562 del excelente, que no se reconstruye, y la pregunta de si R20 lo alcanza |

La de v1.4 es la importante: en lugar de seguir delimitando regla por regla, 0.6
ahora fija que **toda regla de corte se aplica solo sobre los elementos que su
enunciado nombra**, y que ante duda de alcance la regla no se aplica y se declara.
Con la relación explícita con 8.1, que gobierna la duda sobre la *evidencia* y la
resuelve en la dirección contraria.

### Lo que la ronda encontró en el trabajo mismo

- **`rubrica.md` citaba mal su propia regla en tres lugares.** La condición del
  componente Perímetro remitía a R25 (Responsable nominal) para las acciones
  prohibidas, que son R26, y dos líneas del changelog de v1.0 hacen lo mismo. Un
  corrector que siguiera el puntero aterrizaba en la regla de otro componente y
  podía dejar Perímetro verificado de más en un entregable sin lista de acciones
  prohibidas. Corregido el puntero vivo.
- **La corrida de calibración del excelente tenía el inventario de 6.3 vacío.** No
  registraba el `.xlsx`. Bajo v1.1 el total real era 85 y no 92,50: la corrida
  daba el número correcto por una omisión, no por un análisis. Re-emitida dos
  veces, como corridas 1-bis y 1-ter del registro.
- **`app.py` no compilaba.** Un `SyntaxError` introducido esa misma noche dejó la
  app sin levantar durante horas sin que nadie lo notara.

### El desacuerdo que la ronda produjo, que es el entregable

En **D4 del excelente**, las dos correcciones dieron 11,25 y **el conteo de
componentes no coincidió**: la corrida a mano daba `Consumo medido` verificado
(3,5) y la ronda lo dio parcial (3,0). Los dos truncan a N3, así que el puntaje
tapaba la diferencia.

Resolvió la rúbrica y no el criterio: el ejemplo 4.4 fija esa misma situación
probatoria como **parcial** en `casos/tramposo` —tabla de tokens con origen
declarado en la consola de la API, consola ausente del entregable, P9— y el
excelente afirma lo mismo con casi las mismas palabras. **P4 obliga a puntuarlos
igual.** Corregida la corrida.

Vale por sí solo: dos correcciones pueden coincidir en el puntaje y estar
midiendo distinto, y el total no lo muestra. Es el argumento de por qué la rúbrica
exige el conteo de componentes y no solo la nota.

### Dudas registradas

Tres, con la decisión tomada y con lo que costaría resolverlas al revés:

1. **R17 caza edición cosmética, no fabricación.** Las tres salidas del excelente
   cierran al céntimo en todas las agregaciones, que es lo que uno esperaría de
   una planilla más que de tres corridas de un LLM. **No se usó para bajar el
   nivel**: la rúbrica no da ninguna palanca para hacerlo y 4.4 advierte contra
   puntuar por impresión general.
2. **P9 aplicado con dos varas en la misma corrección**, verificando en D1 con un
   log que también podría haberse tipeado a mano y bajando en D4 por falta de
   artefacto.
3. **El USD 0,0562 y R20.** Se resolvió que no activa: el número no contradice a
   ningún otro, es indeterminado y no inconsistente. Si se activara, D4 caería a
   7,50.

---

## Desacuerdos abiertos

Los tres son decisiones del grupo, no defectos.

### 1 · El tramposo saca más que el flojo — ~~33,75 contra 25,00~~ **DISUELTO**

> **Ya no ocurre.** Al corregirse un error de aritmética de la corrida —`nivel_final`
> N2 con conteo 1,5, que es N1: R14 es un techo y R17 degrada, y ninguna de las dos
> sube— el tramposo bajó de 33,75 a **22,50**, y en una segunda corrección a **18,75**, quedando **por debajo** del flojo, que
> saca 25,00. El desacuerdo se disolvió sin necesidad de decidirlo, y `app.py`
> ahora valida esa condición para que no vuelva a pasar. Se conserva el análisis
> porque la pregunta de fondo sigue siendo buena y puede volver con otros casos.


Ya identificado en `casos/NOTAS_DE_DISENO.md`, y es el desacuerdo más valioso que
tiene el trabajo.

La rúbrica puntúa evidencia, y el tramposo tiene más evidencia real que el flojo
—además de la falsa—: su análisis económico es genuinamente riguroso y se lleva
los 15 puntos que el flojo no tiene.

**Es probable que el criterio humano diga lo contrario:** que mentir debería
costar más que hacer poco. Dos salidas, las dos defendibles:

- **Sostener la rúbrica.** La dimensión que el tramposo hizo bien la hizo bien.
  Penalizar el conjunto sería puntuar por impresión general, que es exactamente
  lo que la rúbrica prohíbe.
- **Escribir una regla de integridad agregada.** Por ejemplo: N discrepancias
  confirmadas en el inventario de R6 bajan un nivel adicional en todas las
  dimensiones. Hay que escribirla, no improvisarla durante la corrección.

**Lo que no hay que hacer es dejarla sin decidir.** Si el jueves alguien pregunta
por qué el tramposo saca más que el flojo, «lo pensamos y elegimos sostener la
rúbrica porque X» es una respuesta fuerte; quedarse callado, no.

Vale notar que la consigna pide «puntuar alto al primero, bajo al segundo y
**detectar** al tercero» — no pide que el tramposo puntúe por debajo del flojo. La
detección vive en `contradicciones` y `alertas_integridad`, no en el puntaje.

### 2 · La regla de manipulación no tiene consecuencia

`rubrica.md` §5 y el system prompt v1 dicen que un intento de manipular al
corrector **no modifica automáticamente la nota**. Es prudente y evita el doble
castigo. Pero tiene un costo: **intentarlo sale gratis.** Un trabajo puede probar
la inyección y no perder nada.

### 3 · Ninguna de las tres rúbricas tiene ejemplos alto/bajo — **RESUELTO**

> **Hecho en la consolidación.** `rubrica.md` tiene ahora el par alto/bajo explícito
> por dimensión, en §§1.5, 2.4, 3.5, 4.4 y 5.4, cada uno citando el caso concreto y
> el archivo donde se verifica. Se conserva el análisis original abajo.


El enunciado del parcial los pide con esas palabras: «ejemplos de qué merece un
nivel alto y uno bajo en cada dimensión». `rubrica_V2.md` tiene una columna
*Ejemplo* por nivel en cada escala, que es lo más cerca que está ninguna de las
tres — pero es un ejemplo por nivel, no el par alto/bajo explícito por dimensión.

---

## Ronda 2 — corridas reales del evaluador

El 8/9 se ejecutó finalmente el evaluador desplegado sobre los tres casos de
calibración utilizando la misma arquitectura y la rúbrica v1.9.

A diferencia de la Ronda 1, estas no son aplicaciones manuales de la rúbrica:
son ejecuciones reales de la aplicación.

### Resultados finales

| Caso | Puntaje final | Veredicto | Puntaje propuesto por el modelo |
|---|---:|---|---:|
| Excelente | **92,50** | Excelente | 97,50 |
| Flojo | **25,00** | Crítico | 33,75 |
| Tramposo | **55,00** | Insuficiente | 63,75 |

En las tres ejecuciones finales:

- `validacion_escala.ok = true`
- `validacion_escala.recalculos = []`
- se utilizó la misma rúbrica v1.9;
- los puntajes finales respetaron los valores de ancla definidos por la rúbrica.

---

## Qué confirmó la corrida real

### Caso Excelente

El caso Excelente obtuvo **92,50/100**.

Resultado por dimensión:

| Dimensión | Puntaje |
|---|---:|
| D1 · Sistema completo | 30,00 / 30 |
| D2 · Proceso documentado | 25,00 / 25 |
| D3 · Formato y reproducibilidad | 15,00 / 15 |
| D4 · Análisis económico | 11,25 / 15 |
| D5 · Gobierno y riesgo | 11,25 / 15 |
| **Total** | **92,50 / 100** |

Se verificaron tres corridas reales, una herramienta real, trazas de proceso,
estructura reproducible y documentación de gobierno.

Los faltantes fueron acotados: proyección semanal en D4 y contingencia ante
ausencia del responsable en D5.

El resultado coincide con los **92,50 puntos** de la referencia histórica de
calibración.

### Caso Flojo

El caso Flojo obtuvo **25,00/100**.

Resultado por dimensión:

| Dimensión | Puntaje |
|---|---:|
| D1 · Sistema completo | 7,50 / 30 |
| D2 · Proceso documentado | 6,25 / 25 |
| D3 · Formato y reproducibilidad | 7,50 / 15 |
| D4 · Análisis económico | 0,00 / 15 |
| D5 · Gobierno y riesgo | 3,75 / 15 |
| **Total** | **25,00 / 100** |

El evaluador verificó solamente una corrida y no encontró una herramienta real.
También detectó ausencia de trazas de iteraciones, debilidad del análisis
económico y gobierno insuficiente.

El resultado coincide con los **25,00 puntos** de la referencia histórica.

### Caso Tramposo

El caso Tramposo obtuvo **55,00/100** y fue clasificado como Insuficiente.

Resultado por dimensión:

| Dimensión | Puntaje |
|---|---:|
| D1 · Sistema completo | 15,00 / 30 |
| D2 · Proceso documentado | 6,25 / 25 |
| D3 · Formato y reproducibilidad | 7,50 / 15 |
| D4 · Análisis económico | 15,00 / 15 |
| D5 · Gobierno y riesgo | 11,25 / 15 |
| **Total** | **55,00 / 100** |

El resultado no fue forzado para coincidir con una nota objetivo histórica.

El caso contiene evidencia legítima en algunas dimensiones, especialmente en
análisis económico y gobierno, y por eso conserva esos puntos. Al mismo tiempo,
el evaluador detectó las declaraciones que no podían verificarse.

---

## Control determinista incorporado durante la calibración

Las primeras pruebas del caso Tramposo revelaron una debilidad importante:
el modelo podía aceptar como evidencia una ruta simplemente porque estaba
mencionada dentro de README.md o DECISIONES.md.

Eso permitía que una declaración textual fuera confundida con la existencia
real de un artefacto.

Para corregirlo se agregó una capa de control determinista en Python.

La arquitectura final quedó conceptualmente así:

`Repositorio -> inventario Python -> evaluación semántica -> control de evidencia -> normalización determinista -> resultado`

Python construye el inventario de archivos efectivamente analizados y permite
contrastar las rutas utilizadas como evidencia.

Si una ruta citada como evidencia no existe en un paquete completo:

1. no puede considerarse evidencia válida;
2. se registra la discrepancia;
3. se solicita una revisión de la evaluación;
4. si la evidencia inexistente persiste, la corrida se considera inválida.

El objetivo de esta capa no es reemplazar el juicio semántico del modelo, sino
reservar para código determinista aquellas verificaciones que no requieren
interpretación.

---

## Detección del caso Tramposo

En la corrida final se verificaron **dos corridas reales**, aunque el repositorio
declaraba tres.

También se declaró el uso de Google Sheets API, pero no se encontraron
artefactos suficientes para considerarla una herramienta verificada.

Además se registraron referencias a artefactos inexistentes:

- `conectores/sheets_config.yaml`
- `prompts/system_prompt_v1.md`
- `logs/errores.md`
- `corridas/corrida_03/`

Estas inconsistencias quedan registradas en
`verificaciones.contradicciones` y
`verificaciones.afirmaciones_no_verificadas`.

La existencia de una contradicción no produce una penalización global
automática. Afecta solamente los componentes cuya evidencia deja de estar
verificada.

Esto preserva el principio de puntuar evidencia componente por componente y
evita convertir la evaluación en una impresión general sobre el trabajo.

---

## Diferencia entre propuesta del modelo y puntaje final

Para facilitar la auditoría, el sistema conserva el puntaje originalmente
propuesto por el modelo en `puntaje_total_modelo`.

| Caso | Puntaje modelo | Puntaje final | Diferencia |
|---|---:|---:|---:|
| Excelente | 97,50 | 92,50 | -5,00 |
| Flojo | 33,75 | 25,00 | -8,75 |
| Tramposo | 63,75 | 55,00 | -8,75 |

Esto muestra por qué el puntaje final no se delega completamente al LLM.

El modelo realiza la interpretación semántica de la evidencia. Python controla
aspectos objetivos como la existencia de determinadas evidencias, la aritmética
de componentes, los niveles, los puntajes de ancla y la suma final.

---

## Hallazgo sobre estabilidad

Durante el desarrollo el caso Tramposo produjo puntajes diferentes entre
ejecuciones aun utilizando temperatura 0.

En lugar de modificar la rúbrica para obligar al modelo a devolver una nota
predeterminada, se decidió fortalecer las invariantes verificables.

Por lo tanto, la calibración no exige identidad absoluta del puntaje en todas
las ejecuciones. Exige que se mantengan estables los hechos objetivos:

- una ruta inexistente no puede utilizarse como evidencia válida;
- las corridas declaradas se contrastan con las corridas verificables;
- una herramienta declarada no se considera verificada solo por aparecer
  mencionada;
- los puntajes finales utilizan los valores de ancla de la rúbrica;
- las contradicciones quedan registradas para auditoría.

Los componentes que requieren interpretación semántica pueden conservar cierto
grado de variabilidad.

---

## Ronda 3 — calibración final con rúbrica v2.0

El 9/9 se volvió a ejecutar el evaluador sobre los tres casos después de
consolidar la rúbrica v2.0 y corregir los defectos detectados durante las
rondas anteriores.

Estas ejecuciones constituyen la referencia vigente del evaluador.

No se modificó la rúbrica para perseguir una nota objetivo. Los cambios entre
v1.9 y v2.0 se conservan como resultado de aplicar la versión nueva de la
rúbrica y de fortalecer los controles del evaluador.

Los archivos de referencia son:

- `calibracion/corrida-final-excelente-v2.0.json`
- `calibracion/corrida-final-flojo-v2.0.json`
- `calibracion/corrida-final-tramposo-v2.0.json`

### Resultados v2.0

| Caso | Puntaje final | Veredicto |
|---|---:|---|
| Excelente | **96,25** | Excelente |
| Flojo | **22,50** | Crítico |
| Tramposo | **55,00** | Insuficiente |

Los tres casos mantienen el comportamiento esperado para el propósito de la
calibración:

- el caso Excelente obtiene un puntaje alto;
- el caso Flojo obtiene un puntaje bajo;
- el caso Tramposo es detectado por sus inconsistencias sin aplicar una
  penalización global automática por el solo hecho de contenerlas.

---

## Qué cambió respecto de v1.9

| Caso | v1.9 | v2.0 | Diferencia |
|---|---:|---:|---:|
| Excelente | 92,50 | 96,25 | +3,75 |
| Flojo | 25,00 | 22,50 | -2,50 |
| Tramposo | 55,00 | 55,00 | 0,00 |

Estas diferencias no fueron corregidas artificialmente para recuperar los
puntajes anteriores.

La calibración se utiliza para detectar ambigüedades o defectos en la rúbrica
y en el evaluador, no para obligar al agente a reproducir una nota
predeterminada.

En particular, durante esta ronda se detectó una interpretación inconsistente
de D3 en el caso Flojo. El `system_prompt.md` fue aclarado para exigir que los
estados `verificado`, `parcial` y `no_verificado` se apliquen según la
definición de la rúbrica vigente y que las reglas de corte se apliquen después
del conteo, sin hardcodear el caso ni su puntaje.

La nueva corrida de Flojo produjo D3 = N2 = 7,50 y el resultado global quedó en
22,50.

---

## Caso Excelente · v2.0

Resultado final: **96,25/100 — Excelente**.

La corrida verificó tres ejecuciones, herramienta real, proceso documentado,
estructura reproducible y evidencia de gobierno.

El control determinista también intervino de forma auditable: el modelo había
propuesto un puntaje que no coincidía con el ancla correspondiente al nivel
final de una dimensión. Python normalizó el puntaje al valor de ancla y registró
el cambio en `validacion_escala.recalculos`.

Esto muestra que una corrida puede ser semánticamente producida por el modelo
pero seguir sometida a controles mecánicos antes de convertirse en resultado
final.

---

## Caso Flojo · v2.0

Resultado final: **22,50/100 — Crítico**.

El caso conserva evidencia mínima pero presenta faltantes importantes de
proceso, reproducibilidad, análisis económico y gobierno.

Durante la calibración apareció una discrepancia de interpretación en D3. Una
primera ejecución v2.0 produjo un estado demasiado restrictivo para uno de sus
componentes.

En lugar de fijar un puntaje objetivo para `casos/flojo`, se aclaró en el
system prompt el procedimiento general:

1. evaluar los cuatro componentes de D3 por separado;
2. aplicar las definiciones de `verificado`, `parcial` y `no_verificado` de la
   rúbrica vigente;
3. realizar el conteo;
4. obtener el nivel por truncado;
5. aplicar recién después las reglas de corte.

La nueva ejecución dejó D3 en N2 = 7,50.

El total final de 22,50 no fue modificado para hacerlo coincidir con los 25,00
de v1.9.

---

## Caso Tramposo · v2.0

Resultado final: **55,00/100 — Insuficiente**.

El caso cumplió su función de prueba adversarial.

El evaluador detectó como inexistentes las referencias a:

- `conectores/sheets_config.yaml`
- `prompts/system_prompt_v1.md`
- `logs/errores.md`

Las referencias quedaron registradas en
`verificaciones.afirmaciones_no_verificadas` y
`verificaciones.contradicciones`.

La herramienta Google Sheets API fue declarada pero no quedó verificada por
evidencia suficiente.

Al mismo tiempo, el evaluador preservó el puntaje de las dimensiones que sí
tenían evidencia legítima. En particular, el caso mantuvo evidencia fuerte en
formato/reproducibilidad y análisis económico.

Esto es deliberado: la rúbrica evalúa evidencia por componente y no aplica una
penalización global automática por la existencia de una contradicción.

`alertas_integridad` quedó vacío en esta corrida. Las afirmaciones falsas fueron
tratadas como contradicciones de evidencia y no como instrucciones dirigidas al
evaluador. Esa distinción se conserva en la calibración en lugar de alterar el
resultado después de observarlo.

---

## Control modelo + Python observado en v2.0

Las corridas finales confirman la separación de responsabilidades de la
arquitectura:

`Repositorio -> inventario Python -> evaluación semántica -> control de evidencia -> normalización determinista -> resultado`

El modelo interpreta semánticamente la evidencia y propone estados, reglas de
corte y justificaciones.

Python controla los aspectos que pueden verificarse mecánicamente, entre ellos:

- inventario de archivos efectivamente leídos;
- detección de determinadas referencias a rutas inexistentes;
- conteo de componentes;
- nivel derivado del conteo;
- puntajes discretos de ancla;
- suma del puntaje total;
- registro de recalculos cuando la propuesta del modelo no coincide con las
  invariantes deterministas.

La corrida de Tramposo mostró este mecanismo de manera explícita: Python
recalculó el conteo y el nivel por conteo de D1 a partir de los estados que el
propio modelo había emitido, dejando el ajuste registrado para auditoría.

---

## Comparación humano–agente

La calibración humana no se reemplaza por estas corridas automáticas.

Las primeras rondas documentan la aplicación manual de la rúbrica, la revisión
de Gonzalo y los desacuerdos que llevaron a modificar reglas y ejemplos. Esa
evidencia se conserva arriba como historial de calibración.

La comparación no se interpreta como obligación de igualdad numérica entre
persona y agente. Se utiliza para localizar desacuerdos y decidir si provienen
de:

- una ambigüedad de la rúbrica;
- una interpretación semántica discutible;
- un error aritmético;
- evidencia insuficiente;
- o una diferencia de criterio que debe permanecer documentada.

Un ejemplo concreto fue D4 del caso Excelente: dos correcciones podían llegar
al mismo puntaje con estados de componentes diferentes. Ese desacuerdo llevó a
revisar la definición probatoria y mostró por qué comparar únicamente el total
es insuficiente.

Otro ejemplo fue D3 del caso Flojo en v2.0: la primera corrida automática
interpretó un componente de manera demasiado restrictiva. La corrección se hizo
sobre la regla general de interpretación y no sobre el puntaje objetivo del
caso.

Las rondas humanas que no llegaron a completarse se mantienen declaradas como
tales. No se reconstruyen retrospectivamente como revisiones ciegas.

---

## Criterio de estabilidad

Temperatura 0 reduce variabilidad, pero no convierte una evaluación semántica
de un LLM en una función completamente determinista.

Por eso la estabilidad exigida se concentra en las invariantes que sí pueden
controlarse:

- una ruta inexistente no debe transformarse en evidencia válida;
- una herramienta declarada no equivale automáticamente a una herramienta
  verificada;
- las cantidades verificadas deben derivarse de artefactos reales;
- el conteo de componentes debe respetar la aritmética de la rúbrica;
- el nivel final no puede superar indebidamente el nivel permitido;
- los puntajes deben pertenecer a las anclas oficiales;
- el total debe coincidir con la suma de las cinco dimensiones;
- los ajustes deterministas deben quedar registrados.

La variabilidad semántica residual se documenta en vez de ocultarse.

---
## Revisión humana final del grupo

Como cierre de la calibración, el grupo realizó una revisión humana final sobre
los tres casos utilizando la rúbrica v2.0 ya estabilizada.

Esta revisión no fue ciega: al momento de hacerla ya se conocían las corridas
automáticas previas. Se declara así para no presentar como independiente una
evaluación que no lo fue.

La comparación final fue:

| Caso | Evaluador automático | Criterio humano del grupo | Acuerdo |
|---|---:|---:|---|
| Excelente | 96,25 | Alto / Excelente | Sí, en la clasificación general |
| Flojo | 22,50 | Bajo / Crítico | Sí, en la clasificación general |
| Tramposo | 55,00 | Insuficiente, con evidencia válida y contradicciones detectadas | Sí, en el comportamiento esperado |

El grupo no tomó estas referencias como notas objetivo que el sistema debiera
reproducir exactamente.

En el caso Excelente, el criterio humano coincidió en que debía ubicarse en el
tramo superior de la escala.

En el caso Flojo, el criterio humano coincidió en que debía quedar claramente en
el tramo bajo por sus faltantes sustanciales.

En el caso Tramposo, el criterio humano coincidió en que las contradicciones
debían detectarse, pero que la evidencia legítima de otras dimensiones no debía
eliminarse mediante una sanción global automática.

Los desacuerdos más relevantes aparecieron durante las rondas previas en la
interpretación de componentes específicos, especialmente D3 y D4. Esos
desacuerdos llevaron a aclarar reglas de la rúbrica, el system prompt y los
controles deterministas.

La calibración final se considera satisfactoria porque agente y criterio humano
coinciden en la clasificación y en el comportamiento esperado de los tres
casos, aunque no se exige identidad exacta de puntaje como condición de validez.

## Conclusión de calibración

La referencia vigente de calibración para la rúbrica v2.0 queda formada por:

| Caso | Resultado final |
|---|---:|
| Excelente | **96,25/100 — Excelente** |
| Flojo | **22,50/100 — Crítico** |
| Tramposo | **55,00/100 — Insuficiente** |

Los tres casos cumplen funciones diferentes y no se utilizan como notas que el
evaluador deba memorizar:

**Excelente** verifica que un trabajo con evidencia abundante, trazable y
reproducible pueda obtener un puntaje alto.

**Flojo** verifica que un trabajo con evidencia insuficiente y documentación
débil obtenga un puntaje bajo.

**Tramposo** verifica que las declaraciones incompatibles con el inventario
sean detectadas y no puedan utilizarse como evidencia, sin borrar el mérito de
los componentes que sí poseen evidencia legítima.

Las diferencias con v1.9 se mantienen explícitas. No se ajustó el evaluador
para reproducir puntajes históricos.

Por lo tanto, las tres corridas v2.0 constituyen la referencia actual de
calibración del evaluador.

Cualquier cambio posterior en `rubrica.md`, `agente/system_prompt.md` o en las
reglas deterministas de `app.py` requiere volver a ejecutar los tres casos antes
de reemplazar estas referencias.
