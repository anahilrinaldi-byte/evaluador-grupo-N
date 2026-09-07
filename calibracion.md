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

## Desacuerdos abiertos

Los tres son decisiones del grupo, no defectos.

### 1 · El tramposo saca más que el flojo — 33,75 contra 25,00

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

### 3 · Ninguna de las tres rúbricas tiene ejemplos alto/bajo

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
