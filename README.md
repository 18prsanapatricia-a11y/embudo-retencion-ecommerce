# Análisis de embudo de conversión y retención — E-commerce

Análisis del embudo de conversión de compra y de la retención de usuarios de una plataforma de e-commerce en 10 países de Latinoamérica, desarrollado con **SQL** (CTEs y JOINs) sobre datos de eventos de usuario entre enero y agosto de 2025.

---

## Contexto del problema

El negocio necesitaba responder dos preguntas centrales: **dónde se están perdiendo los usuarios dentro del proceso de compra**, y **qué tan bien se les retiene después de registrarse**. El análisis debía además desagregarse por país, ya que el comportamiento del usuario podía variar significativamente entre mercados, y por cohorte de registro, para detectar si algún mes en particular presentaba un problema distinto al resto.

---

## Herramientas utilizadas

| Herramienta | Uso en el proyecto |
|---|---|
| **SQL** | Motor principal del análisis |
| **CTEs (Common Table Expressions)** | Estructuración de las consultas de embudo y retención en pasos lógicos y legibles |
| **JOINs** | Cruce de eventos de usuario con su información de país y fecha de registro |
| Funciones de ventana / agregación | Cálculo de tasas de conversión entre etapas y de retención por día (D7, D14, D21, D28) |
| Hoja de resumen ejecutivo | Presentación de hallazgos, implicaciones y reflexión personal sobre el análisis |

---

## Metodología

1. **Definición del embudo.** Se modelaron 6 etapas del proceso de compra: `select_item → add_to_cart → begin_checkout → add_shipping_info → add_payment_info → purchase`.
2. **Cálculo de conversión con CTEs.** Cada etapa se calculó como una CTE independiente que contaba usuarios únicos, encadenadas mediante JOINs para obtener la tasa de conversión entre etapa y etapa, tanto a nivel general como por país.
3. **Análisis de retención por cohorte.** Los usuarios se segmentaron por mes de registro (cohorte) y se midió qué porcentaje seguía activo a los 7, 14, 21 y 28 días, tanto de forma agregada como por país.
4. **Validación cruzada de resultados.** Los hallazgos por país se contrastaron entre sí para diferenciar patrones de comportamiento reales de posibles problemas de registro de datos (tracking).

---

## Hallazgos principales

### Embudo de conversión

| Hallazgo | Detalle |
|---|---|
| Mayor punto de fuga del embudo | La transición hacia `add_to_cart` concentra la mayor caída de usuarios en **todos** los países, con una baja promedio del **65.49%** |
| Caso a validar — Uruguay y Bolivia | Tasa de abandono del **0%** entre `begin_checkout` y `purchase`; señal positiva, pero también posible falla de tracking a validar con el equipo técnico |
| Caso a validar — Paraguay | Ausencia total de conversiones después de `add_to_cart`, lo que sugiere una posible incidencia en el flujo de compra o en el registro de eventos para ese mercado |
| Perú | Mejor conversión en la etapa inicial del embudo, pero pierde el **74.55%** de usuarios específicamente en la conversión a `add_to_cart` |

### Retención de usuarios

| Hallazgo | Detalle |
|---|---|
| Caída generalizada | La tasa de abandono entre el día 14 y el día 28 es del **48.86%**, un patrón consistente en todos los países |
| Mejor retención a largo plazo | Perú no lidera la retención inicial, pero termina con la mejor retención a 28 días, **0.85% por encima** del promedio del grupo |
| Cohorte con comportamiento distinto | La cohorte de **agosto** presenta una retención significativamente menor desde el día 7, en comparación con el resto de los meses del periodo analizado |

---

## Implicaciones para el negocio

- Validar el registro de eventos (tracking) en Uruguay, Bolivia y Paraguay antes de tomar decisiones basadas en sus tasas de conversión, ya que los patrones observados son atípicos frente al resto del grupo.
- Priorizar la optimización de la etapa `add_to_cart`, el mayor punto de fuga del embudo en todos los mercados, en conjunto con el equipo de marketing.
- Investigar qué ocurrió durante agosto (cambios de producto, campañas, incidentes) que explique la caída de retención de esa cohorte específica.
- Reforzar la experiencia de usuario después de la segunda semana de registro, dado que la caída de retención entre el día 14 y 28 es un patrón estructural y no aislado a un país.

---

## Reflexión del análisis

La etapa que se priorizaría mejorar primero es `add_to_cart`, por concentrar el mayor punto de fuga del embudo. El comportamiento del usuario resultó bastante consistente entre países en cuanto a retención en las primeras semanas, lo cual es esperable en un funnel de este tipo — la señal más relevante no fue un país aislado, sino el patrón repetido de abandono en la transición hacia `add_to_cart` across todos los mercados.

---

## Archivos del repositorio

```
├── data/
│   └── embudo_retencion_ecommerce.xlsx   # Informe ejecutivo, embudo y retención por país/cohorte
└── README.md
```

---

## Habilidades demostradas

- Consultas SQL avanzadas con CTEs y JOINs para modelar embudos de conversión
- Análisis de retención de usuarios por cohorte (D7/D14/D21/D28)
- Segmentación y comparación de métricas entre múltiples países
- Distinción entre hallazgos de negocio reales y posibles errores de instrumentación de datos
- Redacción de hallazgos, implicaciones y reflexión crítica orientada a stakeholders
