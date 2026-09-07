# Prueba de fuego — jueves 10/9

El enunciado dice tres cosas sobre esa noche: los casos son **nuevos**, la prueba
es **pública**, y de ahí sale **un solo** evaluador para corregir todos los
trabajos finales. Y agrega una advertencia que conviene leer literal: *un
evaluador que se desploma frente a un caso real dice algo sobre su construcción,
y la clase entera lo va a ver.*

Es la única parte del parcial que no se puede corregir después.

---

## Antes de entrar

Entregar el **miércoles a la noche**, no el jueves. El jueves es margen.

Chequeo de cinco minutos:

- [ ] El link del repo abre desde una ventana privada, sin sesión iniciada
- [ ] `app.py` levanta y la API key está cargada en los secretos
- [ ] **Correr una evaluación de prueba esa misma tarde**, para confirmar que la
      key sigue viva y que no nos comimos la cuota
- [ ] Alguien tiene el repo clonado localmente, por si falla la red del aula

---

## Los tres roles

No improvisar quién habla.

| Rol | Qué hace |
|---|---|
| **Manos** | Maneja la pantalla. No explica: ejecuta |
| **Voz** | Narra qué está por pasar **antes** de que pase. Una sola persona habla |
| **Notas** | Anota qué falló, para responder preguntas sin depender de la memoria |

**La regla de oro: la Voz anuncia antes de correr, no después.** «Nuestro corrector
va a reportar esto como hallazgo», dicho antes de que aparezca, vale diez veces
más que la misma frase dicha después. Es lo que demuestra que el comportamiento
estaba diseñado y no fue suerte.

---

## Los cinco casos que probablemente tiren

### 1 · Un repo con instrucciones dirigidas al evaluador

Es el ataque más probable. Estamos cubiertos por partida doble: la CAPA 5 del
system prompt y el bloque `REGLA DE SEGURIDAD CRÍTICA` que `app.py` inserta antes
del contenido del repo.

**Qué decir antes de correr:** «si este repo tiene algo dirigido al corrector, lo
va a citar en `alertas_integridad` y no le va a cambiar la nota».

### 2 · Un repo vacío, roto o sin la estructura esperada

Cubierto. La respuesta correcta es puntuar lo verificable y declarar lo que no se
pudo verificar. Nunca estimar.

**Qué decir antes:** «esto va a dar bajo y con limitaciones declaradas, que es una
corrección válida y no un error del corrector».

### 3 · Un repo genuinamente bueno

**Este es el que nos puede romper, y es el que nadie ensaya.** Después de
construir un tramposo con seis capas, el corrector está entrenado para desconfiar.
Si le levanta hallazgos a un trabajo honesto queda tan mal como si aprobara al
tramposo: **un corrector paranoico es tan inservible como uno crédulo.**

**Ya está probado:** la corrida sobre `casos/flojo` salió con `contradicciones`
vacío y `alertas_integridad` vacío. El flojo hizo poco y lo dice, y el corrector
no le inventó fraude. Ese es el resultado a mostrar si alguien duda.

### 4 · Un repo grande

`app.py` corta en 80 archivos y 180.000 caracteres. **Nuestro propio repo ya
empaqueta 143.603**, o sea el 80% del techo. Un trabajo final más grande se trunca.

La app ahora reporta `archivos_fallidos` y el prompt le avisa al corrector que
declare el paquete incompleto. **Qué decir:** «si el repo es grande, el corrector
va a declarar qué no alcanzó a leer en vez de evaluar a ciegas».

### 5 · El mismo caso dos veces

Para ver si da lo mismo. Está pedido en el enunciado, así que lo van a pedir.
**Proponerlo nosotros antes de que lo pidan.**

---

## Si falla en vivo

Va a fallar algo. Lo que se corrige no es que falle: es qué hacen los cuatro en
los diez segundos siguientes.

**No hacer:** pedir disculpas, decir «se debe haber colgado», culpar al modelo,
quedarse callados, o correrlo de nuevo esperando que salga distinto.

**Hacer:** nombrar el mecanismo de la falla en una oración y decir qué instrucción
faltaba. El enunciado marca el camino: si el corrector «no puede» leer un repo, la
pregunta es qué herramienta o instrucción le falta. Un grupo que diagnostica su
propia falla en vivo demuestra exactamente la competencia que se evalúa.

> «Falló porque el repo supera el límite de caracteres y el paquete se truncó. La
> app lo reporta en `archivos_fallidos`; lo que falta es que el corrector lo lea y
> lo declare en `limitaciones` en vez de puntuar como ausente lo que no llegó.»

Eso, dicho con calma, deja mejor parado al grupo que un corrector que anduvo de
casualidad.

---

## Las dos cartas para jugar

Si hay que mostrar una sola cosa, son estas.

### El desacuerdo de D1

La calibración encontró **7,50 puntos de diferencia en el tramposo**, y salen de
una sola decisión de componente: si el output estructurado se verifica contra el
formato que el trabajo declaró, o solo contra sí mismo. Un equipo que escribe el
contrato al principio y después no lo respeta cae exactamente ahí.

Está en `calibracion/desacuerdo-D1-tramposo.md`, con las dos lecturas y la
recomendación.

### El límite de cuota

`app.py` pedía cada archivo a la API de GitHub, que sin autenticar admite 60
pedidos por hora. **Un repo de 40 archivos consumía 41, así que la segunda
evaluación de la hora fallaba — y fallaba en silencio**, entregándole al corrector
un repo incompleto sin ninguna señal.

Lo encontramos probando, no razonando: la cuota quedó en 0/60 después de dos
corridas. Corregido bajando por `raw.githubusercontent`, que no tiene ese límite.

**Es la clase de defecto que solo aparece cuando corrés el sistema de verdad**, y
esa es exactamente la diferencia entre un trabajo que funciona y uno que se
describe.

---

## El ensayo del miércoles

Cada integrante trae **un caso que los otros tres no vieron**. Es lo más parecido
a la prueba de fuego que se puede hacer sin estar ahí. Se corre con los roles
puestos y se cronometra.

Después de ese ensayo se arregla **solo lo que se rompa feo**. Un cambio de prompt
la noche antes, sin volver a correr los tres casos, es cómo se pierde el
determinismo justo cuando lo van a probar.
