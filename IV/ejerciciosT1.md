#Ejercicios. Tema 1.

	
## Ejercicio nº1.


Nosotros vamos a comprar un [servidor Dell](http://www.dell.com/es/empresas/p/poweredge-t430/pd?oc=pet43001x%20-%20offer%20valid%20until%2028.10.2016&model_id=poweredge-t430), en concreto:


![img](https://i.sli.mg/vEsNT8.png)

Que tiene un precio (sin IVA) de 946€. 

### Amortización a 4 años.
Coste de amortizacion a *4 años* considerando que comprobamos el servidor a principio de año:

946€ / 4 años = 236.5 € / por año

Por tanto tendríamos que pagar:

* 2016, 2017, 2018, 2019 : 236.5 € anuales

### Amortización a 7 años.


La amortización a *7 años* considerando que comprobamos el servidor a principio de año:

946€ / 7 años = 135.15 € / por año

Por tanto tendríamos que pagar:
 
* 2016, 2017, 2018, 2019, 2020, 2021, 2022 : 135.15 € anuales

## Ejercicio nº2



Vamos a comparar el precio entre web hosting y cloud hosting durante un tiempo de un año.



Para el web hosting hemos seleccionado [b2evolution <sup>El * de b2evolution es que el primer mes te cobran 30 € y el resto 60 €.
</sup>] (http://b2evolution.net/web-hosting/dedicated-servers.php)

![img](https://i.sli.mg/kL0Y38.png)

y para el cloud hosting [VMWare](http://vcloud.vmware.com/service-offering/pricing-calculator/on-demand)

![img](https://i.sli.mg/FsUS9d.png)


### Uso: 1%.
B2evolution te va a cobrar lo mismo lo uses lo que lo uses por tanto un año son:

* 30 € primer mes + 60€ * 11 meses = 690€.

VMWare utilizando tecnología similar:
*   12 meses * 40€/mes = 480 €

Web hosting es un 43% más caro.

### Uso: 10%.
 
 B2evolution sigue igual: 

* 30 € primer mes + 60€ * 11 meses = 690€.

Al introducir el nuevo % de utilización en VMWare obtenemos:

* 12 meses  * 47.77€/mes = 573.24 €.

![img](https://i.sli.mg/P7v4NX.png)


Ahora web hosting sale un 20% más caro. Destacar que VMWare  al igual que B2 te ofrece IP pública lo cual encarece el precio comparado con otros competidores de cloud hosting que no estoy seguro de si te lo ofrecen.

 

## Ejercicio nº3.


1. [Link al issue](https://github.com/JJ/IV16-17/issues/1#issuecomment-251473646)
2.  Mi intención de cara al proyecto de la asignatura es crear un bot de Telegram que te aconseje sobre un pelicula basandose en el título de una pelicula que a ti te guste, para ello utilizo la api que me proporciona una web de la cual obtengo en formato json titulos de peliculas parecidos a la pelicula que se le paso al programa (Por ahora solamente soporta titulos en ingles, proximamente en español). Ejemplo de ejecución del programa:

![img](https://i.sli.mg/528gh8.png)

Código fuente (Ruby):

![img](https://i.sli.mg/RgxSI6.png)

Me [descargo](http://www.pgbovine.net/cde/manual/) desde la web y ejecuto la orden
```
./cde ./twitter.rb
```

![img](https://i.sli.mg/Ud3coU.png)

## Ejercicio nº4

Resultado de ejecutar
```
egrep '^flags.*(vmx|svm)' /proc/cpuinfo
```



 



