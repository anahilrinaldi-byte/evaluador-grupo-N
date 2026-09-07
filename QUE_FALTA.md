# Qué falta — traspaso

Estado al **lunes 7/9**. Entrega el **miércoles 9 a la noche**, prueba de fuego el
**jueves 10 a las 18:59**.

Quedan **tres cosas**. Ninguna se puede hacer sin una persona del grupo: no es
que falten por tiempo, es que dependen de la API key, del criterio humano de cada
uno, o de un archivo del campus.

---

## 1 · Correr el agente de verdad — TATI

**Por qué es lo primero.** El criterio de 25 puntos dice que el corrector «corre
sobre un repo real y devuelve el formato completo». Las tres corridas que están en
`calibracion/` son la rúbrica aplicada componente por componente, **no una
ejecución de `app.py`**. Sos la única con la API key.

**Cómo.** La app quedó con tres bugs corregidos y ahora acepta subcarpetas:

```
streamlit run app.py
```

Con `GEMINI_API_KEY` en `.streamlit/secrets.toml` o como variable de entorno.
Evaluá estas tres URLs, una por vez:

```
https://github.com/anahilrinaldi-byte/evaluador-grupo-N/tree/main/casos/excelente
https://github.com/anahilrinaldi-byte/evaluador-grupo-N/tree/main/casos/flojo
https://github.com/anahilrinaldi-byte/evaluador-grupo-N/tree/main/casos/tramposo
```

Guardá cada JSON como `calibracion/app-excelente.json`, `app-flojo.json`,
`app-tramposo.json`, y anotá **modelo y temperatura usados**, que lo pide
`agente/configuracion` §6.

**Qué mirar.** La interfaz ahora dice si la corrida es **válida** antes de mostrar
la nota. Si sale inválida, se archiva como corrida fallida y se corre de nuevo —
la corrida fallida **no se borra**, es evidencia de proceso.

**Contra qué comparar.** Las corridas a mano dan 92,50 · 25,00 · 22,50. Si Gemini
coincide, es determinismo entre correctores distintos y es material fuerte. Si
difiere, ese desacuerdo también es calibración: se documenta dónde y por qué.

*Prompt para tu agente:*

> Cloná https://github.com/anahilrinaldi-byte/evaluador-grupo-N y leé `app.py`,
> `agente/system_prompt.md`, `rubrica.md` y `agente/configuracion`. Ayudame a
> correr la app con mi GEMINI_API_KEY sobre las tres subcarpetas de `casos/`,
> guardar cada salida JSON en `calibracion/app-<caso>.json` con el modelo y la
> temperatura usados, y compararlas contra `calibracion/corrida-*.json`.
> Documentá cualquier diferencia en un archivo nuevo.

---

## 2 · Dos decisiones de criterio — ANAHÍ

Las dos cambian una nota objetivo, así que **hay que resolverlas antes de la ronda
a ciegas**. Son tu rúbrica y tus casos.

### 2.1 · El desacuerdo de D1 — 7,50 puntos

`calibracion/desacuerdo-D1-tramposo.md`. ¿El output estructurado se verifica
contra el formato que el trabajo **declaró**, o solo contra sí mismo? El tramposo
declara un JSON y entrega informes en prosa: cae justo en el medio.

### 2.2 · El desacuerdo de D3 — 3,75 puntos

`calibracion/desacuerdo-D3-tramposo.md`. Las notas de diseño acreditan
«instrucciones OK» y **el tramposo no tiene instrucciones de ejecución en ningún
archivo**. Es el más claro de los dos: no es una lectura ambigua, es un componente
acreditado sin evidencia.

### Y una consecuencia que cambia una decisión abierta

Con D3 corregido, **el tramposo saca 22,50 y el flojo 25,00**. El resultado
contraintuitivo que identificaste —«el tramposo saca más que el flojo»— **se da
vuelta solo**. Quizá no haga falta escribir la regla de integridad agregada que
estaba en discusión: el problema era una nota mal calculada, no la rúbrica.

### 2.3 · Y una tercera, si querés: ¿P9 es demasiado fuerte?

`calibracion/impacto-P9-en-D4.md`. El principio injertado de la v3 deja D4 con
techo práctico en N3, porque casi ningún trabajo adjunta el contador de tokens.
Puede ser correcto o puede ser una vara imposible.

*Prompt para tu agente:*

