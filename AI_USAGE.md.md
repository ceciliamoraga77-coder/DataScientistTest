# Declaración de Uso de IA

Durante el desarrollo de este desafío, utilicé Inteligencia Artificial (Gemini) como asistente para agilizar la escritura de código, generación de gráficos y estructuración de la documentación. Sin embargo, el análisis del negocio, la estrategia de validación y la toma de decisiones metodológicas fueron dirigidas y validadas por mí.

## Correcciones a outputs incorrectos o subóptimos de la IA:

1.  **Error de sintaxis y alucinación de librerías:** Durante la generación del script de carga de datos inicial, la IA cometió un error de sintaxis escribiendo `pd.pd.read_csv()`, lo cual rompió el pipeline. Tuve que intervenir, hacer el *debugging* del error y corregir la llamada a la librería Pandas.
2.  **Manejo de rutas relativas:** La IA sugirió cargar los archivos asumiendo que estaban en el directorio raíz (`train.csv`). Sin embargo, al revisar la estructura del repositorio cloneado, identifiqué que los datos estaban dentro de un subdirectorio. Tuve que corregir manualmente las rutas a `data/train.csv` y `data/test.csv` para asegurar la reproducibilidad del código.
3.  **Profundidad del Análisis Exploratorio (EDA):** Inicialmente, la IA generó el código de limpieza de datos pero omitió la documentación y justificación de por qué se tomaban esas decisiones (ej. tratamiento de nulos en jubilados). Tuve que solicitar explícitamente y guiar a la herramienta para que incluyera celdas de Markdown formales que tradujeran los hallazgos técnicos en *insights* de negocio auditables.

## Elementos validados manualmente:
*   Se validó de forma estricta que la lógica de cálculo económico del producto respetara la fórmula entregada (LGD del 55% y cálculo de intereses) antes de confiar en el bucle de optimización del umbral.
*   Se verificó que la partición *Out-of-Time* (noviembre 2024) respetara la línea temporal y no generara filtración de datos (*data leakage*) hacia el conjunto de entrenamiento.