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


## Paso 4: XGBoost
[XGBoost](https://xgboost.readthedocs.io/en/latest/) XGBoost es una biblioteca optimizada de aumento de gradiente distribuida diseñada para ser altamente eficiente, flexible y portátil. Implementa algoritmos de aprendizaje automático bajo el marco Gradient Boosting. XGBoost proporciona un refuerzo de árbol paralelo (también conocido como GBDT, GBM) que resuelve muchos problemas de ciencia de datos de una manera rápida y precisa. Para este proyecto se utilizó la librería XGBClassifier(). Con ello se realizaron los siguientes pasos:
  1. Definición de las variables categóricas así como la variable objetivo, así como en el apartado de Catboost se realizó one-hot-encoding a las variable 'gender' en base a las variables 'cholesterol' y 'gluc'.
  2. Para la selección de los parámetros del modelo de max_depth y eta se realizaron experiementaciones teniendo un **max_depth** de 6, sin embargo si el valor es muy alto se puede generar un árbol muy complejo que te llevará al overfitting y un consumo muy grande de memoria. Al usar un valor menor el **accuracy desminuía** 
Para el **learning rate** si el eta es más grande hace que el cálculo sea más rápido (porque necesita introducir menos rondas) pero no hace que se alcance el mejor óptimo. Por otro lado, al disminuir el valor de eta hace que el cálculo sea más lento (porque hay que introducir más rondas) pero facilita alcanzar el mejor óptimo. tras experimentaciones con valores como 0.1, 0.001, 0.02, 0.3, 0.03  se percató que el que mejor accuracy nos daba era el 0.03. Se utilizó como objetivo "multi softmax" debido a que es utilizada para problemas de clasificación.

```json
parameters = {
    'max_depth':6,
    'eta':0.03,
    'objective':'multi:softmax',
    'num_class':2, ## numero de clases
}

epochs = 15 ## número de iteraciones
```

## Paso 5: Resultados
Por parte de XGBoost se puede concluir que los resultados obtenidos en base a los parametros descritos en el apartado de XGBoost, se tuvo un accuracy de 73.01%, a pesar de que realizó un one-hot-encoding para mejorar este dato

