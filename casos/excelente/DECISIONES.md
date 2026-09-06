# DECISIONES.md

Historia de construcción del Agente de Rentabilidad y Cobranzas.
Del 20 al 26 de agosto de 2026.

---

## Iteraciones del contrato

Se conservan las tres versiones en `prompts/`:

| Versión | Fecha | Archivo | Estado |
|---|---|---|---|
| v1 | 2026-08-20 | `prompts/versiones/system_prompt_v1.md` | Superada |
| v2 | 2026-08-23 | `prompts/versiones/system_prompt_v2.md` | Superada |
| v3 | 2026-08-25 | `prompts/system_prompt.md` | Vigente |

### De v1 a v2 — el margen mal calculado

**Qué pasó.** La primera corrida devolvió un margen total de 199.997,00
euros. Verifiqué contra la planilla y el número correcto era 171.955,35. La
diferencia, 28.041,65, coincidía exactamente con la suma de la columna
IMPORTE COMERCIAL. Log completo en `logs/2026-08-22_error_margen.log`.

**Diagnóstico.** El contrato v1 decía "calculá el margen de cada curso". El
modelo interpretó margen como precio menos costo del profesor y omitió la
comisión. No fue una alucinación: fue una instrucción ambigua.

**Qué cambié.** Escribí la fórmula explícita en el contrato. En v3 pasó a
ser regla dura RD1, con la advertencia de que omitir la comisión es el error
más frecuente.

**Qué aprendí.** El modelo no adivina reglas de negocio. Si la regla no está
escrita, la inventa de forma plausible — que es peor que fallar, porque el
número parece correcto.

### De v1 a v2 — la salida distinta cada vez

**Qué pasó.** v1 pedía "un reporte claro para la dirección". Tres corridas
sobre los mismos datos dieron tres estructuras distintas: distinto orden de
secciones, distinta cantidad de decimales, a veces con ranking de
comerciales y a veces sin.

**Qué cambié.** JSON con estructura fija, campo por campo, y regla de
ordenamiento de las listas. Fue el cambio que más mejoró el sistema.

### De v2 a v3 — el dato incompleto

**Qué pasó.** Probé con una fila a la que le borré el IMPORTE PROFESOR. El
agente completó el hueco: puso un valor estimado a partir del promedio de
los otros cursos del mismo profesor, y no lo avisó en ningún lado. El
reporte salió con un número inventado que parecía real.

**Qué cambié.** Agregué la regla dura RD3 (nunca estimar valores faltantes)
y la tabla de casos borde de la sección 5, con el contador
`registros_excluidos` en el output para que el hueco sea visible.

**Qué aprendí.** Un agente que completa huecos silenciosamente es más
peligroso que uno que falla ruidosamente.

---

## Decisiones

### D1 · Achicar el alcance: fuera la proyección de ventas

**Alternativa descartada:** incluir proyección de facturación de los
próximos doce meses.

**Por qué:** la base tiene 362 cursos entre 2019 y 2021, sin variables de
mercado ni pipeline comercial. Cualquier proyección sería una extrapolación
de la serie histórica presentada como pronóstico. Preferí un sistema chico
que no miente a uno grande que estima sin base.

### D2 · Fuera el análisis de desempeño de profesores

**Alternativa descartada:** ranking de profesores por margen generado.

**Por qué:** técnicamente trivial —los datos están— pero el margen de un
curso depende del precio al cliente, que fija el comercial, no el profesor.
El ranking mediría algo que el profesor no controla. Un indicador
injusto que además puede tener consecuencias laborales.

### D3 · JSON en lugar de texto libre

**Alternativa descartada:** salida en markdown para pegar directo en el
correo del comité.

**Por qué:** el objetivo del sistema es comparabilidad entre períodos. Con
texto libre no hay comparación posible. El resumen ejecutivo quedó como
campo dentro del JSON: se copia y se pega igual, pero la estructura se
mantiene.

### D4 · Lectura de archivo local en lugar de API de Google Sheets

**Alternativa descartada:** conectar a Sheets para correr sobre el dato vivo.

**Por qué:** es la mejor opción técnica y la habría elegido con más tiempo.
Configurar credenciales y permisos me llevaba dos días que no tenía. El
lector local funciona y está documentado en `herramienta/config_lector.md`,
con la limitación declarada: hay que descargar la planilla antes de correr.

**Es una deuda, no una decisión de diseño.** Lo digo así para no vender como
elección lo que fue una restricción de tiempo.

### D5 · Nivel de autonomía L2 en lugar de L3

**Alternativa descartada:** que el agente distribuya el reporte al comité
automáticamente.

**Por qué:** el reporte alimenta decisiones de cobranza sobre clientes
concretos. Un error en la cartera impaga puede terminar en un reclamo a un
cliente que ya pagó. La revisión humana previa a la distribución no es
opcional.

---

## Cambios de alcance

| Qué saqué | Cuándo | Por qué |
|---|---|---|
| Proyección de ventas a 12 meses | 2026-08-20, antes de escribir el contrato | Sin variables de mercado, sería extrapolación disfrazada de pronóstico (D1) |
| Ranking de profesores | 2026-08-20 | Mide algo que el profesor no controla (D2) |
| Conexión a Google Sheets | 2026-08-24 | Falta de tiempo para configurar credenciales (D4). Es deuda declarada, no diseño |
| Alerta automática por correo al superar umbral de morosidad | 2026-08-25 | Requería nivel L3 y un canal de salida que no está en alcance. Queda como pendiente |

---

## Lo que quedó pendiente

- Conexión a Google Sheets (D4).
- Alerta automática por umbral de morosidad.
- La proyección económica está calculada solo en horizonte anual. Falta el
  horizonte semanal, que el trabajo pide y no llegué a agregar. Lo declaro
  acá para que no se lea como omisión involuntaria.
