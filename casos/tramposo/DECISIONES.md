# DECISIONES.md — La historia de construcción

## Cómo llegué hasta acá

Este documento cuenta el recorrido completo del proyecto, desde la idea
inicial hasta el sistema que funciona hoy. Fueron muchas iteraciones y
bastantes frustraciones, y creo que eso es lo más valioso para compartir.

## Iteración 1 — El planteo inicial

Arranqué pensando en un agente que hiciera todo: reporte, proyección de
ventas, recomendación de precios y análisis de desempeño de profesores.
Rápidamente me di cuenta de que era demasiado. El contrato quedaba enorme y
el agente se dispersaba.

## Iteración 2 — Achicar el alcance

Acá tomé la primera decisión importante: **saqué la proyección de ventas y
la recomendación de precios**. Me quedé solo con el reporte histórico de
rentabilidad y cobranzas. La razón fue que proyectar requería supuestos de
mercado que no estaban en la base, y el agente terminaba inventando. Preferí
un sistema chico que funciona a uno grande que alucina.

También saqué el análisis de desempeño de profesores. Era técnicamente
posible pero me pareció delicado desde lo laboral, y no aportaba al problema
que quería resolver.

## Iteración 3 — El problema del formato

La primera versión del contrato pedía la salida en texto libre. El resultado
era distinto cada vez: a veces empezaba por el margen, a veces por la
cartera, y los números venían con formatos distintos. Imposible de comparar
entre meses.

Cambié el contrato para exigir JSON con estructura fija. Fue el cambio que
más mejoró el sistema.

## Iteración 4 — Errores de cálculo

En las primeras pruebas el agente calculaba mal el margen: restaba solo el
importe del profesor y se olvidaba de la comisión del comercial. Tuve que
escribir la fórmula explícita en el contrato. Después de eso, los números
cerraron contra el cálculo manual.

## Iteración 5 — La conexión con Sheets

Al principio le pasaba el Excel descargado. Funcionaba pero era manual y
había que descargar la planilla cada vez. Migré a la API de Google Sheets
para que corra sobre el dato vivo. Costó configurarlo pero valió la pena.

## Iteración 6 — Ajuste final

Últimos retoques al resumen ejecutivo para que salga en el tono que espera
la dirección: directo, sin jerga técnica, con el número importante primero.

## Lo que aprendí

- Achicar el alcance temprano fue la mejor decisión del proyecto.
- Un contrato sin formato de salida fijo no sirve para nada repetible.
- La IA no adivina reglas de negocio: hay que escribirlas explícitas.
- Documentar mientras se construye es mucho más difícil de lo que parece.

## Pendientes

Me hubiera gustado agregar alertas automáticas por mail cuando la morosidad
supera un umbral, pero no llegué con los tiempos.
