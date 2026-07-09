# Desafío Técnico — Data Scientist Senior (Solución)

Este repositorio contiene la solución al desafío de predicción de default para Andina Crédito. El proyecto fue estructurado enfocando el desarrollo técnico como una herramienta colaborativa para apoyar la toma de decisiones del equipo de Riesgo.

## Estructura del Proyecto

- `01_Exploracion_y_Preparacion.ipynb`: Análisis exploratorio de datos, tratamiento de anomalías (errores de digitación en ingresos y nulos estructurales), ingeniería de características y partición Out-of-Time.
- `02_Modelado_y_Evaluacion.ipynb`: Entrenamiento de LightGBM, evaluación Out-of-Time, validación crítica de la performance obtenida, optimización económica de la política de aprobación y generación del scoreo final.
- `informe_ejecutivo.md`: Resumen de hallazgos, performance del modelo y recomendación de negocio, orientado a la Gerencia de Riesgo.
- `AI_USAGE.md`: Declaración de uso de herramientas de IA y correcciones aplicadas.
- `diccionario_datos.md`: Descripción de las variables del dataset (provisto por el desafío).
- `predictions.csv`: Archivo final con el scoreo de las 12.000 solicitudes de test (`id_solicitud,prob_default`).
- `data/`: Contiene `train.csv` y `test.csv` originales, y las matrices intermedias que generan los notebooks (`X_train_prep.csv`, `X_val_prep.csv`, `test_prep.csv`, `ids_val.csv`, etc.).

## Instrucciones para reproducir el código

1. Clonar este repositorio:

```bash
git clone https://github.com/ceciliamoraga77-coder/DataScientistTest.git
cd DataScientistTest
```

2. Instalar las dependencias requeridas (Python 3.10+):

```bash
pip install -r requirements.txt
```

3. Ejecutar el pipeline en orden, de principio a fin (Kernel → Restart & Run All en cada uno):
   - `01_Exploracion_y_Preparacion.ipynb`: genera las matrices preprocesadas en `data/` (`X_train_prep.csv`, `y_train_prep.csv`, `X_val_prep.csv`, `y_val_prep.csv`, `ids_val.csv`, `test_prep.csv`).
   - `02_Modelado_y_Evaluacion.ipynb`: entrena el modelo, valida su performance, define la política de aprobación y regenera `predictions.csv` en la raíz del proyecto.

## Notas de reproducibilidad

- La semilla aleatoria del modelo está fijada (`random_state=42`) para que el entrenamiento sea determinístico entre corridas.
- La validación usa una partición temporal (*Out-of-Time*, corte en noviembre de 2024) en vez de una partición aleatoria, para simular cómo se comportaría el modelo sobre solicitudes futuras.
- El notebook `02` incluye una sección de validación crítica de la performance obtenida (AUC/PR-AUC atípicamente altos para el estándar de la industria); revisar esa sección antes de dar por definitivas las métricas reportadas en el informe ejecutivo.
