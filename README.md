# Predicción de Enfermedades Cardiovasculares Utilizando Algoritmos de Gradient Boosting
El presente repositorio contiene la investigación y [código](Proyecto1.ipynb) realizados para la creación de modelos predictivos de enfermedades cardiovasculares basados en información extraída de la base de datos ["Cardiovascular Disease dataset"](https://www.kaggle.com/sulianova/cardiovascular-disease-dataset). Dichos modelos fueron creados utilizando la ayuda de libreras como [CatBoost](https://catboost.ai/) y [XGBoost](https://xgboost.ai/).
## Paso 1: Instalación de los paquetes utilizados
En esta sección se describen los paquetes utilizados para la creación del experimento, para una lista acortada de los requerimientos, favor de ver [éste archivo](requirements.txt).

El archivo contiene las dependencias y versiones especificas usadas para desarrollar el proyecto. Puede instalarse con:
> `pip install -r requirements.txt`

El ambiente de programación utilizado fue [Google Collab](https://colab.research.google.com/), un servicio en línea para crear y hostear notebooks. Otras alternativas a esta plataforma incluyen [Kaggle](https://www.kaggle.com/), así como [Jupyter Notebook y JupyterLab](https://jupyter.org/install). El lenguaje de programación que se utilizó es [Python](https://www.python.org/downloads/).

Las librerías de Python utilizadas son las siguientes:
<ul>
  <li><a href="https://numpy.org/install/">Numpy</a>: Una librería de Python que permite la creación de arreglos multidimensionales y operaciones con arreglos.</li>
  <li><a href="https://pandas.pydata.org/docs/getting_started/index.html">Pandas</a>: Una librería de Python para la creación, manipulación y análisis de datos tabulares.</li>
  <li><a href="https://matplotlib.org/stable/users/installing.html">Matplotlib</a>: Una librería graficadora para Python.</li>
  <li><a href="https://plotly.com/python/getting-started/">Plotly</a>: Una librería graficadora multiplataforma.</li>
  <li><a href="https://seaborn.pydata.org/installing.html">Seaborn</a>: Una librería para hacer gráficas estadísticas en Python.</li>
  <li><a href="https://scikit-learn.org/stable/install.html">Scikit-learn</a>: Kit de herramientas de análisis predictivo.</li>
  <li><a href="https://catboost.ai/en/docs/concepts/python-installation">CatBoost</a>: Algoritmo de Gradient Boosting para árboles de decisión con apoyo a variables categóricas.</li>
  <li><a href="https://xgboost.readthedocs.io/en/latest/install.html">XGBoost</a>: Librería optimizada de Gradient Boosting multiplataforma.</li>
</ul>

## Paso 2: Preprocesamiento de datos

Para el preprocesamiento de los datos se crearon dos variables nuevas, a manera de columnas: 
  * above_55: El análisis exploratorio mostró que existe una mayor incidencia de enfermedades cardíacas en personas con más de 55 años. En consecuente se creo una variable binaria que mostraba si la persona tiene más de 55 años (20075 días) con la finalidad de extender el conjunto de datos.
  * New_BMI: Cálculo del índice de masa corporal con base a la propuesta [New BMI](https://people.maths.ox.ac.uk/trefethen/bmi.html) por el profesor Nick Trefethen.

Debido a la alta correlación de above_55 con 'age' se removió la edad en el set X para evitar redundancia y reducir la complejidad del algoritmo. 

## Paso 3: Catboost

[Catboost](https://catboost.ai/) es un algoritmo que usa gradient boosting en árboles de decisión. Para este proyecto se aplicó un clasificador de la librería (CatboostClassifier) con la finalidad de predecir la variable dependiente del dataset (cardio). Para ello se realizaron los siguientes pasos:
   1. Definición de las variables categoricas: Catboost permite aplicar un proceso de conversión para las variables categoricas conocido como Ordered Target Statistics. Para realizar este proceso primero se deben especificar los nombres de columnas a usar, en este caso: 'gender', 'cholesterol', 'gluc'. 
   2. Para la selección de hiperparámetros se tienen ejecutaron iteraciones del modelo dejando que este usara los hiperparámetros por default y después se realizó un grid search para la búsqueda del mejor resultado, probando el accuracy para los parámetros de profundidad, regularización l2, learning rate y el número de iteraciones. 
   3. 

## Paso 4: XGBoost

## Paso 5: Resultados


