# Registro de corridas del corrector

`agente/configuracion` §6 exige que cada corrida se archive con **fecha, modelo y
temperatura usados, versión del system prompt, entregable evaluado, JSON completo
tal como salió, y si fue válida o fallida**.

**Las corridas fallidas no se borran.** Son evidencia del proceso y el insumo de la
calibración: una corrida que falló y se entiende por qué vale más que ninguna.

---

## Corridas registradas

| # | Fecha | Modelo | Temp. | System prompt | Entregable | JSON | Válida |
|---|---|---|---|---|---|---|---|
| 1 | 2026-09-06 | aplicación manual de la rúbrica | — | v2.1 | `casos/excelente` | `corrida-excelente.json` | sí |
| 2 | 2026-09-06 | aplicación manual de la rúbrica | — | v2.1 | `casos/flojo` | `corrida-flojo.json` | sí |
| 2-bis | 2026-09-08 | aplicación manual de la rúbrica | — | `agente/system_prompt.md` | `casos/flojo` | `corrida-flojo.json` | sí, re-emitida |
| 3 | 2026-09-06 | aplicación manual de la rúbrica | — | v2.1 | `casos/tramposo` | `corrida-tramposo.json` | sí |
| 3-bis | 2026-09-08 | aplicación manual de la rúbrica | — | `agente/system_prompt.md` | `casos/tramposo` | `corrida-tramposo.json` | sí, re-emitida |
| 1-bis | 2026-09-07 | aplicación manual de la rúbrica | — | `agente/system_prompt.md` | `casos/excelente` | `corrida-excelente.json` | sí, re-emitida |
| 1-ter | 2026-09-07 | aplicación manual de la rúbrica | — | `agente/system_prompt.md` | `casos/excelente` | `corrida-excelente.json` | sí, re-emitida |

**Corrida 3-bis.** La corrida del tramposo se re-emitió contra **v1.7** por el mismo
defecto que las dos del excelente: daba **Consumo medido** como verificado, en
contradicción directa con el ejemplo 4.4, que fija este caso en N3 y da la razón —la
tabla de tokens declara origen en el contador de la consola de la API y esa consola no
está en ningún archivo, así que por P9 es una afirmación del autor—. D4 pasa de 15 a
11,25 y el total de 22,50 a **18,75**. El tramposo sigue por debajo del flojo.

**Corrida 2-bis.** La corrida del flojo se re-emitió contra **v1.9** por dos errores
que se cancelaban entre sí. Daba D3 en N1 · 3,75 y D4 en N1 · 3,75; da ahora D3 en
N2 · 7,50 y D4 en N0 · 0. **El total no cambia: 25,00 en las dos.** D3 estaba tomada
contra la escala de Cantidad de corridas anterior a v1.9, que trataba la corrida
única como no verificada; D4 contradecía el ejemplo 4.4, que fija este caso en N0.
La corrida real de la app y la ronda a ciegas coincidieron entre sí en la
distribución correcta, y la corrección a mano era la única de las tres que discrepaba.

**Corrida 1-ter.** Segunda re-emisión, contra **v1.3**, por una discrepancia de la
ronda 1 en D4. La corrida daba **Consumo medido** como verificado; el ejemplo 4.4 resuelve
esa misma situación probatoria como **parcial** en `casos/tramposo` —la tabla de tokens
declara origen en la consola de la API y esa consola no está en ningún archivo, así que por
P9 el componente no se verifica—, y el excelente afirma lo mismo con casi las mismas
palabras. P4 obliga a puntuarlos igual. El componente pasa a parcial: el conteo cae de 3,5 a
3,0, ambos truncan a N3 y **el puntaje no cambia**, 11,25. Se corrige igual, porque una
inconsistencia entre dos casos del propio banco de calibración es exactamente lo que P4
prohíbe, y porque el conteo es lo que el jueves se va a poder auditar.

**Corrida 1-bis.** La corrida 1 se re-emitió el 7/9 contra la rúbrica **v1.2**. Su
inventario de consistencia (6.3) estaba incompleto: no registraba `datos/bd_cursos.xlsx`,
la fuente de datos que la documentación del caso cita doce veces y que no está en el árbol.
Bajo v1.1 esa omisión escondía una activación de R6 → R2 que habría dejado D1 en 22,5 y el
total en 85. Bajo v1.2 el hallazgo queda registrado y R6-bis lo cubre como insumo externo no
versionado con C5 incumplido: el total de 92,5 se sostiene por la excepción, no por omisión.
El archivo anterior no se conserva por separado — el historial de git tiene la versión previa.

**Las tres primeras no se ejecutaron con `app.py`.** Son la rúbrica aplicada
componente por componente, con el JSON del esquema completo. Sirven como referencia
contra la cual comparar, no como cumplimiento del criterio de «corre sobre un repo
real».

---

## Para completar cuando se corra la app

Copiá una fila por corrida. Las URLs de los tres casos:

```
https://github.com/anahilrinaldi-byte/evaluador-grupo-N/tree/main/casos/excelente
https://github.com/anahilrinaldi-byte/evaluador-grupo-N/tree/main/casos/flojo
https://github.com/anahilrinaldi-byte/evaluador-grupo-N/tree/main/casos/tramposo
```

| # | Fecha | Modelo | Temp. | System prompt | Entregable | JSON | Válida |
|---|---|---|---|---|---|---|---|
| 4 | | gemini-2.5-flash | 0 | | `casos/excelente` | `app-excelente.json` | |
| 5 | | gemini-2.5-flash | 0 | | `casos/flojo` | `app-flojo.json` | |
| 6 | | gemini-2.5-flash | 0 | | `casos/tramposo` | `app-tramposo.json` | |

**La columna «Válida» la contesta la propia app.** Muestra el veredicto de validez
antes de la nota: si dice que la corrida no es válida, se archiva igual como
fallida, se anota el motivo, y se corre de nuevo.

---

## Qué mirar al comparar

Las corridas a mano dan **92,50 · 25,00 · 18,75**.

**Si Gemini coincide**, es determinismo entre dos correctores distintos aplicando la
misma rúbrica. Es el resultado más fuerte que puede dar esta pieza.

**Si difiere**, hay que documentar dónde y por qué, y decidir si el problema es de
la rúbrica —un descriptor que admite dos lecturas— o del prompt —el corrector no
aplica lo que la rúbrica dice. Confundir esas dos causas es el error clásico.

**Y hay que correr el mismo caso tres veces** para la prueba de determinismo, con el
diff de las tres salidas. Está pedido dos veces en el enunciado del parcial.
