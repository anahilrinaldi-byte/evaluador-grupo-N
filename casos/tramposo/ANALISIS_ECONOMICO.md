# Análisis económico

## Medición de tokens

Medición real tomada sobre las corridas ejecutadas, con el contador de la
consola de la API.

| Corrida | Tokens entrada | Tokens salida |
|---|---|---|
| Corrida 01 | 18.420 | 1.240 |
| Corrida 02 | 18.420 | 1.310 |
| **Promedio** | **18.420** | **1.275** |

Los tokens de entrada son estables porque la base enviada es siempre la
misma (362 registros más el system prompt). La variación está en la salida.

## Costo por corrida

Modelo utilizado: **GPT-4o mini**.
Tarifa vigente al 2026-08-30, tomada de la página de precios de OpenAI:

- Entrada: USD 0,150 por millón de tokens
- Salida: USD 0,600 por millón de tokens

Cálculo:

- Entrada: 18.420 / 1.000.000 × 0,150 = **USD 0,00276**
- Salida: 1.275 / 1.000.000 × 0,600 = **USD 0,00077**
- **Costo por corrida: USD 0,00353**

## Proyección de operación

Supuesto de uso: el reporte es mensual, pero durante el cierre se corre
varias veces para validar. Estimo **3 corridas por semana** en promedio
(cierre mensual más validaciones y consultas ad hoc de la dirección).

| Horizonte | Corridas | Costo |
|---|---|---|
| Semanal | 3 | USD 0,0106 |
| Anual | 156 | USD 0,5507 |

El costo es marginal frente al ahorro: el armado manual del reporte tomaba
aproximadamente 2 horas mensuales de un analista, unas 24 horas al año.

## Elección de modelo

Se evaluaron dos opciones:

**GPT-4o mini (elegido).** Resuelve correctamente la agregación y el
formato JSON, con costo por corrida de USD 0,00353.

**GPT-4o (descartado).** Tarifa de USD 2,50 por millón de tokens de entrada,
unas 16 veces más caro: el costo por corrida sube a aproximadamente USD
0,0468. Se corrió una prueba con este modelo y el resultado fue equivalente
al del modelo chico: mismos números, misma estructura, mismo nivel de
detalle en el resumen ejecutivo.

Aplicando el criterio del curso —el modelo más chico que hace bien la
tarea— se eligió 4o mini. La tarea es agregación aritmética y formateo, no
razonamiento complejo: no justifica un modelo frontier.
