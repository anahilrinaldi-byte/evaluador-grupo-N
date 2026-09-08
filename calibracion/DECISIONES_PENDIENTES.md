# Tres decisiones pendientes

**Para Anahí.** Cada una cambia una nota objetivo, así que **hay que resolverlas
antes de que nadie corrija a ciegas** — si no, las notas contra las que se compara
cambian a mitad de camino.

Están las tres con sus dos opciones y la consecuencia de cada una. **Marcá una por
decisión y commiteá este archivo.** El detalle completo está en los documentos que
se citan; acá está lo justo para elegir.

---

## Decisión 1 · D1 del tramposo — 7,50 puntos

`desacuerdo-D1-tramposo.md`

**La pregunta.** El componente `output_estructurado` pide «formato de salida
declarado **y** dos o más corridas que lo satisfacen con la misma estructura».

El tramposo declara un JSON en su system prompt y entrega **dos informes en prosa
markdown**. ¿Eso cumple la condición?

| | Lectura | Componente | D1 queda en |
|---|---|---|---|
| **A** | Las dos corridas comparten estructura entre sí, que es lo que la condición pide literalmente | verificado | N1 · 7,50 |
| **B** | El formato está declarado pero ninguna corrida lo satisface: se cumple media condición | parcial | N0 · 0,00 |

**Lo que está en juego más allá del caso:** si el output se valida solo contra sí
mismo, la dimensión deja de medir que el sistema respete su propio contrato. Un
equipo que escribe el contrato al principio y después no lo respeta cae justo acá,
y va a aparecer el domingo 13.

- [ ] **A** — verificado. La nota objetivo del tramposo no cambia por esta decisión.
- [x] **B** — parcial. Hay que cambiar el texto de la condición en `rubrica.md` §1.1 a «dos o más corridas que satisfacen **el formato declarado**, validado campo por campo».

> **Antes de elegir, un hecho que hay que tener a la vista: la premisa de A no se
> cumple.** A dice que «las dos corridas comparten estructura entre sí». Verificado
> encabezado por encabezado, **no la comparten**: de cinco secciones cada una,
> comparten dos.
>
> | | `corrida_01` | `corrida_02` |
> |---|---|---|
> | Comunes | Resumen ejecutivo · Alertas | Resumen ejecutivo · Alertas |
> | Propias | Margen por país · Margen por tipo de curso · Cartera impaga | Ranking de comerciales · Cartera impaga por cliente · Observación sobre comisiones |
>
> O sea que el output del tramposo no satisface ni el formato declarado —el JSON de
> `prompts/system_prompt.md:37`— ni el formato de la otra corrida. Bajo cualquiera
> de las dos lecturas el componente **no puede ir a verificado**.
>
> Eso no vacía tu decisión, la mueve: ya no elegís entre dos lecturas defendibles,
> sino **si además hay que cambiar el texto de la condición en §1.1** para que un
> caso futuro —dos corridas consistentes entre sí y ninguna fiel al contrato
> declarado— quede resuelto de antemano. Ese caso es el que va a aparecer el
> domingo 13 y la rúbrica hoy no lo resuelve.
>
> `calibracion/corrida-tramposo.json` ya tiene `output_estructurado` en **parcial**,
> que es el resultado compatible con lo verificado.

---

## Decisión 2 · D3 del tramposo — 3,75 puntos

`desacuerdo-D3-tramposo.md`

**El hecho, verificado.** Las notas de diseño justifican N2 con «Estructura e
instrucciones OK». Pero **el tramposo no tiene instrucciones de ejecución en
ningún archivo**: ni un comando, ni una dependencia, ni qué modelo usar. Sus nueve
secciones cubren todo menos cómo correrlo. La sección 9 se llama «Estructura del
repositorio» y es un árbol de archivos —que además lista cuatro rutas que no
existen, porque es parte de la trampa T1.

Esta no es una diferencia de lectura: **es un componente acreditado sin evidencia.**
R1 dice que solo cuenta lo abrible.

