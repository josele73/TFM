# TFM -- Control de cambios en imágenes aéreas segmentadas

El objetivo de este proyecto es la comparación de las imágenes segmentadas que contienen las construccioines en las imágenes áreas del PNOA con imágenes de años anteriores con el fin de localizar cambios          (Nuevas construcciones, construcciones que ya no existan y grandes ampliaciones).

El proyecto tienes tres fases:

![Ciclo fases](/img/esquema.jpg)


#  Obtención de imágenes PNOA 
Inicialmente se debe trabajar desde el encorno QGIS para extraer porciones de la capa que contiene las de 630.000 construcciones de la provincia de Alicante.

![QGIS Alicante](/img/alicante.jpg)

Se obtiene un shapefile con unas dimensiones de 960 m por 960m. 
El notebook 1_Obtencion_imagenes_PNOA.ipynb obtiene las coordenadas del shapefile y extrae del servicio WMS del PNOA un array de 10*10 imágenes RGB con una dimensiones de 384*384 pixeles.
Teniendo en cuenta que la resolución del PNOA en la provincia de Alicante es 25 cm/píxel, cada imagen mide 96*96 metros.

![RGB](/img/680667.12_4238075.56_680763.12_4238171.56.jpg)
