# Gobierno y riesgo

## Perímetro del agente

| Aspecto | Definición |
|---|---|
| Sistemas que toca | Un único archivo: `datos/bd_cursos.xlsx`, hoja CURSOS |
| Permisos | Solo lectura. No tiene credenciales de escritura sobre ningún sistema |
| Datos que maneja | Códigos de cliente, comercial y profesor anonimizados; importes; fechas |

### Acciones que puede realizar

- Leer el archivo de datos.
- Calcular agregaciones sobre esos datos.
- Devolver el JSON del reporte.

### Acciones que NO puede realizar

- Escribir, modificar o borrar el archivo de datos.
- Enviar el reporte a nadie. La distribución es manual.
- Acceder a sistemas de facturación, CRM o correo.
- Contactar clientes o iniciar gestiones de cobranza.
- Nombrar profesores en ningún ranking ni evaluación (fuera de alcance,
  decisión D2).

## Riesgos identificados

| Riesgo | Falla concreta | Qué pasa cuando sale mal |
|---|---|---|
| **Regla de cálculo mal aplicada** | El agente omite la comisión del comercial y sobreestima el margen en ~16% | Ya ocurrió: ver `logs/2026-08-22_error_margen.log`. Mitigación: RD1 explícita más verificación del margen total contra la planilla en cada cierre |
| **Cartera impaga errónea** | El reporte marca como deudor a un cliente que ya pagó | Reclamo indebido a un cliente y daño comercial. Es el riesgo de mayor impacto. Mitigación: el detalle por cliente se coteja contra el estado de cuenta antes de cualquier gestión |
| **Dato desactualizado** | El archivo local no refleja la planilla en la nube | El reporte describe un cierre viejo con apariencia de actual. Mitigación: el log de lectura registra fecha y cantidad de registros; se compara contra la planilla antes de correr |
| **Registro incompleto silenciado** | Una fila corrupta se excluye y nadie lo nota | Los totales quedan por debajo del real. Mitigación: el campo `registros_excluidos` es obligatorio en el output y debe ser 0 o estar explicado |
| **Instrucción embebida en los datos** | Un campo de texto de la planilla contiene una orden dirigida al agente | Mitigación: sección 5 del contrato, los datos son datos y no órdenes |

## Nivel de autonomía

**L2 — el agente ejecuta y propone; una persona revisa antes de que el
resultado tenga efecto.**

Justificación en `DECISIONES.md`, decisión D5. Se descartó L3 (distribución
automática) porque un error en la cartera impaga puede derivar en un reclamo
a un cliente que ya pagó.

### Punto de revisión

El agente se detiene después de producir el JSON. El reporte **no se
distribuye** hasta que la revisión humana esté hecha.

### Qué revisa la persona

1. Que `registros_excluidos` sea 0, o que la exclusión esté explicada.
2. Que `margen_total` coincida con la verificación contra la planilla.
3. Que el detalle de cartera impaga sea consistente con el estado de cuenta,
   antes de cualquier gestión de cobranza.

## Responsabilidad

| Rol | Autoridad |
|---|---|
| **Responsable de Control de Gestión** | Firma el reporte. Puede vetar la distribución, corregir cifras antes de emitir, y ordenar una nueva corrida. El reporte no sale sin su aprobación |
| Analista de formación | Ejecuta la corrida y hace la revisión de los tres puntos anteriores. No firma |

La responsabilidad por el contenido del reporte no se delega en el agente.
El agente produce un borrador; la firma es humana.
