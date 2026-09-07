# Desacuerdo · D3 del caso tramposo

**3,75 puntos**, y salen de un componente que las notas de diseño dan por
cumplido y no existe en ningún archivo.

| | D3 · Formato y reproducibilidad |
|---|---|
| Nota objetivo en `casos/NOTAS_DE_DISENO.md` | **N2 · 7,50** |
| Corrida del corrector | **N1 · 3,75** |
| Diferencia | **3,75** |

## De dónde sale

Las notas justifican el N2 así: *«Estructura e instrucciones OK; solo 2 corridas
(R14) y salidas editadas (R17)»*. Cuenta dos componentes verificados —estructura
e instrucciones—, lo que da conteo 2 y por lo tanto N2.

**Pero el tramposo no tiene instrucciones de ejecución en ningún archivo.**

Sus nueve secciones son: nota personal, el problema, qué hace, arquitectura,
evidencia de funcionamiento, supervisión, análisis económico, gobierno, proceso y
estructura del repositorio. Ninguna dice cómo correrlo: ni un comando, ni una
dependencia, ni qué modelo usar.

La sección 9 se llama **«Estructura del repositorio»** y es lo más cerca que hay.
Pero es un árbol de archivos, no una guía de ejecución — y encima ese árbol es
parte de la trampa T1, porque lista cuatro rutas que no existen.

Con `instrucciones_ejecucion` en no verificado, el conteo queda en 1 y el nivel
en N1.

## Y hay un error de aritmética, que era mío

La primera versión de `corrida-tramposo.json` tenía los componentes sumando 1,5
—o sea N1— y declaraba `nivel_final: N2`, citando R14 y R17.

**Ninguna regla de corte sube de nivel.** R14 es un techo y R17 degrada un
componente: las dos bajan o topean. Un conteo de 1,5 no puede terminar en N2 por
aplicar reglas que solo restan.

Estaba mal y el validador no lo detectaba, porque solo controlaba que los puntajes
fueran valores de ancla y que el total cerrara — y 7,50 es un valor de ancla
válido y la suma cerraba con él.

**Se agregó el chequeo a `validar_corrida`:** si `nivel_final` es mayor que
`nivel_por_conteo`, la corrida no es válida. No estaba en la especificación
original de `agente/configuracion` §5; lo agregamos porque una de nuestras propias
corridas lo violaba sin que nada lo notara.

## Por qué esto importa más allá del caso

Es la misma familia que el desacuerdo de D1: **algo que se lee como cumplido y no
lo está.** El tramposo tiene una sección que *parece* documentación de uso, y
alguien leyendo rápido —incluida quien diseñó el caso— la cuenta como tal.

Un trabajo final que trae un árbol de archivos y ninguna instrucción de ejecución
va a caer exactamente ahí el domingo 13.

## Qué hay que decidir

**Adoptar N1 y corregir la nota objetivo del tramposo en
`casos/NOTAS_DE_DISENO.md`**, de 7,50 a 3,75 en D3.

Es el caso más claro de los dos desacuerdos: no es una diferencia de lectura de
una condición ambigua, es un componente que se acreditó sin evidencia. La regla
R1 de la propia rúbrica dice que solo cuenta lo abrible.

## Estado de la nota del tramposo

| | Total |
|---|---|
| Nota objetivo original | 33,75 |
| Con el desacuerdo de D3 resuelto (esta corrida) | **22,50** |
| Si además se adopta P9 en D4 | 18,75 |

**Y el efecto que hay que mirar:** con 22,50 el tramposo queda **por debajo** del
flojo, que saca 25,00. El resultado contraintuitivo que las notas de diseño
identificaron —«el tramposo saca más que el flojo»— se da vuelta solo, sin
necesidad de una regla de integridad agregada.

Eso cambia la decisión que estaba abierta: quizá no haga falta escribir esa regla,
porque el problema era una nota mal calculada y no la rúbrica.
