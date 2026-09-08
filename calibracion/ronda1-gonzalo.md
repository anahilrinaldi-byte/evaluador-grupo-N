# Ronda 1 · corrección a ciegas — gonzalo

**No mires las notas de los otros hasta que los cuatro archivos estén subidos.**
El desacuerdo es el entregable: si mirás antes, se pierde.

Puntuá los **tres casos** en las **cinco dimensiones**, con `rubrica.md` en la mano.

---

## Valores permitidos

V2 usa anclas discretas: **no existen valores intermedios.** Solo podés poner uno
de estos cinco por dimensión.

| Nivel | D1 (30) | D2 (25) | D3 (15) | D4 (15) | D5 (15) |
|---|---|---|---|---|---|
| N4 | 30 | 25 | 15 | 15 | 15 |
| N3 | 22,5 | 18,75 | 11,25 | 11,25 | 11,25 |
| N2 | 15 | 12,5 | 7,5 | 7,5 | 7,5 |
| N1 | 7,5 | 6,25 | 3,75 | 3,75 | 3,75 |
| N0 | 0 | 0 | 0 | 0 | 0 |

**Cómo se llega al nivel:** contá los cuatro componentes de la dimensión
—verificado 1, parcial 0,5, no verificado 0—, sumá y truncá hacia abajo.
3,5 trunca a 3, o sea N3.

---

---

## Cómo se produjo esta ronda

La corrección se hizo en diálogo con un agente de IA distinto del que venía
construyendo la rúbrica, y a propósito: el que la escribió ya había puntuado los
tres casos y no podía volver a mirarlos sin contaminar el resultado. El
procedimiento fue componente por componente, exigiendo la condición textual antes
que la impresión general, y declarando cada regla de corte aunque no se activara.

Las decisiones discutibles las tomé yo y están firmadas abajo, en «Dónde dudé».
Cuatro de ellas cambiaron la rúbrica: la ronda produjo las versiones v1.2, v1.3 y
v1.4, todas con su changelog.

**Versión de la rúbrica contra la que se tomó cada dimensión.** La vara cambió
durante la corrección y eso hay que declararlo, porque una calibración donde la
vara se mueve y no se dice no vale nada.

| Dimensión | Versión | ¿La versión la afecta? |
|---|---|---|
| D1 | v1.1, sostenida por R6-bis en v1.2 | Sí. Bajo v1.1 estricta, D1 sería 22,5 |
| D2 | v1.1 | No |
| D3 | v1.2 | No — no hay rutas de insumo en juego |
| D4 | v1.2 | No |
| D5 | v1.3 | No |

## Caso EXCELENTE — `casos/excelente`

| Dim | Componentes (V / P / NV) | Conteo | Nivel | Puntaje | Por qué |
|---|---|---|---|---|---|
| D1 Sistema | Contrato V · Herramienta V · Output V · Gancho V | 4,0 | N4 | 30 | Los cuatro abribles. Falta `datos/bd_cursos.xlsx`, citado doce veces, y el trabajo no declara la ausencia: bajo R6-bis es insumo externo no versionado, se registra y no baja el nivel, con la excepción agotada |
| D2 Proceso | Iteraciones V · Fallas V · Decisiones V · Alcance V | 4,0 | N4 | 25 | Dos versiones anteriores del contrato, el error del 22/08 con log, decisiones con alternativa descartada, y dos recortes de alcance declarados. R11 verificada y no aplica: hubo discrepancia en D1 pero la documentación no narra la construcción del artefacto inexistente |
| D3 Formato | Estructura V · Corridas V · Reconstruibilidad V · Instrucciones V | 4,0 | N4 | 15 | Las cinco rutas de 3.1 presentes; tres corridas con filtros distintos que anidan bien (149 España ⊂ 362); quince campos idénticos en las tres salidas, incluido `"error": null`; seis pasos de ejecución con modelo y temperatura |
| D4 Económico | Consumo **P** · Costo V · Proyección **P** · Elección V | 3,0 | N3 | 11,25 | Costo recalculable al sexto decimal y elección con criterio y alternativa. Consumo parcial: el origen está declarado pero la consola de la API no está en ningún archivo, y 4.4 resuelve esa misma situación como parcial en el tramposo. Proyección parcial: falta el horizonte semanal, declarado en tres archivos |
| D5 Gobierno | Perímetro V · Riesgos V · Autonomía V · Responsabilidad **P** | 3,5 | N3 | 11,25 | Perímetro con cinco acciones prohibidas; cinco riesgos anclados a acción y consecuencia; L2 coherente con el flujo y R24 verificada en sus dos sentidos. Responsabilidad parcial: define quién firma y qué veta, y no dice qué pasa si esa persona no está disponible |
| **Total** | | | | **92,50** | |

