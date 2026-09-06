# Configuración de la herramienta de lectura

## Qué es

El agente no recibe los datos pegados en el prompt: los lee del archivo.
Esto es lo que lo convierte en un agente y no en un chatbot con contexto.

## Herramienta

| Campo | Valor |
|---|---|
| Tipo | Lectura de archivo local |
| Archivo | `datos/bd_cursos.xlsx` |
| Hoja | `CURSOS` |
| Modo | Solo lectura |
| Registros esperados | 362 (fila 1 = encabezado) |

## Columnas requeridas

El lector valida que existan las quince columnas del contrato antes de
entregar los datos al agente. Si falta alguna, corta y devuelve el error
`columna_faltante`, que el agente traduce al JSON de salida.

## Verificación de lectura

Cada corrida deja un log en `logs/`. El log registra: archivo leído, hoja,
cantidad de filas, columnas detectadas y registros excluidos por dato
incompleto.

## Limitación conocida

El lector trabaja sobre el archivo local. Si el equipo comercial actualiza
la planilla en la nube, hay que volver a descargarla. Se evaluó conectar
directamente a Google Sheets vía API y se descartó por tiempo: ver
`DECISIONES.md`, decisión D4.
