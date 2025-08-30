# responsibleAI-proyecto01

## Integrantes

- Brian Carrillo
- Carlos López
- Josué Morales
- Arturo Argueta
- Diego Alonzo

## Objetivo del proyecto

Construir un modelo base para predecir ingresos (>50K) con el dataset Adult Census (UCI) y evaluar rendimiento global y segmentado por grupos sensibles (sexo, edad, raza, país), analizando brechas de métricas y aplicando una mitigación simple (class_weight) para estudiar trade-offs de desempeño–equidad.

## Estructura

- `project-eda.ipynb`: análisis exploratorio de datos (distribuciones, correlaciones, sesgos iniciales).
- `baseModel.ipynb`: preprocesamiento, entrenamiento (Regresión Logística y Árbol), métricas globales, evaluación segmentada y comparación sin/con `class_weight="balanced"`.
- `requirements.txt`: librerías necesarias para ejecutar los notebooks.
- `img/`: imágenes usadas en el EDA.

## Requisitos

- Windows 10/11, PowerShell (v5+)
- Python 3.10–3.12 recomendado
- pip actualizado

Nota: los notebooks usan `kagglehub` para descargar automáticamente el dataset Adult (UCI). No se requiere configurar credenciales de Kaggle para este dataset público.

## Configuración rápida (Windows PowerShell)

1) Clonar el repositorio

```powershell
git clone https://github.com/carrillo21108/responsibleAI-proyecto01.git
cd responsibleAI-proyecto01
```

2) Crear y activar entorno virtual

```powershell
python -m venv .venv
.\.venv\Scripts\Activate
```

3) Instalar dependencias

```powershell
pip install --upgrade pip
pip install -r requirements.txt
# Opcional (si el EDA lo requiere):
pip install seaborn
```

## Cómo ejecutar

Opción A — VS Code

1) Abrir la carpeta del proyecto en VS Code.
2) Seleccionar el kernel de Python del entorno `.venv` para los notebooks.
3) Abrir `project-eda.ipynb` y/o `baseModel.ipynb` y ejecutar “Run All”.

Opción B — Jupyter Notebook/Lab

```powershell
jupyter notebook
# o
jupyter lab
```

Abrir `project-eda.ipynb` y/o `baseModel.ipynb` y ejecutar las celdas en orden.

## Datos

Por defecto, `baseModel.ipynb` descarga el dataset Adult con:

- `kagglehub.dataset_download("uciml/adult-census-income")`
- Lee `adult.csv` desde la ruta descargada.

Si ya tienes un CSV local, puedes indicar su ruta con la variable de entorno `ADULT_CSV` antes de ejecutar el notebook:

```powershell
$env:ADULT_CSV = "C:\\ruta\\a\\tu\\adult.csv"
```

El notebook detecta columnas numéricas/categóricas comunes del dataset Adult y mapea la etiqueta objetivo a binario (<=50K vs >50K).

## Resultados esperados

- EDA: tablas/resúmenes de datos, gráficos de distribución y correlación; notas sobre sesgos iniciales (p. ej., desbalance por sexo, edad, raza, país).
- Modelo base: métricas globales (accuracy, F1, ROC AUC), matriz de confusión y reporte por clase.
- Evaluación segmentada: tablas por `sex`, `age_bin`, `race` (y top-5 `native_country` si aplica) con `accuracy`, `f1`, `tpr`, `fpr`, `positive_rate`, más brechas (max–min).
- Mitigación: comparación sin/con `class_weight="balanced"` destacando trade-offs (↑recall/TPR, ↑FPR, variación de positive rate).

## Troubleshooting

- Error al descargar datos con `kagglehub`: reintenta ejecutar la celda; verifica conexión/proxy. Como alternativa, descarga manualmente el CSV desde UCI/Kaggle y define `ADULT_CSV`.
- Incompatibilidad de versiones: usa Python 3.10–3.12 y reinstala dependencias en un entorno limpio.
- Kernel no detecta `.venv`: en VS Code, selecciona el intérprete de `.venv` (Command Palette → Python: Select Interpreter).

## Reproducibilidad y notas

- Semillas: `train_test_split(..., random_state=42)` para partición reproducible.
- Pipelines: uso de `ColumnTransformer` + `OneHotEncoder(handle_unknown="ignore")` para robustez ante categorías nuevas.
- Umbral: los modelos clasifican con umbral 0.5; ajustar umbral puede cambiar significativamente TPR/FPR y brechas.
