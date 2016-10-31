#Ejercicios. Tema 3.

	
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

Seguimos utilizando la aplicación del anterior tema para este ejercicio y el siguiente vamos a comprobar si las urls "../todas_recetas" muestran al salida que debe y si "../toda_receta/receta_en_concreto" también devuelve un resultado correcto. 

../todas_recetas debe devolver todas las recetas que se han dado de alta en la aplicación en formato json mientras que "../toda_receta/receta_en_concreto" devuelve en formato json los datos de una receta en concreto, he aqui los contraladores:

![img](https://i.sli.mg/G205L6.png)

Flask comprueba con que url se accede a la aplicación y busca aquella que cumple el patrón especificado en nuestro fichero de controladores, cuando llega a  "../todas_recetas" realiza una consulta a ala base de datos para obtener todas aquellas "Recetas" contenida en ella, vease modelos de la aplicación:
![img](https://i.sli.mg/fGuveP.png)

Tras lo cual empaqueta todas las recetas en formato JSON y devuelve este paquete como respuesta.

El funcionamiento de ../toda_receta/receta_en_concreto" es similar.




## Ejercicio nº4.

Para testear estas dos url utlizamos los test que se pueden ver en la siguiente captura:

![img](https://i.sli.mg/EUB83d.png)

Básicamente introducen datos en la aplicación para que esta  al ser accedida a través de las dos url anteriores pueda devolver algún resultado. Tras lo cual establecemos en la variable ** array ** cual es la respuesta que esperamos del servidor y haciendo uso de la función assertEqual que nos proporciona unittest en python comprobamos si el resultado esperado es el que recibimos al consultar las url.


También podemos observarlo directamente a través del navegador:

![img](https://i.sli.mg/WEeCIe.png)
