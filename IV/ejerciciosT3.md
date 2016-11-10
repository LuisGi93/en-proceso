#Ejercicios. Tema 3.

Todos estos ejercicios los he realizado sobre Bluemix, decir que algunos ejercicios mencionan cosas específicas de heroku o cosas que resulta sumamente complicado realizar sobre Bluemix pero que en Heroku sale fácil.
En el siguiente enlace explico todo el proceso de despliegue de mi aplicación sobre Heroku para dejar constancia en el ejercicio 5, 6, 7 y 8 que comprendo lo que se pide pero que a la hora de intentar emular lo que me piden en Bluemix puede que algo no lo haya podido cumplir del todo:
[Consultar punto 3](https://github.com/LuisGi93/proyectoIV2016-2017/blob/hito2/README.md)
	
## Ejercicio nº1.

Nos damos de alta tanto en Bluemix como en Openshift.

![img](https://i.sli.mg/KCwpci.png)


## Ejercicio nº2.

Como en Openshift tras darnos de alta nos piden que nos pongamos a la cola para entrar (y tras unas horas aún no llega el email):

![img](https://i.sli.mg/nFRgcL.png)

optamos por crear una aplicación flask en Bluemix:

![img](https://i.sli.mg/8DFCV1.png)
![img](https://i.sli.mg/TErwBb.png)

Para poder modificarla en local y poder transferirla a Bluemix utilizamos la linea de comandos que nos ofrecen y nos descargamos el esqueleto de muestra de aplicación flask que nos ofrece Bluemix:


![img](https://i.sli.mg/jsRBht.png)


## Ejercicio nº3.


La aplicación que hicimos para el tema anterior permitia a los usuarios darse de alta, loguearse y una vez se habían logueado estos podían introducir recetas, listar todas las recetas que habían introducido ellos y listas todas las recetas introducidas por los demás usuarios.


En concreto para la aplicación las urls ../todas_recetas debe devolver todas las recetas que se han dado de alta en la aplicación mientras que "../toda_receta/receta_en_concreto" devuelve los datos de una receta en concreto, he aqui los contraladores:

![img](https://i.sli.mg/cSGndD.png)
![img](https://i.sli.mg/RM0g4K.png)

Flask comprueba con que url se accede a la aplicación y busca aquella que cumple el patrón especificado en nuestro fichero de controladores, cuando llega a  "../todas_recetas" realiza una consulta a ala base de datos para obtener todas aquellas "Recetas" contenida en ella, vease modelos de la aplicación:
![img](https://i.sli.mg/fGuveP.png)

Y una salida para las rutas todas_recetas y todas_recetas/receta_en_concreto:

![img](https://i.sli.mg/s9MOa8.png)



## Ejercicio nº4.

Vamos a testear las urls "../todas_recetas" y "../todas_recetas/receta_en_concreto" utlizamos los test que se pueden ver en la siguiente captura:

![img](https://i.sli.mg/6BLTns.png)

Básicamente introducen datos en la aplicación para que esta  al ser accedida a través de las dos url anteriores pueda devolver algún resultado. Tras lo cual establecemos en la variable ** array ** cual es la respuesta que esperamos del servidor y haciendo uso de la función assertEqual que nos proporciona unittest en python comprobamos si el resultado esperado es el que recibimos al consultar las url.



## Ejercicio nº5.

Nosotros vamos a echar a andar nuestra aplicación de flask en Bluemix para lo cual necesitamos crear un fichero .yml que le dice a bluemix que recursos va a necesitar nuestra aplicación y con que url se despliega nuestra aplicación lo cual es importante ya que si previamente estamos usando la url especificada en el archivo .yml y desplegamos una nueva aplicación sobre esa url entonces nos encontraremos con errores bastante extraños. Además del .yml hace falta  un fichero Procfile que le indica a bluemix  y demás PaaS como iniciar nuestra aplicación:


![img](https://i.sli.mg/lFx50h.png)

Con el comando que se muestra en la captura indicamos que nuestra aplicación es un servidor web que recibe peticiones y además se le indica el comando con el cual se debe iniciar nuestra aplicación.

Además como nuestra aplciación hace uso de múltiples paquetes bluemix al igual que hacía travis busca el setup.py  para instalar todos aquellos paquetes que se indica en estos ficheros.


Vamos a subir a Bluemix la aplicación de prueba que nos dan para flask para lo cual utilizamos la línea de comandos de bluemix.
En primer lugar nos logueamos utilizando la flag login y después utilizamos el comando push para enviar la aplicación a bluemix

![img](https://i.sli.mg/X0rZQ7.png)



##Ejercicio nº6



Como estamos trabajando con IBM bluemix no podemos probar nuestra aplicación con foreman utilizamos el método que hemos utilizado hasta ahora para verificar los test antes de subir la aplciación seguimos testeando localmente como haciamos hasta ahora utlizando unistest de python y el comando  python test.py.


En este ejercicio unimos la aplicación de prueba con la que hemos ido desarrollando. En primer lugar modificamos el fichero que se ejecuta para arrancar nuestra aplicación para especificar el puerto que utiliza bluemix que es una variable de entorno para lo cual nos fijamos en como viene hecho en el fichero de prueba:

![img](https://i.sli.mg/mSYV9j.png)

En la imagen de arriba el run.py es el de nuestra aplicación y el welcome.py el de la aplicación de prueba de flask. Emulamos lo que hace para que nuestra aplicación sepa en que puerto escuchar.


En la aplicación realizada no se cuenta con una página tipo index.html que cuando apuntes a la raiz de la url de la aplicación te salga un mensaje de algo cosa que si tiene la aplicación de ejemplo de IBM Bluemix:
![img](https://i.sli.mg/TNe1uB.png)



Vamos a hacer que nuestra aplicación funcione como hasta ahora y además lo que hace la aplicación de prueba, es decir que cuando se acceda a urlaplicación muestre por ejemplo el mensaje de bienvenida de la aplicación de prueba y cuando se accede a urlaplicacion/modauth/todasrecetas muestre todas las recetas que  contiene nuestra aplicación. 

Ejecutamos los test de nuevo tras añadir el contenido de la aplicación de prueba  de IBM a nuestra aplicación. Los test pasan por lo que a continuación la subimos a Bluemix.

![img](https://i.sli.mg/CkMkkH.png)

Tras subirla a Bluemix:

![img](https://i.sli.mg/ZOFDZm.png)



##Ejercicio nº7 y 8

Por alguna razón desconocida hay que armar un follón para poder integrar github con Bluemix. En primer lugar hay que abrirse una cuenta en https://hub.jazz.net/ que en principio pensaba que sería un sitio falso para pillar credenciales pero por lo que se menciona en ibm.com es de su propiedad:

En el proceso de creación de la cuenta insertamos el repositorio de github que contiene nuestro proyecto.

![img](https://i.sli.mg/OkvNWm.png)

Especificamos que tareas queremos que se realizen cuando detecte que  produce un push sobre el repositorio de github del proyecto.

![img](https://i.sli.mg/zQz8dI.png)

Creamos una tarea que lanza de nuevo nuestra aplicación en bluemix en cuanto realizamos un push al repositorio del poryecto:

![img](https://i.sli.mg/y4aX1t.png)


En cuanto detecta un push automáticamente empieza a relanzar nuestra aplicación en bluemix:

![img](https://i.sli.mg/TrgFjs.png)


![img](https://i.sli.mg/6y7d2A.png)

Se puede observar en la siguiente captura como clona el repositorio y lo manda al host que le hemos indicado en el manifest.yml:

![img](https://i.sli.mg/2bcyIa.png)

Tras su despliegue comprobamos su funcionamiento:

![img](https://i.sli.mg/W07rMa.png)

Si se quiere probar en primer lugar hay que acceder  a:

https://semana5ivhyaloid-penicil.mybluemix.net/auth/sigup
para registrarse después hay que loguearse en:
https://semana5ivhyaloid-penicil.mybluemix.net/auth/signin
Se introduce una receta en:
https://semana5ivhyaloid-penicil.mybluemix.net/auth/introducir_receta
y se comprueba que se ha introducido en 
https://semana5ivhyaloid-penicil.mybluemix.net/auth/todas_recetas
o en https://semana5ivhyaloid-penicil.mybluemix.net/auth/todas_recetas/titutlodemireceta.


Decir que no he aún tengo pendiente como añadir a la secuencia de despliegue que no despliegue si no se pasan los test pero que hasta el momento no he conseguido comprender del todo como hacerlo ya que en hub.jazz.net no hay ni virtualenvs, ni nada para añadir travis ni forma de instalar la aplicación sobre hub.jazz.net antes de que la envie..



