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
> sube— el tramposo bajó de 33,75 a **22,50** y quedó **por debajo** del flojo, que
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

## Pendiente para la ronda 2

1. **Correr el agente de verdad**, con la API key, sobre los tres casos. Ninguna
   corrida real existe todavía en el repositorio, y el criterio de 25 puntos pide
   que el corrector «corre sobre un repo real y devuelve el formato completo».
2. **Corrección a ciegas de los cuatro** sobre los mismos casos, cada uno por
   separado, antes de mirar las notas del otro. Lo de acá arriba es una
   aplicación de la rúbrica, no cuatro criterios humanos independientes.
3. **Verificar D2, D4 y D5 de forma independiente**, como se hizo con D1 y D3.
4. **Decidir los tres desacuerdos abiertos** y dejar la decisión escrita.
5. **Prueba de determinismo:** el mismo caso tres veces, con el diff de las tres
   salidas.

---

## Corridas del corrector — 6/9

Las tres primeras ejecuciones del corrector sobre los tres casos, con el JSON
completo del esquema de la CAPA 6. Archivadas en `calibracion/`.

| Caso | Corrida | Nota objetivo | Desvío |
|---|---|---|---|
| Excelente | **92,50** · Excelente | 92,50 | **0,00** |
| Flojo | **25,00** · Crítico | 25,00 | **0,00** |
| Tramposo | **22,50** · Crítico | 33,75 | **−11,25** |

Dos de tres con desvío cero. El tercero acumula **dos desacuerdos independientes**,
los dos documentados:

- **D1 · 7,50 puntos** — `calibracion/desacuerdo-D1-tramposo.md`. Cuánto vale
  `output_estructurado` cuando el trabajo declara un JSON y entrega prosa.
- **D3 · 3,75 puntos** — `calibracion/desacuerdo-D3-tramposo.md`. Las notas de
  diseño acreditan instrucciones de ejecución que no existen en ningún archivo.

Las otras tres dimensiones coinciden exactamente.

### Lo que las corridas confirman

**El corrector discrimina.** 92,50 contra 22,50 son 70 puntos de separación entre
dos casos que comparten dominio, extensión y calidad de prosa. La única diferencia
entre ellos es que uno tiene los artefactos y el otro los declara.

**No castiga en bloque.** El tramposo se lleva los 15 puntos completos de análisis
económico, que es la única dimensión que hizo bien. Si el corrector hubiera bajado
también esa, tendríamos un evaluador que puntúa por impresión general — que es
exactamente lo que la rúbrica prohíbe.

**No inventa fraude donde no lo hay.** El caso flojo salió con `contradicciones`
vacío y `alertas_integridad` vacío. Es el chequeo de falsos positivos que
`casos/NOTAS_DE_DISENO.md` marca como el más importante de ese caso.

**Detecta al tramposo por evidencia, no por tono.** Cuatro contradicciones
registradas —las cuatro rutas inexistentes— y una alerta de integridad por la
apelación a la simpatía, con efecto declarado nulo sobre el puntaje.

### Aclaración sobre estas corridas

Son la aplicación del system prompt sobre cada caso, componente por componente,
con la salida en el esquema completo. **No se ejecutaron a través del modelo y la
temperatura que fija `agente/configuracion`.** Según la sección 6 de ese documento,
cada corrida debe archivarse con el modelo y la temperatura usados: o se repiten
con la configuración oficial, o se registra cuál fue la de estas.

Lo que sí cambia respecto de ayer: antes no existía ninguna corrida y ahora hay
tres, con un desacuerdo encontrado.
