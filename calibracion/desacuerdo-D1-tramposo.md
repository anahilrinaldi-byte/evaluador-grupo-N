# Desacuerdo · D1 del caso tramposo

**7,50 puntos de diferencia, y sale de una sola decisión de componente.**

| | D1 · Sistema completo |
|---|---|
| Nota objetivo declarada en `casos/NOTAS_DE_DISENO.md` | **N1 · 7,50** |
| Corrida del corrector (`corrida-tramposo.json`) | **N0 · 0,00** |
| Diferencia | **7,50** |

Es el único punto donde las dos evaluaciones difieren. Las otras cuatro dimensiones
coinciden exactamente.

## De dónde sale

Las dos evaluaciones aplican la misma cadena: conteo de componentes → nivel →
**R2 baja un nivel** porque R6 encontró cuatro rutas afirmadas que no existen.

La diferencia está un paso antes: **cuánto vale el componente `output_estructurado`.**

| | Componente | Conteo | Nivel | Tras R2 |
|---|---|---|---|---|
| Notas de diseño | verificado (1) | 2,0 | N2 | **N1 · 7,50** |
| Corrida | parcial (0,5) | 1,5 | N1 | **N0 · 0,00** |

## Las dos lecturas, y las dos se sostienen

La condición de verificación de V2 dice: *«Existe formato de salida declarado **y**
dos o más corridas que lo satisfacen con la misma estructura. Se valida campo por
campo.»* Son dos requisitos unidos por una «y».

**Lectura A — verificado.** `prompts/system_prompt.md` declara el formato: «Devolvés
un JSON con esta estructura». Y hay dos corridas que comparten estructura entre sí,
que es lo que la condición pide literalmente: *la misma* estructura, no
necesariamente *la declarada*.

**Lectura B — parcial.** El formato está declarado pero **ninguna corrida lo
satisface**: los dos `reporte.md` son informes en prosa markdown, no el JSON del
contrato. Se cumple la primera mitad de la condición y no la segunda, que es la
definición exacta de componente parcial.

## Por qué importa más allá de este caso

No es un detalle de siete puntos. Es la pregunta de **si el output estructurado se
verifica contra el formato que el trabajo declaró, o solo contra sí mismo.**

Un trabajo que declara un esquema JSON y después entrega informes redactados está
justamente en el hueco entre las dos lecturas. Y el domingo 13, cuando se corrijan
los finales de verdad, ese caso va a aparecer: es exactamente lo que hace un equipo
que escribió el contrato al principio y después no lo respetó.

## Recomendación

**Adoptar la lectura B y escribirlo en la condición.** Cambiar el texto de
`rubrica_V2.md` §1.1 de «dos o más corridas que lo satisfacen» a **«dos o más
corridas que satisfacen el formato declarado, validado campo por campo»**.

Motivo: si el output se valida solo contra sí mismo, la dimensión deja de medir que
el sistema respeta su propio contrato y pasa a medir únicamente que sea consistente.
Dos informes igual de desprolijos pasarían.

**Consecuencia si se adopta:** la nota objetivo del tramposo baja 7,50 puntos y
hay que actualizar `casos/NOTAS_DE_DISENO.md`. El resto de los objetivos no cambia.

**Y un efecto colateral que conviene mirar:** el tramposo se acerca al flojo en vez
de superarlo con holgura. Sumado al desacuerdo de D3, la corrida lo deja en 22,50
contra los 25,00 del flojo: **el resultado contraintuitivo se da vuelta.** El que
hizo poco y lo dijo termina por encima del que hizo poco y dijo que hizo mucho.

---

**Quién decide:** la lente de construcción, que es la dueña de D1. Antes de la ronda
a ciegas del grupo, porque cambia una nota objetivo.
