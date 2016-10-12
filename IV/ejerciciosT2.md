#Ejercicios. Tema 2.

	
## Ejercicio nº1.

Vamos a instalar el entorno virtual de desarrollo para ello seguimos seguimos los pasos descritos en la [web de virtualenv](https:/virtualenv.pypa.io/en/latest/installation/):

1. Instalamos virtualenv, para ello primero instalamos pip y después virtualenv:


![img](https://i.sli.mg/UuKx8j.png)

2. Creamos el entorno virtual y lo "activamos" :

![img](https://i.sli.mg/r9emfK.png)

Podemos observar a la izquierda del promp de la shell el entorno virtual que ahora mismo estamos usando en mi caso "entorno_proyecto".
Ahora en este proyecto instalamos la ultima versión de Django que usaremos en el ejercicio segundo.


![img](https://i.sli.mg/vvI0aK.png)

Intentando comprender que es virtualenv instalo dos versiones diferentes de Django dando el resultado de la imagen de más arriba. Si quisiera probar dos versiones de Django para un proyecto tendría que crearme para cada uno un entorno virtual independiente. 

Vuelvo a instalar la última versión y desactivo el entorno virtual de desarrollo utilizando la orden `deactivate`


## Ejercicio nº2.

Aprovechando este ejercicio voy a empezar a desarrollar el esqueleto de mi proyecto. Como primer paso vuelvo a activar el entorno virtual de desarrollo que contiene la version 10.2 de Django  y hay creo el proyecto:


![img](https://i.sli.mg/PZol3p.png)


La aplicación que voy a implementar permite a un usuario administrador crear peliculas y editar sus datos y a los usuarios públicos votar las películas.


## Referencias:



http://jj.github.io/IV/documentos/temas/Desarrollo_basado_en_pruebas
http://docs.python-guide.org/en/latest/dev/virtualenvs/
https://docs.djangoproject.com/en/1.10/intro/tutorial01/
http://www.simononsoftware.com/virtualenv-tutorial-part-2/

