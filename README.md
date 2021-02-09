# screw_inspection

Este repositorio hace parte de un proyecto de inspección con visión artificial en una linea de producción de partes de vehiculos. El problema a tratar es de detección, segmentación y clasificación de 9 tipos de tuercas de distintos tamaños, algunas con diferentes colores, las cuales no son tan fáciles de distinguir para la vista humana.

## Detección
Para esta etapa se detectan posibles ubicaciones de tuercas (no incluido en el repositorio), y posterior a eso se hace una etapa de refinamiento de detección pasando los canidatos por un detector binario. Lo que incluyo en este repositorio es la creación del modelo (detector_model_train.ipynb).

### Dataset
Dado que el problema es detectar si hay o no hay tuerca en la ventana, para el dataset se tienen 2 clases. Las ventanas donde hay tuercas, vendría siendo la clase 1, y las ventanas donde NO hay tuercas, o hay tuercas incompletas que no sirven para la clasificación, son tomadas como clase 0. (En el documento se observan algunos ejemplos de cada clase).

### Extracción de caracteristicas y modelo.
Se extrajeron caracteristicas HOG para tener información sobre la forma de los objetos. Luego se utilizaron 3 capas full conected con función de activación sigmoid, regularización L2 y dropout = 0.25. Finalmente se conecta una capa tipo Softmax.

### Resultados
Precisión con los datos de Test:  0.9971098303794861

              precision    recall  f1-score   support

           0       0.99      1.00      1.00       173
           1       1.00      0.99      1.00       173

    accuracy                           1.00       346
    macro avg       1.00     1.00      1.00       346
    weighted avg    1.00     1.00      1.00       346


## Clasificación
Luego de la etapa de refinamiento de la detección, se tienen ventanas de las tuercas a que pasaran a la etapa de clasificación, el archivo que se incluye en este repositorio corresponde a la creación del modelo de clasificación (classifier_model_train.ipynb).

### Dataset
Para este dataset se tienen 9 clases correspondientes a las 9 tuercas. Por cada tuerca se tomaron 16 imágenes en distintas posiciones respecto a la cámara para añadir robustez ante la variación traslacional. Posteriormente se realiza una etapa de Data Augmentation en la cual se le hace un vertical flip, se rota la imagen, se varia la iluminación y se le aplica un shift vertical y horizontalmente. Las imagenes son normalizadas al rango 0 a 1.
Nota: Posteriormente también se le añade ruido, zoom in y zoom out a las imagenes para añadir más rubustez. Esto no está todavía en esta versión de archivos.

### Modelo
Para la extracción de caracteristicas se utilizó una red neuronal convolucional de 2 capas de 16 y 32 filtros respectivamente (5x5) con función de activación relú, aplicando un dropout de 0.2 y MaxPooling 2x2 en cada capa. Luego hay 2 capas full conected con activación Relu. Se probó añadir capas BatchNormalization pero empeoraba considerablemente los resultados, por lo que no se incluyeron. Finalmente hay una capa tipo Softmax.

### Resultados
Precisión en el Test:  0.9983726739883423

              precision    recall  f1-score   support

           0       1.00      1.00      1.00       410
           1       1.00      1.00      1.00       409
           2       1.00      1.00      1.00       410
           3       1.00      1.00      1.00       410
           4       1.00      1.00      1.00       409
           5       1.00      1.00      1.00       410
           6       1.00      1.00      1.00       410
           7       1.00      1.00      1.00       410
           8       1.00      1.00      1.00       409

    accuracy                           1.00      3687
    macro avg      1.00      1.00      1.00      3687
    weighted avg   1.00      1.00      1.00      3687
