# El injerto de P9 cambió dos notas

Hallazgo de la consolidación del 7/9. **Lo introduje yo y lo verifiqué después**,
que es el orden equivocado: conviene dejarlo escrito así.

## Qué pasó

Las tres corridas de `calibracion/corrida-*.json` se hicieron contra la V2 **antes**
de injertarle los principios P7, P8 y P9 de la v3 de Migue. Uno de esos principios
cambia puntajes.

**P9 · Evidencia original.** «Una tabla de resultados es evidencia del resultado
solo si existe el artefacto que la produjo; si no, es una afirmación del autor y
le aplica P1.»

Eso pega de lleno en el componente `consumo_medido` de D4, porque **ningún caso
adjunta el contador de tokens**. Los tres declaran una medición y ninguno tiene el
artefacto que la respalda.

## La distinción que decide

Los tres declaran «medición real». La diferencia está en **si los números se pueden
cotejar contra algo verificable**:

| Caso | Qué dice | Con qué se coteja | Componente |
|---|---|---|---|
| **Excelente** | Tabla por corrida: 362, 149 y 120 registros | Los tres `salida.json` dicen `cursos_analizados` 362, 149 y 120. **Coinciden** | **parcial** |
| **Tramposo** | Dos corridas, entrada estable en 18.420 | Existen dos corridas y la estabilidad está explicada. Coherente consigo mismo | **parcial** |
| **Flojo** | «aproximadamente 15.000 tokens» | Nada. Sin desglose, sin origen, sin corroboración | **no verificado** |

Ninguno llega a verificado, porque a ninguno se le puede abrir el contador.

## El recálculo

| Caso | D4 antes | D4 después | Total antes | Total después |
|---|---|---|---|---|
| Excelente | N3 · 11,25 | N3 · 11,25 | 92,50 | **92,50** |
| Flojo | N1 · 3,75 | N0 · 0,00 | 25,00 | **21,25** |
| Tramposo | N4 · 15,00 | N3 · 11,25 | 22,50 | **18,75** |

**El excelente no se mueve.** Ya estaba en N3 porque su proyección cubre solo el
horizonte anual: el conteo bajó de 3,5 a 3 y las dos cifras truncan al mismo nivel.
Es una buena señal de que las anclas discretas absorben cambios chicos sin mover la
nota.

## Dos consecuencias que hay que mirar

### 1 · Ningún caso llega a N4 en D4

Con P9 aplicado, la cima de esa escala **no está probada por ningún caso**. Es el
problema de «el medio y el techo de la escala sin probar», ahora en el techo y
concreto.

Corregido en `rubrica.md`: el ejemplo de nivel alto de D4 ya no señala al tramposo
—que sería falso— sino que describe qué haría falta para llegar a N4 y usa al
tramposo como el mejor N3.

**Yo mismo había puesto ese ejemplo mal**, afirmando que el tramposo era N4 en una
rúbrica que, con el principio injertado tres párrafos más arriba, lo deja en N3.

### 2 · ¿P9 es demasiado fuerte para D4?

Es la pregunta abierta y la tiene que decidir el grupo.

Aplicado literal, **casi ningún trabajo final va a adjuntar un volcado del contador
de tokens**, así que `consumo_medido` sería parcial o peor casi siempre y D4
quedaría con techo práctico en N3. Puede ser lo correcto —medir sin guardar la
medición es exactamente lo que P9 castiga— o puede ser una vara imposible.

Dos salidas, las dos defendibles:

- **Sostener P9 como está.** Quien mide de verdad puede guardar una captura. Es
  barato y es la diferencia entre medir y decir que se midió.
- **Escribir una excepción en la condición de `consumo_medido`.** Por ejemplo: una
  medición sin artefacto cuenta como verificada si sus cifras se pueden cotejar
  contra corridas que sí existen, como pasa en el caso excelente.

## Pendiente

Las tres corridas de `calibracion/corrida-*.json` **quedaron desactualizadas** y no
las reescribí, porque el recálculo depende de la decisión de arriba. Una vez
decidido, se rehacen las de flojo y tramposo. La del excelente no cambia.

Y hay que actualizar las notas objetivo de `casos/NOTAS_DE_DISENO.md`, que son
anteriores a la consolidación.
