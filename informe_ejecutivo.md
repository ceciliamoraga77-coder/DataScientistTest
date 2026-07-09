# Informe Ejecutivo: Modelo de Probabilidad de Default

**Para:** Gerencia de Riesgo, Andina Crédito 
**Fecha:** Julio 2026

## 1. Contexto y Hallazgos en la Información Histórica

Durante la auditoría inicial de las 45.300 solicitudes históricas, se identificaron y corrigieron patrones que afectaban la calidad de la información:

- **Anomalía en Ingresos Declarados:** aproximadamente un 10% de las solicitudes presentaban ingresos irrisorios (menores a $10.000 CLP). Se determinó que correspondía a un error sistemático de digitación (montos ingresados en miles). Se aplicó un factor de corrección para no distorsionar la evaluación de la carga financiera del cliente.
- **Ausencia de Antigüedad Laboral:** se detectó que los valores faltantes en esta variable correspondían en su totalidad a trabajadores informales y jubilados. En lugar de imputar promedios que sesgarían el análisis, se aislaron como un segmento independiente (valor constante -1), permitiendo que el modelo capture ese patrón sin distorsionar la media de los demás segmentos.
- **Desbalance de clases:** la tasa base de incumplimiento observada es de ~9,9% (default) versus ~90,1% (al día), consistente con lo esperable en una cartera de consumo. Esto llevó a priorizar métricas robustas al desbalance (PR-AUC) por sobre el Accuracy.

## 2. Enfoque Metodológico y Rendimiento Esperado

Se entrenó un modelo predictivo (LightGBM) como una herramienta de apoyo técnico-colaborativo para la toma de decisiones. Se utilizaron las solicitudes desembolsadas entre 2024-01 y 2024-10, reservando las desembolsadas entre 2024-11 y 2025-02 como conjunto de validación *Out-of-Time (OOT)*. Se eligió este esquema temporal porque el modelo se usará sobre solicitudes futuras; una validación aleatoria tradicional mezclaría información de distintos períodos y entregaría una estimación engañosa.

- **Performance en validación OOT:** ROC-AUC = 0,97, PR-AUC = 0,91.
- **Variables más relevantes:** `score_buro`, `num_contactos_ult_trimestre`, `uso_linea_credito_pct`, `deuda_sistema`, `monto_solicitado`.

**Nota de honestidad sobre la performance:** un ROC-AUC de 0,97 está muy por sobre el rango típico de la industria para modelos de riesgo de consumo (0,70–0,85 con datos reales), lo que motivó una investigación específica. Al auditar las variables más predictivas contra el Diccionario de Datos, **se confirmó una fuga de información (*data leakage*)**. El diccionario estipula que los datos corresponden a un *"snapshot a la fecha de extracción"*. Esto significa que variables como `num_contactos_ult_trimestre` están contabilizando interacciones recientes (ej. gestiones de cobranza de créditos que ya están en mora), en lugar de la información que existía al momento de la solicitud. Para una implementación productiva, el modelo deberá ser reentrenado excluyendo estas variables de comportamiento post-desembolso, lo cual sincerará el AUC a un rango estándar de mercado.

## 3. Recomendación de Política de Aprobación e Impacto Financiero

Se simuló la ganancia neta de la cartera para distintos umbrales de probabilidad de default sobre el conjunto de validación (11.671 solicitudes), usando la economía del producto entregada (ganancia = monto × 12% anual × plazo/12 × 0,5 si el cliente paga; pérdida = monto × 55% si cae en default).

- **Umbral recomendado:** aprobar solicitudes con probabilidad de default estimada por debajo de **40%**. Este umbral maximiza la ganancia neta esperada sobre el conjunto de validación.
- **Tasa de rechazo esperada:** **16,8%** de las solicitudes.
- **Impacto económico** (medido sobre el conjunto de validación OOT):
  - Ganancia aprobando el 100% de las solicitudes (política actual): **$337.138.700 CLP**.
  - Ganancia con la política optimizada (umbral 40%): **$1.600.875.400 CLP**.
  - **Incremento: +$1.263.736.700 CLP (+375% respecto a aprobar todo).**

*Nota: Estas cifras están calculadas sobre el set de validación; deben escalarse proporcionalmente al volumen mensual real de solicitudes de Andina Crédito para dimensionar el impacto anualizado.*

## 4. Limitaciones y Próximos Pasos

- **Reconstrucción de la Base de Datos:** Dado el hallazgo de la sección 2, es imperativo establecer una mesa de trabajo conjunta con Ingeniería de Datos para reconstruir el dataset utilizando una lógica "as-of" (fotografía exacta al día de la solicitud de cada cliente), eliminando así la fuga de información.
- **Monitoreo continuo:** las condiciones macroeconómicas varían; se recomienda recalibrar el modelo trimestralmente en conjunto con el equipo técnico.
- **Nuevas fuentes de datos:** para optimizar la evaluación de los segmentos "informal" e "independiente", se sugiere explorar a futuro la integración de datos transaccionales u *open banking* que complementen el score de buró tradicional.
- **Explicabilidad:** el modelo actual usa importancia global de variables como proxy de interpretabilidad; para una implementación productiva se recomienda incorporar herramientas como SHAP para auditar localmente cada solicitud.