# TFM -- Control de cambios en imágenes aéreas segmentadas

El objetivo de este proyecto es la comparación de las imágenes segmentadas que contienen las construccioines en las imágenes áreas del PNOA con imágenes de años anteriores con el fin de localizar cambios          (Nuevas construcciones, construcciones que ya no existan y grandes ampliaciones).

El proyecto tienes tres fases:

![Ciclo fases](/img/esquema.jpg)


#  Obtención de imágenes PNOA 

Inicialmente se debe trabajar desde el encorno QGIS para extraer porciones de la capa que contiene las de 630.000 construcciones de la provincia de Alicante.

![QGIS Alicante](/img/alicante.jpg)

Se obtiene un shapefile con unas dimensiones de 960 m por 960m. 

**1_Obtencion_imagenes_PNOA.ipynb** 

Obtiene las coordenadas del shapefile y extrae del servicio WMS del PNOA un array de (10,10)
de imagenes RGB de 384*384 pixeles.
Teniendo en cuenta que la resolución del PNOA en la provincia de Alicante es 25 cm/píxel, cada imagen mide 96*96 metros.

![RGB](/img/680667.12_4238075.56_680763.12_4238171.56.jpg)

**2_Creacion_imagenes_segmentadas.ipynb** 

Se obtiene un fichero Geotiff de cada shapefile, posteriormente se trocea el Geptiff para obtener otro de dimensiones (384,384) que coinciden en nombre con las imágenes RGB.
De esta forma obtenemos el dataset para el proyecto.


![TIFF](/img/680667.12_4238075.56_680763.12_4238171.56_tiff.jpg)

/datos/1_Creacion_dataset/shape.rar Contiene 40 shapefiles que se han utilizado para obtener las 4000 imágenes RGB y 4000 imágenes segmentadas

#  Red Neuronal

En esta fase del proyecto, entrenamos y testeamos el modelo de red neuronal 

**3_modelo_vgg19_2_200.ipynb** 

Creamos y entrenamos el modelo Keras, se han utilizado varias backbones y distintas cantidades de epoch. Se obtiene las métricas de la validación y se guarda el modelo. Se muestran algunas imágenes obtenidas del modelo.

Características de la red del ejemplo:
-	VGG19
-	Batch = 2
-	Epoch = 200
-	Loss = JaccardLoss


**4_prediccion_imagenes.ipynb** 

EN un modelo similar al anterior, cargamos los pesos guardados del modelo anterior. Ahora puede predecir las imágenes segmentadas desde imágenes RGB. Modificando algunos parámetros se han llegado a nalizar imágenes RGB de 1 km2.
Se guardan las imágenes analizadas para su posterior comparación.

