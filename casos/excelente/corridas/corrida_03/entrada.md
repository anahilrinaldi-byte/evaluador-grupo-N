# Entrada · Corrida 03

**Fecha de ejecución:** 2026-08-26 09:02
**Archivo de datos:** `datos/bd_cursos.xlsx`, hoja CURSOS, 362 registros
**Modelo:** GPT-4o mini · temperatura 0

Objetivo de esta corrida: verificar el filtro temporal y comprobar que el
reporte es utilizable como cierre anual, que es el uso previsto en
producción.

## User prompt enviado

```
Generá el reporte de rentabilidad y cobranzas sobre la base de cursos
disponible en datos/bd_cursos.xlsx.

Filtro a aplicar: AÑO=2021
Período informado: Cierre anual 2021
Fecha de corrida: 2026-08-26

Devolvé únicamente el JSON de la sección 6 del contrato.
```
