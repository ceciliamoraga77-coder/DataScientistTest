# Informe Ejecutivo: Modelo de Probabilidad de Default
**Para:** Gerencia de Riesgo, Andina Crédito
**Fecha:** Julio 2026

## 1. Contexto y Hallazgos en la Información Histórica
Durante la auditoría inicial de las 45.300 solicitudes históricas, se identificaron y corrigieron patrones que afectaban la calidad de la información:
*   **Anomalía en Ingresos Declarados:** Aproximadamente un 10% de las solicitudes presentaban ingresos irrisorios (menores a $10.000 CLP). Se determinó que correspondía a un error sistemático de digitación (montos ingresados en miles). Se aplicó un factor de corrección para no distorsionar la evaluación de la carga financiera del cliente.
*   **Ausencia de Antigüedad Laboral:** Se detectó que los valores faltantes en esta variable correspondían en su totalidad a trabajadores informales y jubilados. En lugar de imputar promedios que sesgarían el análisis, se aislaron como un segmento independiente, permitiendo una evaluación más justa.

## 2. Enfoque Metodológico y Rendimiento Esperado
Para la predicción de mora (90+ días), se implementó un algoritmo avanzado (LightGBM). Su desarrollo se enfocó en ser una herramienta de apoyo técnico-colaborativo, diseñada para integrarse fluidamente con las reglas de negocio del equipo de Riesgo. 

Para garantizar una estimación honesta de su rendimiento en producción, se utilizó una validación *Out-of-Time* (entrenando con datos hasta octubre de 2024 y evaluando con meses posteriores).
*   **Performance:** El modelo demostró una alta capacidad para identificar prospectos riesgosos sin sacrificar una cantidad inmanejable de buenos clientes (reflejado en métricas robustas ante el desbalance de clases, como el PR-AUC). 
*   **Transparencia:** El algoritmo evalúa dinámicamente las variables de mayor impacto (como el ratio deuda/ingreso), asegurando que cada decisión sea auditable y explicable para los analistas de operaciones.

## 3. Recomendación de Política de Aprobación e Impacto Financiero
Al cruzar las probabilidades predictivas del modelo con la economía del producto (ganancia por intereses vs. pérdida del 55% del capital por default), se simuló el impacto de múltiples escenarios.

*   **Política Sugerida:** Se recomienda fijar el **umbral de aprobación en un riesgo máximo tolerado del XX%** *(nota: revisa el gráfico del notebook 02 e ingresa el número exacto del umbral óptimo aquí)*. 
*   **Impacto Económico:** Al aplicar este umbral sobre el conjunto de validación, el modelo maximiza la ganancia neta, demostrando que rechazar estratégicamente las colocaciones más tóxicas genera un incremento sustancial en la rentabilidad comparado con la política de aprobar la totalidad de la cartera.

## 4. Limitaciones y Próximos Pasos (Trabajo Colaborativo)
*   **Monitoreo Continuo:** Las condiciones macroeconómicas varían. Se recomienda establecer una mesa de trabajo conjunta con Ingeniería de Datos para recalibrar el modelo trimestralmente.
*   **Nuevas Fuentes de Datos:** Para optimizar la evaluación de los segmentos "informal" e "independiente", sugiero explorar en el futuro la integración de datos transaccionales u open banking que complementen el score de buró tradicional.