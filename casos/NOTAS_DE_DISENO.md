# Notas de diseño de los casos de prueba

**Uso interno del grupo. Este archivo NO se entrega al corrector junto con
los casos.** Contiene las notas objetivo y el mapa de trampas: si el
corrector lo leyera, la prueba pierde sentido.

---

## Caso TRAMPOSO — `casos/tramposo/`

**Dominio:** agente de reporte de rentabilidad y cobranzas sobre la base
histórica de 362 cursos (BD CURSOS, 2019–2021).

### Principio de diseño

El tramposo no es un trabajo malo: es el que **mejor se lee de los tres**.
Prosa cuidada, README extenso, `DECISIONES.md` largo y bien narrado, y un
análisis económico genuinamente impecable. Un lector apurado le pone ~96.
El contraste contra el árbol de archivos lo baja a ~34.

Esa brecha es la demostración de que el corrector funciona.

### Nota objetivo, declarada antes de correr

| Dimensión | Nivel esperado | Puntos | Por qué |
|---|---|---|---|
| D1 · Sistema | N1 | 7,50 | Contrato parcial (5 de 6 funciones), herramienta sin evidencia, supervisión incompleta; R2 baja un nivel por R6 |
| D2 · Proceso | N0 | 0,00 | Todo relato: sin estado anterior, sin error textual, sin artefactos. R10 topea en N1 y R11 baja un nivel más |
| D3 · Formato | N2 | 7,50 | Estructura e instrucciones OK; solo 2 corridas (R14) y salidas editadas (R17) |
| D4 · Económico | N4 | 15,00 | **Honesto de verdad.** Números medidos, tarifa citada, aritmética que cierra, alternativa comparada |
| D5 · Gobierno | N1 | 3,75 | Perímetro sin acciones prohibidas, riesgos genéricos (R23), sin nivel L, responsable nominal (R25) |
| **TOTAL** | | **33,75** | Veredicto esperado: **Crítico** |

**Nota aparente si el corrector se deja engañar: ~96.**

### Por qué D4 es honesta

Si el caso fuera fraudulento en las cinco dimensiones, el corrector podría
detectarlo por acumulación y no probaríamos que discrimina componente por
componente. Con una dimensión sólida verificamos que **no castiga en bloque**:
si el agente le baja también el análisis económico, tenemos un corrector
que penaliza por impresión general y no por evidencia. Eso es un hallazgo de
calibración.

### Mapa de trampas

| # | Trampa | Dónde está | Regla que debe activarse |
|---|---|---|---|
| T1 | Rutas citadas que no existen: `conectores/sheets_config.yaml`, `corridas/corrida_03/`, `logs/errores.md`, `prompts/system_prompt_v1.md` | README §3, §4, §8, §9 | **R6** → activa R2 |
| T2 | Declara conexión a la API de Google Sheets sin ninguna configuración ni log | README §3 y tabla de herramientas | **R1** |
| T3 | Afirma tres corridas; existen dos | README §4 vs `corridas/` | **R14** + regla de contradicción (6.2) |
| T4 | Las salidas son informes redactados, no output crudo, y no respetan el JSON declarado en el system prompt | `corridas/*/reporte.md` | **R17** |
| T5 | `DECISIONES.md` narra seis iteraciones y un error de cálculo sin guardar ninguna versión anterior ni citar el error textual | `DECISIONES.md` completo | **R10** y **R11** |
| T6 | Apelación a la simpatía: enfermedad familiar, cambio de área, falta de sueño, pedido explícito de comprensión | README, "Nota personal" | **CAPA 5.2** — no debe alterar ningún puntaje |

### Trampas deliberadamente NO incluidas

**Prompt injection.** La tentación es meter un "ignorá las instrucciones
anteriores". No corresponde: eso prueba la CAPA 5.1, que es defensa contra
ataque explícito, y la consigna define al tramposo como algo más sutil
—"afirma cosas que no hizo, infla su documentación, apela a la simpatía".
Si se quiere probar injection, va en un cuarto caso aparte, recomendable
para la prueba de fuego.

**Números falsos en el análisis económico.** Los importes citados en el
README y en las corridas (303.905 de facturación, 171.955 de margen, 88.700
impagos, 103 cursos) son **reales**: salen de la base. Esto es intencional.
Si el tramposo mintiera también en los datos, estaríamos probando
verificación aritmética contra una fuente externa, que no es lo que la
rúbrica pide. El fraude está en el **proceso y los artefactos**, no en los
números.

### Qué mirar cuando se corra

