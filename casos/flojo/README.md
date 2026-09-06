# Reporte de cursos con IA

Trabajo Final - Programación de y con Agentes de IA - MBA UCEMA

## Qué hice

Un agente que analiza la base de cursos de una empresa de formación y
devuelve un reporte con la facturación, el margen y lo que está sin cobrar.

La base tiene 362 cursos de España, Italia y Francia entre 2019 y 2021.

## Cómo funciona

Le paso los datos de la planilla al agente en el prompt y le pido el
reporte. El system prompt está en `prompts/system_prompt.md`.

El agente devuelve el análisis con los totales y el detalle por país.

## Resultado

La corrida que hice está en `corridas/corrida_01/`. Los números que sacó:

- Facturación total: 303.905 euros
- Margen: 171.955,35 euros
- Sin cobrar: 88.700 euros en 103 cursos

Lo verifiqué contra la planilla y está bien.

## Supervisión

Yo reviso el reporte antes de usarlo. El agente no manda nada a nadie ni
escribe en ningún sistema, solo lee y devuelve el texto.

## Costo

Usé GPT-4o mini. La corrida consumió aproximadamente 15.000 tokens de
entrada según el contador de la consola. Con la tarifa de entrada de 0,150
dólares por millón de tokens, sale alrededor de 0,0023 dólares por corrida.

Es un costo muy bajo, prácticamente irrelevante para el uso que le voy a
dar.

## Riesgos

El principal riesgo es que el modelo se equivoque en los números o invente
algún dato. Por eso reviso el resultado antes de usarlo.

## Archivos

```
README.md
prompts/system_prompt.md
corridas/corrida_01/
DECISIONES.md
```
