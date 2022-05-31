**PROYECTO FINAL INTELIGENCIA ARTIFICIAL**

**INTEGRANTES DEL GRUPO** :

- Juan David Velásquez
- Nathalia Rivadeneira

**INTRODUCCIÓN**

El rendimiento de una aeronave es crucial para evaluar la disponibilidad de una misión, también para comparar aeronaves y decidir cuál es más adecuada para una misión determinada

Las aeronaves deben poder operar de manera segura durante todo su régimen de vuelo de tal manera que se obtenga un resultado seguro a partir de fallas específicas que ocurran en cualquier punto a lo largo del rango de vuelo.

**Dataset**

El conjunto de datos seleccionado contiene 861 aviones y sus características, como velocidad máxima, velocidad de crucero, alcance, etc. Para el análisis de datos el dataset cuenta con muchos valores faltantes, diferentes formatos de datos y unidades para las mismas características.

**Características:**

Modelo: Nombre del avión.

Empresa: Nombre de la empresa.

Tipo de motor: Tipo de motor utilizado en el avión.

Motor HP o lbs thr ea: Potencia del eje o empuje para la atmósfera estándar internacional (ISA). Unidades: HP o lbf.

Velocidad máxima Nudos: Velocidad máxima del avión. Unidades: Nudo o Mach.

Rcmnd cruise Nudos: Alta velocidad de crucero del avión. Unidades: Nudo.

Stall Knots sucio: velocidad de pérdida del avión en configuración &quot;sucia&quot; (flaps hacia afuera, tren hacia abajo, etc.). Unidades: Nudo.

Fuel gal/lbs: Capacidad de combustible del avión. Unidades: gal o lb.

Techo de servicio de todos los motores: densidad-altitud máxima del avión con todos los motores en funcionamiento. Unidades: ft (como densidad-altitud).

Techo de servicio de salida: densidad-altitud máxima del avión con un motor en funcionamiento. Unidades: ft (como densidad-altitud).

Tasa de ascenso de todos los motores: Tasa de ascenso del avión con todos los motores en funcionamiento. Unidades: pies/min.

Tasa de ascenso Eng out: Tasa de ascenso del avión con un motor en funcionamiento. Unidades: pies/min.

Despegue sobre 50 pies: la velocidad de ascenso del avión durante el despegue normal para un obstáculo de 50 pies. Unidades: pies/min.

Carrera de despegue: carrera de despegue del avión. Unidades: pies

Despegue sobre 50 pies: velocidad de descenso del avión durante un aterrizaje normal para un obstáculo de 50 pies. Unidades: pies/min.

Carrera de despegue: carrera de aterrizaje del avión. Unidades: pies

Carrera de despegue: carrera de aterrizaje del avión. Unidades: pies

Libras de peso bruto: peso bruto del avión (también conocido como peso total). Unidades: libras

Libras de peso en vacío: peso bruto del avión (también conocido como peso en vacío del fabricante). Unidades: libras

Longitud ft/in: longitud del avión. Unidades: pies + pulgadas.

Altura ft/in: altura del avión. Unidades: pies + pulgadas.

Envergadura ft/in: Envergadura del avión. Unidades: pies + pulgadas.

**DESARROLLO**

Inicialmente se comienza con la limpieza de los datos, en este caso, descartando algunas columnas como el modelo del avión (referencia del avión), la compañía fabricante, altura máxima con un motor, velocidad en pérdida con condiciones deplorables, entre otras características no relevantes para la clasificación (o predicción) del tipo de motor de cierto avión dependiendo de sus especificaciones de vuelo y adicionalmente, con las columnas restantes se elimina por muestra cuales contienen datos no numéricos.

Tras realizar la limpieza general de los datos, se convierten las clases o etiquetas en valores numéricos que en este caso es así:

- Motor tipo Pistón: Le corresponde clase 0.
- Motor tipo Propjet (Turbohélice): Asociada con clase 1.
- Motor tipo Jet (Propulsión a chorro): Etiquetada como clase 2.

Una vez realizada la conversión de las etiquetas a valores numéricos entre 0 a 2, se guarda el dataset limpio denominado como Aircraft\_Handbook.csv.

Por consideración del grupo, se dividió el dataset en 80% como entrenamiento y 20 % de validación, tomando como base que el 71% de los datos son con motor de Pistón, 18% como motor tipo Jet y Propjet como 11% de los datos.

En la sección de análisis por componentes principales PCA, se utilizó un método diferente para encontrar la reducción dimensional más apropiada en la solución del problema. En este caso, se implementó un ciclo tal que fuera reduciendo dimensionalmente las características hasta encontrar la mínima tal que no baje del 99.7% de la suma de la varianza explicada.

Finalmente, en la sección de entrenamiento y pruebas de los clasificadores, se puso en comparación 5 clasificadores diferentes, donde se empleó el método de Búsqueda por rejilla mediante validación cruzada (GridSearch CV), para así, encontrar el mejor clasificador entre una variedad de hiperparámetros dados. Por otro lado, en la evaluación de las métricas, se utilizaron evaluadores tales como el accuracy, F1 – Score, Recall y el coeficiente de correlación de Matthews (siendo este último el de mayor confianza para determinar la viabilidad y exactitud de los clasificadores), tales que estos resultados se muestran en la sección final del código de cada clasificador mostrados en un Dataframe de Pandas organizado.