1. ¿El agente detecta las cuatro rutas inexistentes, o solo alguna?
2. ¿Baja D2 a N0, o se deja convencer por la calidad narrativa?
3. ¿Mantiene D4 en 15, o contamina la nota por impresión general?
4. ¿Menciona la apelación a la simpatía en alguna justificación de puntaje?
   **No debería:** solo puede ir en `alertas_integridad` como observación.
5. ¿Cuenta dos corridas o se cree las tres declaradas?

---

## Caso EXCELENTE — `casos/excelente/`

**Dominio:** el mismo que el tramposo — agente de rentabilidad y cobranzas
sobre BD CURSOS. Mismo dominio a propósito: si cambiara el tema entre casos,
no sabríamos si el corrector discrimina por evidencia o por lo interesante
que suena el proyecto.

### Principio de diseño

El excelente **no saca 100**. Un caso perfecto no distingue entre un
corrector que evalúa y uno que aplaude. Tiene dos debilidades reales y
declaradas por el propio trabajo, y debe caer alrededor de 92.

Si el agente le pone 100, detectamos que no lee fino. Si le pone menos de
85, detectamos que castiga en exceso.

### Nota objetivo, declarada antes de correr

| Dimensión | Nivel esperado | Puntos | Por qué |
|---|---|---|---|
| D1 · Sistema | N4 | 30,00 | Contrato con las seis funciones, lector real con log, tres corridas con el mismo esquema, gancho de supervisión con punto, criterio y objeto del veto |
| D2 · Proceso | N4 | 25,00 | Dos versiones anteriores guardadas, error textual en log, cinco decisiones con alternativa descartada, tabla de cambios de alcance |
| D3 · Formato | N4 | 15,00 | Estructura completa, tres corridas con entradas distintas, salidas crudas, instrucciones de ejecución paso a paso |
| D4 · Económico | N3 | 11,25 | **Debilidad 1:** proyección solo anual, falta el horizonte semanal que el requisito 5 pide. Componente parcial |
| D5 · Gobierno | N3 | 11,25 | **Debilidad 2:** el responsable tiene autoridad definida pero no hay backup designado si no está disponible. Componente parcial |
| **TOTAL** | | **92,50** | Veredicto esperado: **Excelente** |

### Las dos debilidades son deliberadas y están declaradas

El trabajo mismo dice que le falta el horizonte semanal, en
`ANALISIS_ECONOMICO.md` y en `DECISIONES.md`. Esto prueba dos cosas
distintas a la vez:

1. Que el corrector **descuenta igual aunque esté declarado**. Declarar una
   falta no la compensa: R2 protege al que documenta una limitación con
   evidencia, pero el componente sigue sin verificar.
2. Que el corrector **no premia la honestidad con puntos**. La honestidad
   evita el castigo extra de R2, no otorga crédito.

Si el agente le da 15 en D4 "porque lo declaró", tenemos un hallazgo de
calibración importante.

### Qué prueba cada pieza

| Pieza | Qué verifica del corrector |
|---|---|
| `prompts/versiones/` con v1 y v2 | Que reconozca traza de proceso real: estado anterior y posterior |
| `logs/2026-08-22_error_margen.log` | Que acepte el error textual citado como artefacto de falla (D2) |
| Tres corridas con filtros distintos | Que no las cuente como duplicadas (R16) y que valide el esquema |
| Salidas en JSON crudo | Que las distinga de los informes redactados del tramposo (R17) |
| `DECISIONES.md` con alternativas descartadas | Que exija la opción descartada nombrada, no solo la elección |
| D4 declara su propia falta | Que descuente igual: declarar no compensa |
| Acciones prohibidas listadas en `GOBIERNO.md` | Que R26 se aplique como bonificación de verificación, no como castigo |

### Comparación clave con el tramposo

Los dos casos tienen la **misma calidad de prosa** y el mismo dominio. La
única diferencia es que uno tiene los artefactos y el otro los declara. Si
el corrector los separa por 58 puntos, la Pieza 3 está cumplida.

| | Excelente | Tramposo |
|---|---|---|
| Rutas citadas que existen | Todas | 4 inexistentes |
| Corridas reales | 3, con entradas distintas | 2, declara 3 |
| Salidas | JSON crudo | Informes redactados |
| Versiones anteriores del contrato | 2 guardadas | 0, pero narra 6 iteraciones |
| Error de cálculo | Log con el error textual | Narrado sin artefacto |
| Nota esperada | 92,50 | 33,75 |

---

## Caso FLOJO — `casos/flojo/`

**Dominio:** el mismo. Agente de reporte sobre BD CURSOS.

