# Desafío Técnico — Data Scientist Senior (Solución)

Este repositorio contiene la solución al desafío de predicción de default para Andina Crédito. El proyecto fue estructurado enfocando el desarrollo técnico como una herramienta colaborativa para apoyar la toma de decisiones del equipo de Riesgo.

## Estructura del Proyecto

*   `01_Exploracion_y_Preparacion.ipynb`: Análisis exploratorio de datos, tratamiento de anomalías (errores de digitación en ingresos y nulos estructurales) e ingeniería de características.
*   `02_Modelado_y_Evaluacion.ipynb`: Entrenamiento de LightGBM, evaluación Out-of-Time y optimización económica de la política de aprobación.
*   `informe_ejecutivo.md`: Resumen de hallazgos y recomendación de negocio.
*   `AI_USAGE.md`: Declaración de uso de herramientas de IA y correcciones aplicadas.
*   `predictions.csv`: Archivo final con el scoreo de las 12.000 solicitudes.

## Instrucciones para reproducir el código

1.  Clonar este repositorio:
    ```bash
    git clone [https://github.com/ceciliamoraga77-coder/DataScientistTest.git](https://github.com/ceciliamoraga77-coder/DataScientistTest.git)
    cd DataScientistTest
    ```
2.  Instalar las dependencias requeridas (Python 3.10+):
    ```bash
    pip install -r requirements.txt
    ```
3.  Ejecutar el pipeline en orden:
    *   Ejecutar todas las celdas de `01_Exploracion_y_Preparacion.ipynb` (esto generará las matrices limpias en la carpeta `/data`).
    *   Ejecutar todas las celdas de `02_Modelado_y_Evaluacion.ipynb` (esto entrenará el modelo y regenerará el archivo `predictions.csv` en la raíz).