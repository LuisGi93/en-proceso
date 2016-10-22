#Ejercicios. Tema 2.

	
## Ejercicio nº1.

Vamos a instalar el entorno virtual de desarrollo para ello seguimos seguimos los pasos descritos en la [web de virtualenv](https:/virtualenv.pypa.io/en/latest/installation/):

	1º. Instalamos virtualenv, para ello primero instalamos pip y después virtualenv:


![img](https://i.sli.mg/UuKx8j.png)

	2º. Creamos el entorno virtual y lo "activamos" :

![img](https://i.sli.mg/ZYm4AY.png)

Podemos observar a la izquierda del promp de la shell el entorno virtual que ahora mismo estamos usando en mi caso "entorno_proyecto".
Ahora en este proyecto instalamos la ultima versión de Flask.


![img](https://i.sli.mg/JE7fT1.png)


## Ejercicio nº2.

Para el desarrollo de la web voy a usar Flask para lo cual he seguido la estructura contenida en: [digitalocean](https://www.digitalocean.com/community/tutorials/how-to-structure-large-flask-applications)

La aplicación de una manera simple permite que los usuarios se registren en el sistema permitiendoles introducir recetas y ver las recetas que ellos mismos han introducido o las que han introducido los demás usuarios:

![img](https://i.sli.mg/qkTvRm.png)

![img](https://i.sli.mg/VJcpSz.png)

También registro y login:


![img](https://i.sli.mg/qsf8ON.png)
![img](https://i.sli.mg/92iZhW.png)


## Ejercicio nº3.

Antes de este ejercicio realize el nº4 para probar si el empaquetado de mi programa funciona, es decir, que utilizando un fichero de configuración puedo "comprimir" mi programa y moverlo a una máquina distinta a la de desarrollo y a través de un único comando ser capaz de instalar todo lo que requiere mi programa.
Como se observa en las imágenes de más abajo funciona tanto para python 2.7.9 como para la versión 3.4:


![img](https://i.sli.mg/AjwbAq.png)



## Ejercicio nº4.

Para empaquetar mi programa en python utilizo setuptools y el fichero que indica que dependencias tiene mi programa es setup.py:

![img](https://i.sli.mg/WmFezo.png)

Como se puede observar en las dos imágenes arriba de esta al ejecutar el comando:
```
python miaplicacion/setup.py install
```
Produce como resultado que en mi entorno virtualenv se instalen todos los paquetes contenidos en "install_requries" siendo posible tras la instalación de estos ejecutar el programa en cualquier otro ordenador diferente del de usado para desarrollo. Además en  MANIFEST.in incluyo aquellos ficheros adicionales necesarios para el funcionamiento de mi programa:

![img](https://i.sli.mg/mPqkxs.png)


## Ejercicio nº5

Documentamos algunos de los controladores de la aplicación utilizando la sintasis de pydoc tras lo cual generamos la documentación en formato html:

![img](https://i.sli.mg/wqunV8.png)

## Ejercicio nº 6 y 7
 
Creo los test unitarios utilizando unittest de python y compruebo que mis modelos se insertán correctamente en la base de datos para lo cual utilizo unittest.  También creo un test para obtener todas las recetas actualmente almacenadas en la base de datos de la aplicación web:
![img](https://i.sli.mg/Avvdx2.png)

Al no estar implementada esta última funcionalidad da error.

A continuación voy a mis controladores e implemento la funcionalidad ejecutando nuevamente los test:


![img](https://i.sli.mg/rVukNn.png)


Los programas anteriores se han ejecutado utilizando unittest de python.

## Ejercicio nº 8

Para integración continua vamos a utilizar travis para lo cual nos creamos un fichero .travis.yml:
```
language: python
python:
  - "2.7.9"
  - "3.4.5"
install: "python setup.py install"
script: "python test.py"
```
Tras lo cual subimos todos los ficheros a github, nos damos de alta en travis y en 2 minutos podemos nos ponemos a corregir algun que otro fallo que se me había pasado en el .travis.yml tras lo cual podemos observar que todo va correctamente:

![img](https://i.sli.mg/aSkVQp.png)


La salida de los test es la esperada:

![img](https://i.sli.mg/15y473.png)
![img](https://i.sli.mg/2Ejld0.png)





