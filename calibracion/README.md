# Carpeta de calibración

Qué hay acá y para qué sirve cada cosa.

## Corridas del corrector

| Archivo | Qué es |
|---|---|
| `corrida-excelente.json` | Salida completa sobre `casos/excelente`. Total **92,50** |
| `corrida-flojo.json` | Salida completa sobre `casos/flojo`. Total **25,00** |
| `corrida-tramposo.json` | Salida completa sobre `casos/tramposo`. Total **22,50** |

Las tres son la rúbrica aplicada componente por componente, con el JSON del
esquema de la CAPA 6. **No se ejecutaron con `app.py` y la API key.** Cuando se
corran con la app, se archivan al lado de estas y se comparan: si coinciden,
tenemos determinismo entre correctores distintos, que es material fuerte. Si
difieren, ese desacuerdo también es calibración.

## Desacuerdos

`desacuerdo-D1-tramposo.md` — 7,50 puntos de diferencia contra la nota objetivo,
y salen de **una sola decisión de componente**: cuánto vale `output_estructurado`
cuando el trabajo declara un formato JSON y entrega informes en prosa.

**Lo decide la lente de construcción, y hay que hacerlo antes de la ronda 1**,
porque cambia una nota objetivo.

## Ronda 1 — corrección a ciegas

`ronda1-anahi.md` · `ronda1-tati.md` · `ronda1-migue.md` · `ronda1-gonzalo.md`

Cada uno completa **el suyo** y lo commitea. **Nadie mira el de otro hasta que
los cuatro estén subidos.**

### Por qué a ciegas

Si nos ponemos de acuerdo antes, el archivo queda vacío de lo único que se
corrige. El enunciado del parcial dice, textual, que un desacuerdo honesto y bien
resuelto suma más que una calibración perfecta sin historia.

### Qué se hace con los resultados

Se miden **dos brechas distintas**, y confundirlas es el error clásico:

- **Humano contra humano.** Si los cuatro discrepamos entre nosotros, el problema
  es de la **rúbrica**: un descriptor admite dos lecturas.
- **Humano contra agente.** Si coincidimos entre nosotros y el corrector se
  aparta, el problema es del **prompt**.

### Antes de arrancar, dos chequeos

1. **¿La escala del medio está probada?** Por cada dimensión, mirar si algún caso
   cae en N2 o N3. Si los tres dan N0, N1 o N4, esa parte de la escala nunca se
   probó — y es donde va a caer la mayoría de los trabajos reales del domingo 13.
2. **¿El cambio se probó contra los tres casos?** Un descriptor endurecido para
   atrapar al tramposo puede estar castigando al excelente sin que nadie lo note.