> En https://github.com/anahilrinaldi-byte/evaluador-grupo-N hay tres decisiones
> de calibración pendientes, documentadas en `calibracion/desacuerdo-D1-tramposo.md`,
> `desacuerdo-D3-tramposo.md` e `impacto-P9-en-D4.md`. Leelas junto con
> `rubrica.md` y `casos/tramposo/`. Ayudame a decidir cada una, y por cada decisión
> que tomemos actualizá el texto de la condición en `rubrica.md` y la nota objetivo
> en `casos/NOTAS_DE_DISENO.md`.

---

## 3 · El README estándar de la materia — ~~MIGUE~~ **HECHO**

**Migue lo subió el 7/9 a las 17:20.** Sigue la estructura estándar de la materia
—qué construimos, cómo se lo pedimos, qué funciona, qué falló, qué aprendimos— y
sus afirmaciones técnicas están verificadas contra el código: la priorización de
archivos, el soporte de subcarpetas, la validación de salida y el reporte de árbol
truncado son todas ciertas.

Único ajuste posterior: la lista de estructura no incluía `calibracion.md`, que es
uno de los siete elementos que exige la consigna. Agregado.

*Prompt para tu agente:*

> Te paso el README estándar de la materia. El repo es
> https://github.com/anahilrinaldi-byte/evaluador-grupo-N y su `README.md` tiene
> solo el título y los integrantes. Armame el README completo: el estándar, los
> cuatro integrantes, y una guía corta de qué hay en el repo y cómo correr el
> evaluador. Mirá `app.py` para las instrucciones de ejecución y
> `PRUEBA_DE_FUEGO.md` para el contexto.

---

## 4 · La corrección a ciegas — LOS CUATRO

Es la pieza de **15 puntos** y la única que no se puede hacer sin los cuatro.
**Las plantillas ya están**: `calibracion/ronda1-tunombre.md`, con la grilla y la
tabla de valores permitidos.

**Cada uno completa el suyo, solo, y lo commitea. Nadie mira el de otro hasta que
los cuatro estén subidos.** Si nos ponemos de acuerdo antes, el archivo queda
vacío de lo único que se corrige.

*Prompt para el agente de cada uno:*

> Tengo que hacer una corrección a ciegas. Repo:
> https://github.com/anahilrinaldi-byte/evaluador-grupo-N
> Leé `rubrica.md` completa y los tres casos de `casos/`. Ayudame a puntuar los
> tres en las cinco dimensiones aplicando la rúbrica componente por componente.
> La rúbrica usa anclas discretas: no hay valores intermedios. El nivel sale de
> contar los cuatro componentes (verificado 1, parcial 0,5, no verificado 0),
> sumar y truncar hacia abajo.
>
> **NO leas `calibracion/` ni `casos/NOTAS_DE_DISENO.md`**: tienen las respuestas
> y me contaminan. Quiero llegar a mi propio criterio.
>
> Volcá el resultado en `calibracion/ronda1-MINOMBRE.md`, que ya tiene la plantilla.

**Esa línea de "no leas las respuestas" es la más importante del documento.** Sin
ella los cuatro agentes convergen al mismo número y la calibración no mide nada.

---

# Lo que ya está cerrado

| | Estado |
|---|---|
| **Rúbrica ejecutable** | Una sola versión. Cinco dimensiones, escalas de cinco niveles con anclas discretas, ejemplos de nivel alto y bajo por dimensión, nueve principios. Las diez citas de los ejemplos verificadas literalmente contra los archivos |
| **Agente corrector** | `app.py` con cuatro defectos de falla silenciosa corregidos y la validación de corrida implementada. Probado de punta a punta sobre cuatro objetivos |
| **Casos de prueba** | Los tres completos, estructura reconstruida, las cinco ausencias deliberadas verificadas una por una |
| **Calibración** | Tres corridas archivadas, dos desacuerdos documentados con su recomendación, un análisis de impacto |
| **Prueba de fuego** | `PRUEBA_DE_FUEGO.md` con los tres roles, los cinco casos que probablemente tiren y qué decir si falla en vivo |
| **Proceso** | 63 commits, los cuatro integrantes presentes |

## Los controles que corrimos sobre todo esto

- Los valores de ancla coinciden en la rúbrica, en `app.py` y en las tres corridas
- Las tres corridas pasan el validador y la aritmética de conteo cierra en las quince dimensiones
- Las claves del esquema de salida coinciden entre el system prompt, el validador y las corridas
- Todos los valores están dentro del vocabulario controlado de §6.2
- Ningún documento cita un número desactualizado
- Todas las referencias de archivo resuelven, salvo las ausencias deliberadas
