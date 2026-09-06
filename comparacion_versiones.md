# Comparación de las tres versiones

Documento de decisión. Hay tres rúbricas y tres system prompts conviviendo en el
repositorio, y el enunciado pide `rubrica.md` y `agente/` — en singular. Hay que
elegir una base antes de seguir, y esta es la evidencia para elegirla.

**No se trata de descartar dos.** La propuesta es una base más injertos, para que
no se pierda el trabajo de nadie.

---

## Qué aporta cada versión

### v1 — `rubrica.md` + `agente/system_prompt.md` (Tati)

Es la fundación. Las cinco dimensiones oficiales del Trabajo Final con sus pesos
—30 / 25 / 15 / 15 / 15—, los seis principios de evaluación, y el system prompt con
las ocho fases del procedimiento.

**Las otras dos versiones no se escribieron desde cero: partieron de estos archivos.**
Todo lo que sigue está construido arriba de esta base.

**Su límite:** las bandas son anchas. «27–30 puntos — Excelente» son cuatro puntajes
posibles y nada dice cuál elegir. El enunciado del parcial pide una rúbrica «tan
precisa que un agente la aplica igual dos veces», y una banda de cuatro valores es
exactamente lo contrario.

### V2 — `rubrica_V2.md` + `agente/system_prompt_v2.md` (Anahí)

Es la más ejecutable de las tres, por dos mecanismos que no están en las otras.

**Anclas de puntaje.** «El corrector asigna el valor exacto del nivel. No existen
rangos ni valores intermedios.» D1 solo puede valer 30 · 22,5 · 15 · 7,5 · 0. No hay
nada que elegir dentro de una banda, así que la varianza en el paso de puntuación es
cero por construcción.

**Aritmética de conteo.** Cada dimensión tiene cuatro componentes; verificado vale 1,
parcial 0,5, no verificado 0; se suman y se trunca hacia abajo. Convierte un juicio
en una cuenta.

Además trae el sistema de reglas numeradas R1 a R26, el mapa de demarcación entre
dimensiones que evita el doble conteo, y la definición de «función cubierta»: un
requisito se verifica por lo que resuelve, no por la etiqueta que usa.

### v3 — `rubrica-v3.md` + `agente/system_prompt_v3.md` (Migue)

Es la que tiene la epistemología más afilada. Tres distinciones que **no están en
V2** y que son defendibles frente a un reclamo:

**§3.3 — «no demostrado» no es lo mismo que «incumplido».** La ausencia de evidencia
no permite afirmar que algo no exista fuera del repositorio; pero un requisito no
demostrado tampoco puede recibir los puntos que dependen de su demostración. Es la
formulación correcta y evita que el corrector afirme de más.

**§3.5 — evidencia de existencia vs. evidencia de funcionamiento.** Que el archivo
esté no prueba que ande.

**§3.6 — evidencia original.**

También rompe las bandas en bloques por puntaje —«30 puntos», «29 puntos», «27–28
puntos»—, que es mejor que v1, pero todavía deja rangos.

---

## El dato que decide

**Las notas objetivo de los tres casos están calculadas contra V2, y solo contra V2.**

`casos/NOTAS_DE_DISENO.md` declara, antes de correr nada: excelente 92,50 · flojo
25,00 · tramposo 33,75. Esas cifras no son aproximaciones: son sumas exactas de la
tabla de anclas de V2.

| Caso | Descomposición | Total |
|---|---|---|
| Excelente | 30 + 25 + 15 + 11,25 + 11,25 | 92,50 |
| Flojo | 7,5 + 6,25 + 3,75 + 3,75 + 3,75 | 25,00 |
| Tramposo | 7,5 + 0 + 7,5 + 15 + 3,75 | 33,75 |

Y la derivación de cada nota cita reglas por número —R2, R6, R10, R11, R14, R17,
R23, R25, R26—. Esas reglas aparecen **51 veces en V2 y cero veces en v1 y en v3**.

**Consecuencia:** si la base es v1 o v3, las notas objetivo de los tres casos dejan
de tener sustento y hay que recalcular la calibración entera. Con la entrega el
miércoles y esa pieza valiendo 15 puntos, no es una opción realista.

V2 no es una preferencia de estilo: es carga estructural.

---

## Propuesta

**Base: V2.** Se renombra `rubrica_V2.md` a `rubrica.md` y
`agente/system_prompt_v2.md` a `agente/system_prompt.md`.

**Injertos de v3**, como principios generales antes de las dimensiones:

- §3.3 · no demostrado ≠ incumplido
- §3.5 · evidencia de existencia vs. de funcionamiento
- §3.6 · evidencia original

Ninguno choca con la aritmética de V2: son reglas de lectura de la evidencia, no de
puntuación. Se suman sin tocar las anclas ni el conteo de componentes.

**De v1 no se injerta nada** porque V2 ya la contiene: V2 partió de esos archivos.

**Qué pasa con los archivos descartados.** Se borran del repositorio. El enunciado
avisa que la corrección la asiste un agente y que el formato importa; tres rúbricas
en la raíz son tres candidatas a que el corrector lea la equivocada. **El trabajo no
se pierde: queda entero en la historia de commits**, que es donde el criterio de
proceso lo va a buscar.

---

## Lo que sigue faltando en las tres

Ninguna de las tres versiones tiene los **ejemplos de nivel alto y nivel bajo por
dimensión**. El enunciado del parcial los pide con esas palabras, dentro del criterio
de 25 puntos. V2 tiene una columna *Ejemplo* por nivel en cada escala, que es lo más
cerca que está ninguna — pero es un ejemplo por nivel, no el par alto/bajo explícito
que se pide.

Es lo primero que hay que escribir después de decidir la base.