## Caso FLOJO — `casos/flojo`

> **Pendiente.** El excelente consumió la ronda entera porque cada dimensión
> destapó un problema de la rúbrica. Acá está D2, que es una de las cinco celdas
> genuinamente ciegas.


| Dim | Componentes (V / P / NV) | Conteo | Nivel | Puntaje | Por qué |
|---|---|---|---|---|---|
| D1 Sistema | Contrato **P** · Herramienta **NV** · Output **P** · Gancho **P** | 1,5 | N1 | 7,5 | Contrato con cinco de seis funciones: falta el comportamiento ante fallo. Sin herramienta, y declarado en tres lugares —los datos se pegan en el prompt—. Una sola corrida, y el formato no se puede validar contra sí mismo. El gancho dice que revisa antes de usarlo, y no dice qué puede vetar |
| D2 Proceso | Iteraciones **NV** · Fallas **NV** · Decisiones V · Alcance V | 2,0 → **R10** | **N1** | 6,25 | Dos decisiones con alternativa nombrada y motivo, y el lector de Excel resignado por falta de tiempo, que es recorte deliberado y no falla. Pero cero estado anterior de ningún artefacto y ninguna falla con artefacto: la falla del margen se narra y no se transcribe. R10 se activa y topea en N1 |
| D3 Formato | | | | | |
| D4 Económico | | | | | |
| D5 Gobierno | | | | | |
| **Total** | | | | | |

## Caso TRAMPOSO — `casos/tramposo`

> **Pendiente.** Acá están tres de las cinco celdas ciegas: D1, D3 y D5.


| Dim | Componentes (V / P / NV) | Conteo | Nivel | Puntaje | Por qué |
|---|---|---|---|---|---|
| D1 Sistema | | | | | |
| D2 Proceso | | | | | |
| D3 Formato | | | | | |
| D4 Económico | | | | | |
| D5 Gobierno | | | | | |
| **Total** | | | | | |

---

## Dónde dudé

Anotá acá las dimensiones donde no estabas seguro, aunque hayas puesto un número.
**Esto vale tanto como las notas:** una duda compartida por dos personas es un
descriptor mal escrito, y eso es lo que la ronda tiene que encontrar.

- **R17 caza edición cosmética, no fabricación.** Detecta que una salida fue
  retocada; no detecta que nunca fue producida por un modelo. Un JSON escrito a
  mano con aritmética perfecta la pasa sin despeinarse. Las tres salidas del
  excelente cierran al céntimo en todas las agregaciones, que es lo que uno
  esperaría de una planilla más que de tres corridas de un LLM. **No lo usé para
  bajar el nivel**, y quiero ser explícito sobre por qué: la rúbrica no me da
  ninguna palanca para hacerlo y 4.4 advierte contra puntuar por impresión
  general. Bajarlo sería exactamente lo que la rúbrica prohíbe. Queda como límite
  conocido de R17.
- **Apliqué P9 con dos varas en la misma corrección.** En D1 di la Herramienta por
  verificada apoyándome en un log que también podría haberse tipeado a mano, y en
  D4 bajé el Consumo a parcial porque la consola de la API no está. La diferencia
  que sostengo: en D1 el artefacto existe y sus timestamps corroboran contra
  `entrada.md`, y en D4 no existe ninguno y la tabla es el único soporte de sí
  misma. Pero alguien puede señalar con razón que es la misma regla con dos
  exigencias.
- **USD 0,0562 no es reconstruible.** El documento da la tarifa de entrada de
  GPT-4o (2,50/M) y nunca la de salida. El resto implícito son 0,0025, o sea una
  tarifa de 1,799/M que no está escrita en ningún lado. **Dudé si activaba R20**
  —«los números no cierran entre sí»— y resolví que no: el número no contradice a
  ningún otro, es indeterminado y no inconsistente. Es un defecto de base de
  cálculo (R19) sobre una cifra que no es la viga del componente. Si se activara,
  D4 caería a 7,50.
- **El N1 del flojo se apoya en dos lecturas benignas que la rúbrica no explicitaba.**
  Contrato parcial exige contar funciones y no archivos —no hay `user_prompt.md`—, y
  Gancho parcial exige aceptar «antes de usarlo» como criterio de activación
  incondicional. Si cualquiera de las dos se resuelve en estricto, la dimensión cae
  a N0 = 0. Es lo contrario del D4 del excelente, que daba 11,25 en tres de cuatro
  escenarios. Originó v1.5.