- [x] **A** — corregir la nota objetivo a **N1 · 3,75** y actualizar `casos/NOTAS_DE_DISENO.md`.
- [ ] **B** — sostener N2 · 7,50, explicando qué evidencia acredita el componente de instrucciones.

> **YA IMPLEMENTADA COMO A — solo falta que la confirmes o la objetes.**
> `calibracion/corrida-tramposo.json` tiene D3 en **N1 · 3,75** y el total en
> **22,50**, y `calibracion.md` ya registra que el tramposo quedó por debajo del
> flojo. No es una decisión que tengas que investigar: el hecho está verificado
> —el tramposo no tiene instrucciones de ejecución en ningún archivo— y R1 dice
> que solo cuenta lo abrible.
>
> Si estás de acuerdo, no hay nada que hacer. Si preferís B, hay que decir qué
> evidencia acredita el componente y revertir la corrida y el registro.

**Consecuencia si va A:** el tramposo baja a **22,50** y queda **por debajo** del
flojo, que saca 25,00. El resultado contraintuitivo que identificaste —«el tramposo
saca más que el flojo»— **se corrige solo**, sin necesidad de escribir la regla de
integridad agregada que estaba en discusión. El problema era una nota mal
calculada, no la rúbrica.

---

## Decisión 3 · ¿P9 es demasiado fuerte para D4?

`impacto-P9-en-D4.md`

**El principio injertado de la v3** dice que una tabla de resultados es evidencia
solo si existe el artefacto que la produjo. **Ningún caso adjunta el contador de
tokens**, así que `consumo_medido` queda parcial o peor en los tres.

Aplicado literal, casi ningún trabajo final va a adjuntar un volcado del contador,
y **D4 queda con techo práctico en N3**.

- [x] **A** — sostener P9 como está. Quien mide de verdad puede guardar una captura: es barato y es la diferencia entre medir y decir que se midió.
- [ ] **B** — escribir una excepción en la condición de `consumo_medido`: una medición sin artefacto cuenta como verificada si sus cifras se pueden cotejar contra corridas que sí existen, como pasa en el caso excelente.

**Consecuencia si va A:** flojo baja a 21,25 y tramposo pierde 3,75 más.

> **RESUELTA EN LA RONDA 1 — va A.** Esta decisión se cruzó con la ronda de
> calibración a ciegas y quedó resuelta ahí, así que no hace falta que la
> vuelvas a tomar; queda acá por trazabilidad. La razón está escrita como
> decisión registrada en el changelog de `rubrica.md` v1.4: la corroboración que
> ofrece el caso excelente —la tabla de tokens ajusta linealmente contra las
> cantidades de registros de tres corridas que sí existen— es exactamente la que
> la opción B pedía admitir, y aun así se sostuvo el parcial. La diferencia con
> R6-bis, que sí admite corroboración indirecta, no es de calidad sino de cuántas
> fuentes independientes intervienen: en R6-bis el artefacto derivado corrobora un
> tercer objeto, y acá la tabla corrobora contra sí misma. La opción B, además,
> habría movido la nota de `casos/tramposo`, que el ejemplo 4.4 fija por escrito.
> Si estás en desacuerdo, decilo y se reabre: es una decisión, no un hecho.

---

## Y una cuarta que apareció después

**R6-bis · insumo externo frente a artefacto del entregable.** Está en curso: surgió
durante la ronda a ciegas de Gonzalo, cuando se encontró que
`casos/excelente/datos/bd_cursos.xlsx` se cita doce veces en ocho archivos y no
existe.

Cuando el texto esté redactado va a entrar en `rubrica.md` con línea de changelog
fechada, porque es un cambio de rúbrica en medio de una calibración.

---

## Después de decidir

1. Marcá las casillas y commiteá este archivo.
2. Actualizá `casos/NOTAS_DE_DISENO.md` con las notas objetivo que cambien.
3. Si alguna decisión cambia el texto de una condición, editá `rubrica.md` y agregá la línea de changelog.
4. Avisá al grupo: las notas objetivo son la referencia de la corrección a ciegas.
