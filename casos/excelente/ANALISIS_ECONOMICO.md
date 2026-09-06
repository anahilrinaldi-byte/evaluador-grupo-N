# Análisis económico

## Consumo de tokens

Medición real tomada de la consola de la API en las tres corridas
documentadas en `corridas/`.

| Corrida | Filtro | Tokens entrada | Tokens salida |
|---|---|---|---|
| 01 | Sin filtro, 362 registros | 21.480 | 1.390 |
| 02 | España, 149 registros | 9.240 | 980 |
| 03 | Año 2021, 120 registros | 7.860 | 1.050 |
| **Promedio** | | **12.860** | **1.140** |

Los tokens de entrada varían con la cantidad de registros filtrados. Para el
costeo se usa la corrida 01, que es el peor caso: base completa.

## Costo por corrida

**Modelo:** GPT-4o mini
**Tarifa:** consultada el 2026-08-26 en la página de precios de OpenAI
- Entrada: USD 0,150 por millón de tokens
- Salida: USD 0,600 por millón de tokens

**Peor caso (corrida 01, base completa):**
- Entrada: 21.480 / 1.000.000 × 0,150 = USD 0,003222
- Salida: 1.390 / 1.000.000 × 0,600 = USD 0,000834
- **Costo por corrida: USD 0,004056**

**Caso promedio:**
- Entrada: 12.860 / 1.000.000 × 0,150 = USD 0,001929
- Salida: 1.140 / 1.000.000 × 0,600 = USD 0,000684
- **Costo por corrida: USD 0,002613**

## Proyección anual

**Supuesto de volumen:** el reporte es un cierre mensual. Se corre una vez
para el cierre general y una vez por cada uno de los tres países, más
aproximadamente cuatro consultas ad hoc de la dirección por mes. Total
estimado: **8 corridas mensuales, 96 anuales**.

| Concepto | Cálculo | Resultado |
|---|---|---|
| Costo anual (peor caso) | 96 × USD 0,004056 | **USD 0,39** |
| Costo anual (promedio) | 96 × USD 0,002613 | **USD 0,25** |

**Contraste con el costo actual.** El armado manual del reporte toma unas 2
horas por cierre. A 12 cierres anuales son 24 horas de un analista.

> **Pendiente declarado:** el trabajo pide la proyección en horizonte
> semanal y anual. Acá está calculada solo la anual. El horizonte semanal no
> se agregó por falta de tiempo; con el supuesto de 8 corridas mensuales el
> cálculo es directo, pero prefiero no presentar como hecho algo que no
> ejecuté ni verifiqué.

## Elección de modelo

Criterio del curso: el modelo más chico que hace bien la tarea.

**Probado primero: GPT-4o mini.** Resolvió correctamente las agregaciones y
respetó el JSON en las tres corridas. Los números coinciden con la
verificación manual contra la planilla.

**Alternativa evaluada: GPT-4o.** Tarifa de entrada USD 2,50 por millón,
unas 16,7 veces más cara: el costo por corrida sube a USD 0,0562. Se corrió
una vez sobre la base completa y devolvió exactamente los mismos números y
la misma estructura.

**Decisión: 4o mini.** La tarea es lectura, agregación aritmética y
formateo. No hay razonamiento ambiguo ni interpretación de texto libre. Un
modelo mayor no aporta nada verificable y cuesta 14 veces más.

**Condición de revisión:** si el volumen de la base creciera al punto de
exceder la ventana de contexto, o si se agregaran análisis cualitativos
sobre comentarios de clientes, habría que reevaluar.
