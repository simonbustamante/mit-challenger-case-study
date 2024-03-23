# mit-challenger-case-study
Datos
A continuación, se muestran todos los datos de las observaciones que conforman la base de datos con la que se desarrollará el caso de estudio. Los datos se distribuyen en tres columnas:

Observación: es sencillamente el número de la observación.
Y: es la etiqueta de fallo/éxito para cada observación. Si hubo fallo: Y=1. En el resto de casos: Y=0.
X: es la temperatura (en grados Fahrenheit) en el momento del lanzamiento.
Observation	Y	X		Observation	Y	X		Observation	Y	X
1	1	53		41	0	68		81	1	75
2	1	53		42	0	68		82	1	75
3	1	53		43	0	68		83	0	75
4	0	53		44	0	68		84	0	75
5	0	53		45	0	68		85	0	75
6	1	57		46	0	69		86	0	75
7	0	57		47	0	69		87	0	75
8	0	57		48	0	69		88	0	75
9	0	57		49	0	69		89	0	75
10	0	57		50	0	69		90	0	75
11	1	58		51	1	70		91	0	76
12	0	58		52	0	70		92	0	76
13	0	58		53	0	70		93	0	76
14	0	58		54	0	70		94	0	76
15	0	58		55	0	70		95	0	76
16	1	63		56	1	70		96	0	76
17	0	63		57	0	70		97	0	76
18	0	63		58	0	70		98	0	76
19	0	63		59	0	70		99	0	76
20	0	63		60	0	70		100	0	76
21	0	66		61	0	70		101	0	78
22	0	66		62	0	70		102	0	78
23	0	66		63	0	70		103	0	78
24	0	66		64	0	70		104	0	78
25	0	66		65	0	70		105	0	78
26	0	67		66	0	70		106	0	79
27	0	67		67	0	70		107	0	79
28	0	67		68	0	70		108	0	79
29	0	67		69	0	70		109	0	79
30	0	67		70	0	70		110	0	79
31	0	67		71	0	72		111	0	80
32	0	67		72	0	72		112	0	80
33	0	67		73	0	72		113	0	80
34	0	67		74	0	72		114	0	80
35	0	67		75	0	72		115	0	80
36	0	67		76	0	73		116	0	81
37	0	67		77	0	73		117	0	81
38	0	67		78	0	73		118	0	81
39	0	67		79	0	73		119	0	81
40	0	67		80	0	73		120	0	81
En el notebook de Google Colab asociado a este caso de estudio podrá encontrar la información de la tabla en el archivo challenger-data.csv. Para poder acceder a dicho información e importarla a nuestro entorno de ejecución simplemente se introducen los siguientes comandos en Python:

import pandas as pd
import numpy as np 

data = pd.read_csv("challenger-data.csv") 
Ahora ya puede acceder fácilmente a los datos para el análisis posterior.

Visualización de los datos
Con la estructura de datos, es posible trazar los datos en un gráfico. Por ejemplo, si desea trazar la frecuencia de los fracasos a cada temperatura en un gráfico, se puede realizar del modo siguiente. Se utilizará la biblioteca matplotlib para generar los gráficos. Asegúrese de que esté instalada antes de proceder.

# hacer subconjuntos de datos
failures = data.loc[(data.Y == 1)]
no_failures = data.loc[(data.Y == 0)] 

# frequencias
failures_freq = failures.X.value_counts()
no_failures_freq = no_failures.X.value_counts()

# mostrar gráficos
import matplotlib as mpl  
from matplotlib import pyplot as plt 
plt.scatter(failures_freq.index, failures_freq, c='red', s=40) 
plt.scatter(no_failures_freq.index, 
np.zeros(len(no_failures_freq)), c='blue', s=40)  
plt.xlabel('X: Temperature')  
plt.ylabel('Number of Failures')  
plt.show()
Regresión logística
Ya está listo para utilizar la regresión logística. Deberá tener las siguientes bibliotecas instaladas antes de proceder:

Patsy
Statsmodels
Una vez instaladas, puede usar el fragmento de código siguiente para entrenar el modelo y obtener los resultados:

from patsy import dmatrices
import statsmodels.discrete.discrete_model as sm

# obtenga los datos en el formato correcto
y, X = dmatrices('Y ~ X', data, return_type = 'dataframe')

# ejecute el modelo
logit = sm.Logit(y, X)
result = logit.fit() 

# obtenga un resumen de los resultados el modelo
print result.summary() 
Ahora ya tiene el modelo, y los resultados deberían proporcionar el coeficiente estimado (columna coef, fila X), la constante (columna coef, fila intercept), los errores estándar (std err), los valores-p, y los intervalos de confianza del 95%:

Optimization terminated successfully.
	Current function value: 0.242411
	Iterations 7 

                            Logit Regression Results
============================================================================== 
Dep. Variable:                       Y   No. Observations:                 120
Model:                           Logit   Df Residuals:                     118
Method:                            MLE   Df Model:                           1
Date:                 Fri, 20 Nov 2020   Pseudo R-squ.:                 0.1549 
Time:                         17:13:40   Log-Likelihood:               -29.089 
converged:                        True   LL-Null:                      -34.420 
Covariance Type:             nonrobust   LLR p-value:                 0.001094
==============================================================================
                 coef     std err          z      p>|z|     [0.025      0.975]
------------------------------------------------------------------------------
Intercept      7.4049       3.041      2.435      0.015      1.445      13.365 
X             -0.1466       0.047     -3.104      0.002     -0.239      -0.054
==============================================================================