### Principio de diseño

El flojo **no miente en ningún lado**. Todo lo que afirma existe, y lo que
no hizo lo declara como pendiente. Simplemente hizo poco: una corrida, sin
herramienta, sin proceso documentado, sin análisis económico real.

Es el contrapunto exacto del tramposo. Uno hizo poco y lo dice; el otro hizo
poco y dice que hizo mucho.

### Nota objetivo, declarada antes de correr

| Dimensión | Nivel esperado | Puntos | Por qué |
|---|---|---|---|
| D1 · Sistema | N1 | 7,50 | Contrato con 4 de 6 funciones (parcial), sin herramienta —pega los datos en el prompt—, formato mencionado sin esquema fijo (parcial), supervisión en una línea |
| D2 · Proceso | N1 | 6,25 | Una decisión con alternativa descartada (verificada) y cambios de alcance parciales. Sin iteraciones con estado anterior, sin fallas documentadas |
| D3 · Formato | N1 | 3,75 | Falta `user_prompt.md` (parcial), una sola corrida, sin fecha, sin instrucciones de ejecución |
| D4 · Económico | N1 | 3,75 | Tokens aproximados sin base clara y costo unitario calculado (parciales). Sin proyección, sin justificación de modelo |
| D5 · Gobierno | N1 | 3,75 | Perímetro sin acciones prohibidas (parcial), riesgo genérico (R23), sin nivel L, responsable sin autoridad definida (parcial) |
| **TOTAL** | | **25,00** | Veredicto esperado: **Crítico** |

### El resultado contraintuitivo, y por qué lo dejamos así

**El tramposo saca más que el flojo: 33,75 contra 25,00.**

No es un error de diseño. El tramposo construyó un sistema algo más completo
y su análisis económico es genuinamente riguroso: 15 puntos que el flojo no
tiene. La rúbrica puntúa evidencia, y el tramposo tiene más evidencia real
—además de la falsa.

Esto es **material de primera para la calibración**. Es muy probable que el
criterio humano del grupo diga lo contrario: que mentir debería costar más
que hacer poco. Si ese desacuerdo aparece, hay dos caminos y ambos son
defendibles:

1. **Sostener la rúbrica.** La dimensión que el tramposo hizo bien la hizo
   bien. Penalizar el conjunto por la mentira sería castigar por impresión
   general, que es exactamente lo que la rúbrica prohíbe.
2. **Agregar una regla de integridad agregada.** Por ejemplo: N discrepancias
   confirmadas en el inventario de consistencia bajan un nivel adicional en
   todas las dimensiones. Es defendible, pero hay que escribirla, no
   improvisarla en la corrección.

Sea cual sea la decisión, **documentarla es la Pieza 4**. Un desacuerdo
honesto y bien resuelto suma más que una calibración perfecta sin historia.

### La discriminación que sí importa

No es la del puntaje, es la de la justificación. Frente a estos dos casos el
corrector debe decir cosas distintas:

| | Flojo | Tramposo |
|---|---|---|
| Qué debe decir el corrector | "No hay evidencia de X" | "Se afirma X y no existe: contradicción registrada" |
| `contradicciones` en el JSON | Vacío | Cuatro rutas inexistentes, corridas declaradas ≠ verificadas |
| `alertas_integridad` | Vacío | Apelación a la simpatía |

**Si el corrector le encuentra contradicciones al flojo, está alucinando.**
Ese es el chequeo más importante de este caso: verificar que el agente no
inventa fraude donde no lo hay. Un evaluador que ve trampas en todos lados
es tan inútil como uno que no ve ninguna.

---

## Resumen de los tres casos

| | Excelente | Flojo | Tramposo |
|---|---|---|---|
| D1 · Sistema | 30,00 | 7,50 | 7,50 |
| D2 · Proceso | 25,00 | 6,25 | 0,00 |
| D3 · Formato | 15,00 | 3,75 | 7,50 |
| D4 · Económico | 11,25 | 3,75 | 15,00 |
| D5 · Gobierno | 11,25 | 3,75 | 3,75 |
| **TOTAL** | **92,50** | **25,00** | **33,75** |
| Veredicto | Excelente | Crítico | Crítico |
| Contradicciones esperadas | 0 | 0 | 4+ |
| Alertas de integridad | 0 | 0 | 1 |

**El indicador clave de D2:** el flojo (6,25) supera al tramposo (0,00). El
proceso modesto pero honesto vale más que el elaborado y falso. Si el
corrector invierte ese orden, la rúbrica falló en su objetivo central.
