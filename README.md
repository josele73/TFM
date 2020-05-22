# TFM -- Control de cambios en imágenes aéreas segmentadas

El objetivo de este proyecto es el control de cambios en imágenes segmentadas obtenidas a partir del análisis de las construcciones en las imágenes áreas del PNOA mediante redes neuronales con imágenes de años anteriores con el fin de localizar cambios. Estos cambios pueden ser:  Nuevas construcciones, construcciones que ya no existan y grandes ampliaciones.

El proyecto tienes tres fases:

![Ciclo fases](/img/esquema.jpg)


#  Obtención de imágenes PNOA 

Inicialmente se debe trabajar desde el entorno QGIS para extraer porciones de la capa que contiene las de 630.000 construcciones de la provincia de Alicante.

![QGIS Alicante](/img/alicante.jpg)

Se obtiene un shapefile con unas dimensiones de 960 m por 960m. 

**1_Obtencion_imagenes_PNOA.ipynb** 

Obtiene las coordenadas del shapefile y extrae del servicio WMS del PNOA un array de (10,10)
de imagenes RGB de 384x384 pixeles.
Teniendo en cuenta que la resolución del PNOA en la provincia de Alicante es 25 cm/píxel, cada imagen mide 96x96 metros.

![RGB](/img/680667.12_4238075.56_680763.12_4238171.56.jpg)

**2_Creacion_imagenes_segmentadas.ipynb** 

Se obtiene un fichero Geotiff de cada shapefile, posteriormente se trocea el Geptiff para obtener otro de dimensiones (384,384) que coinciden en nombre con las imágenes RGB.
De esta forma obtenemos el dataset para el proyecto.


![TIFF](/img/680667.12_4238075.56_680763.12_4238171.56_tiff.jpg)

/datos/1_Creacion_dataset/shape.rar 
Contiene 40 shapefiles que se han utilizado para obtener las 4000 imágenes RGB y 4000 imágenes segmentadas

#  Red Neuronal

En esta fase del proyecto, entrenamos y testeamos el modelo de red neuronal 

**3_modelo_vgg19_2_200.ipynb** 

Creamos y entrenamos el modelo Keras, se han utilizado varias backbones y distintas cantidades de epoch. Se obtiene las métricas de la validación y se guarda el modelo. Se muestran algunas imágenes obtenidas del modelo.

Características de la red del ejemplo:
-	VGG19
-	Batch = 2
-	Epoch = 200
-	Loss = JaccardLoss

/Datos/2_Red_Neuronal/dataset.rar 
Incluye un dataset reducido con 2000 imágenes RGB y 2000 segmentadas para entrenar el modelo.


**4_prediccion_imagenes.ipynb** 

En un modelo similar al anterior, cargamos los pesos guardados del modelo anterior. Ahora puede predecir las imágenes segmentadas desde imágenes RGB. Modificando algunos parámetros se han llegado a nalizar imágenes RGB de 1 km2.
Se guardan las imágenes analizadas para su posterior comparación.

No se ha podido ingluir un fichero .h5 con los pesos por limitación de espacio de Github


#  Comparación de imágenes

**5_Comparación_Imagenes**

Comparamos la imagen segmentada original con la imagen segmentada predicha por el modelo.
Creamos un dataframe para ambas imágenes con las siguientes columnas:
-	BBOX: Coordenadas del cuadro delimitador
-	Ancho: Ancho del cuadro
-	ALto: Alto del cuadro
-	IoU: Valor IoU más alto del cuadro
-	BOX_CRUCE: Caja de la otra imagen con la que se obtiene el IoU mas alto
-	ACC: Pixeles bien predichos
-	SUPERFICIE: Superficie de la construcción que engloba la caja
-	SUP_INV: Superficie de la construcción de la caja, pero en la imagen con la que compara

Comparamos la imagen segmentada original con la imagen segmentada predicha por el modelo.
Creamos un dataframe para ambas imágenes con las siguientes columnas:
-	BBOX: Coordenadas del cuadro delimitador
-	Ancho: Ancho del cuadro
-	ALto: Alto del cuadro
-	IoU: Valor IoU más alto del cuadro
-	BOX_CRUCE: Caja de la otra imagen con la que se obtiene el IoU mas alto
-	ACC: Pixeles bien predichos
-	SUPERFICIE: Superficie de la construcción que engloba la caja
-	SUP_INV: Superficie de la construcción de la caja, pero en la imagen con la que compara


Analizamos los campos para obtener las siguientes incidencias:

-	No Detectada
-	Nueva Construcción
-	ampliación

Por último, guardamos las incidencias y sus coordenadas UTM en un fichero .csv

/Datos/3_Comparación_Imagenes/ 
Incluye 3 pares de imágenes de ejemplo para su comparación.

**6_Visualizacion_QGIS.ipynb**

Muestra como cargar el archivo csv en QGIS como una capa vectorial y como visualizar los resultados de forma categorizada.