- **Casi le exijo más al flojo que al excelente.** Mi primera lectura del Gancho fue
  0, contando el criterio de activación como ausente — cuando en el excelente había
  aceptado la lectura incondicional de «se detiene después de producir el JSON». El
  flojo dice lo mismo con menos palabras. Lo anoto porque es el mecanismo exacto por
  el que un corrector le exige más al trabajo que ya le pareció flojo, y valía un
  punto entero de conteo.
- **En D2 del flojo, el cambio de alcance se apoya en la misma frase que la
  decisión.** El párrafo del lector de Excel verifica los dos componentes. No lo
  bajé: 2.1 no lo prohíbe y el excelente hace lo mismo con Google Sheets —aparece
  como decisión D4 y como fila de la tabla de recortes—, así que rechazarlo acá
  rompe P4. Es la verificación más delgada del caso. Si cayera a parcial, el
  conteo baja a 1,5 y el puntaje no se mueve, porque R10 ya topea en N1.
- **R10 tenía un disparador que nadie puede verificar.** Pedía documentación
  «escrita íntegramente al final», y no hay forma de comprobar cuándo se escribió:
  el único indicio sería el historial de commits, que N3 prohíbe usar y hasta
  mencionar. Un corrector que se tomara esa condición en serio concluiría que R10
  nunca aplica y D2 del flojo pasaría de 6,25 a 12,5. Son 6,25 puntos decididos por
  una condición imposible. Originó v1.7.
- **Output estructurado del flojo: lo tomé en no verificado y quedó en parcial.**
  Mi razón para el cero era que una sola corrida no se puede validar contra sí
  misma. Lo resolvió la decisión de Anahí sobre D1 del tramposo, cruzada con P4: si
  un formato declarado que **ninguna** corrida satisface vale parcial —que es lo
  que ella decidió—, un formato declarado que **una** corrida satisface no puede
  valer menos. El flojo está estrictamente mejor que el tramposo en este
  componente. El puntaje no se mueve, 1,0 y 1,5 truncan los dos a N1.
- **Dos de las cinco celdas ciegas quedaron sin corregir** (D2-flojo, y D1, D3 y
  D5 del tramposo). Lo anoto como límite de esta ronda, no como resultado.

## Contradicciones y alertas que encontré

Solo para el tramposo, en principio. Si le encontrás contradicciones al flojo,
anotalo igual: sería una señal de que la rúbrica induce falsos positivos.

### En el caso excelente

- **`datos/bd_cursos.xlsx` se cita doce veces en ocho archivos y no está en el
  árbol.** Es la única ruta afirmada e inexistente del entregable, y el trabajo
  nunca declara la ausencia. Originó R6-bis.
- **`herramienta/config_lector.md:26` dice «Cada corrida deja un log en `logs/`»** y
  hay log para una de las tres corridas. Originó R6-ter.
- **El pie de `prompts/versiones/system_prompt_v1.md` cita D1 y D2** cuando esas
  dos decisiones son recortes de alcance.
- **De las tres fallas narradas, solo la del margen tiene artefacto.**

### En el caso flojo

- **El ejemplo 1.5 afirmaba algo falso sobre el caso que cita.** Decía que al flojo
  le falta «alcance explícito», y `prompts/system_prompt.md:15` dice «No hagas
  proyecciones ni recomendaciones, solo el análisis de lo que pasó», que es fuera de
  alcance en negativo. Cubre cinco funciones y no cuatro. El nivel no cambia porque
  1.2 pone cuatro y cinco en la misma casilla, pero un ejemplo que describe mal su
  propio caso enseña a leer mal. Corregido en v1.5.
- **A favor del flojo, y vale registrarlo:** la fórmula del margen está bien desde el
  principio, con la comisión del comercial incluida — que es justo el error que al
  excelente le costó una iteración entera.

### En la rúbrica misma

- **`rubrica.md` citaba mal su propia regla en tres lugares.** La condición del
  componente Perímetro remitía a R25 (Responsable nominal) para las acciones
  prohibidas, que son R26, y dos líneas del changelog de v1.0 hacen lo mismo. Un
  corrector que siguiera el puntero aterrizaba en la regla de otro componente y
  podía dejar Perímetro verificado de más. Corregido el puntero vivo; las dos del
  changelog quedan con nota de numeración, porque es registro histórico.
- **Mi propia corrida de calibración del excelente tenía el inventario de 6.3
  vacío.** No registraba el `.xlsx`. Bajo v1.1 el total real era 85 y no 92,5.
  Re-emitida dos veces, como corridas 1-bis y 1-ter.
- **`app.py` no compilaba.** Un `SyntaxError` introducido esa misma noche dejó la
  app sin levantar. Lo anoto acá porque es el hallazgo de mayor consecuencia de
  toda la ronda: la rúbrica estaba en su mejor momento y el sistema que la
  ejecuta estaba caído.
