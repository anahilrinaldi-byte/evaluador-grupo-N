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
| 3 | 2026-09-06 | aplicación manual de la rúbrica | — | v2.1 | `casos/tramposo` | `corrida-tramposo.json` | sí |
| 1-bis | 2026-09-07 | aplicación manual de la rúbrica | — | `agente/system_prompt.md` | `casos/excelente` | `corrida-excelente.json` | sí, re-emitida |

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

Las corridas a mano dan **92,50 · 25,00 · 22,50**.

**Si Gemini coincide**, es determinismo entre dos correctores distintos aplicando la
misma rúbrica. Es el resultado más fuerte que puede dar esta pieza.

**Si difiere**, hay que documentar dónde y por qué, y decidir si el problema es de
la rúbrica —un descriptor que admite dos lecturas— o del prompt —el corrector no
aplica lo que la rúbrica dice. Confundir esas dos causas es el error clásico.

**Y hay que correr el mismo caso tres veces** para la prueba de determinismo, con el
diff de las tres salidas. Está pedido dos veces en el enunciado del parcial.