**REGRESIÓN LOGÍSTICA:** En este clasificador se variaron hiperparámetros como la norma de la penalidad y el parámetro de regularización entre 10 diferentes valores, para esto, siguiendo una escala logarítmica tal que permita tomar una variedad suficiente de parámetros para evaluar en la clasificación.

**MÁQUINAS DE SOPORTE VECTORIAL (SVM):** Para la búsqueda de mejores hiperparámetros se iteraron 3 funciones de Kernel diferentes, los cuales para solucionescon una alta dimensionalidad en las características suelen dar resultados óptimos. Adicionalmente, con el mismo objetivo, se tomaron los mismos parámetros de regularización de la regresión logística y finalmente, se consideró iterar entre un clasificador de tipo OvA u OvO, con el fin de evaluar posibilidades y opciones que ofrece el clasificador, a pesar de que, si bien se sabe, las clases están altamente desequilibradas (más del 70% de los datos alojados en una clase).

**KNN – K Vecinos más cercanos:** Para este clasificador se iteraron el número de vecinos a evaluar, el tamaño de hoja (leaf size), el parámetro de la potencia para la métrica de Minkowski, el tipo de peso por medida de distancias (uniforme o ya sea distancia) y finalmente el tipo de métrica de distancia.

**Naive - Bayes Gaussiano:** Sabiendo que este clasificador no es paramétrico, simplemente se evalúa su rendimiento con sus configuraciones predeterminadas.

**Redes Neuronales (Perceptrón Multicapa):** Este clasificador, que para el desarrollo fue la técnica más sofisticada y la que más toma tiempo de iterar por el método de búsqueda por rejilla en validación cruzada, se barrieron parámetros tales como el tamaño de las capas ocultas, las funciones activación, el solucionador o &quot;solver&quot;, el parámetro alfa de regularización por L2 y finalmente la tasa de aprendizaje para actualizaciones en los pesos de las funciones de la red neural.

**RESULTADOS**

Se realizaron pruebas con 5 diferentes clasificadores utilizando el método gridsearch cv

- Regresión logística

![](https://github.com/NathaliaRivadeneira/Proyecto-inteligencia-artificial/blob/main/Imagenes/regresion%20logistica%20p.PNG)

- Máquinas de soporte vectorial (SVM)

![](https://github.com/NathaliaRivadeneira/Proyecto-inteligencia-artificial/blob/main/Imagenes/maquinas%20de%20sop%20vec%20p.PNG)

- KNN – K vecinos más cercanos

![](https://github.com/NathaliaRivadeneira/Proyecto-inteligencia-artificial/blob/main/Imagenes/KNN%20P.PNG)

- Naive bayes gausiano

![](https://github.com/NathaliaRivadeneira/Proyecto-inteligencia-artificial/blob/main/Imagenes/naive%20bayes%20p.PNG)

- Redes neuronales

![](https://github.com/NathaliaRivadeneira/Proyecto-inteligencia-artificial/blob/main/Imagenes/regresion%20logistica%20p.PNG)

**CONCLUSIONES**

- Entre los cinco clasificadores puestos a prueba, los dos que mejor resultado obtuvieron fueron las máquinas de soporte vectorial (SVM) y las redes neuronales, con un valor de coeficiente de correlación de Matthews dede 0.9341 y 0.9352 respectivamente. En este caso, ambos clasificadores obtuvieron una calificación relativamente alta con el dataset de pruebas con un valor muy cercano entre sí, por lo que, ambas técnicas pueden ser opciones apropiadas para dar solución al problema de clasificación.
- En general, es preciso resaltar que los clasificadores utilizados para llevar a cabo la tarea de predicción (o clasificación) entre tres diferentes tipos de motor de aeronave, tuvieron un puntaje superior a 86 %, siendo entonces el que menor puntaje obtuvo fue el clasificador de Naive Bayes gaussiano, que a pesar de que su calificación es de un 87.1 % sigue siendo un método viable para el análisis del problema en cuestión, puesto que está a una diferencia de alrededor de un 6% con los mejores clasificadores experimentados en el desarrollo del proyecto.
- Durante el desarrollo de la fase de análisis por componentes principales, a pesar de que se hizo una reducción dimensional de 16 a 14 características, al momento de reducirla a 13 o inferior a esta, la suma de la varianza explicada se reducía a menos del 90%, por tanto, con esto, es preciso afirmar que el dataset en su mayoría cuenta con características mayormente independientes, tales que una gran parte describen más del 95% de la información del dataset en cuestión, por tanto, solo fue posible reducir 2 dimensiones del total de las características entregadas sin afectar en mayor medida la pérdida de datos o información que será utilizada para el proceso de clasificación en las diferentes técnicas implementadas.
- A pesar de que las redes neuronales implementadas en el desarrollo del proyecto (MLPC de la librería de scikit) obtuvieron el mejor resultado, en efecto, son las que más tardan en finalizar el proceso de búsqueda por rejilla a través de validación cruzada para el mejor clasificador, tomando más de 3 minutos en procesar, adicionalmente, el tiempo que tarda en entrenar es de 248.57 s, lo que quiere decir que es un método computacionalmente exigente y lento para utilizar en aplicaciones con máquinas de hardware limitado, por tanto, debe requerirse pruebas previas para evaluar su viabilidad en proyectos con estas características.