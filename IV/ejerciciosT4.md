#Ejercicios. Tema 4.


## Ejercicio nº1.

Para la inslación de lxc utilizo el paquete contenido de lxc contenido por defecto en mi máquina virtual de Debian-Jessie. Tras la instalación procedo a hacer un lxc-checkconfig cruzando los dedos para que no haya nada en rojo:

![img](https://ibin.co/39KstFPqq0QB.png)<sup>Instalando lxc.</sup>

![img](https://ibin.co/39KszBD0a5tn.png)<sup>Salida checkconfig.</sup>


El proceso de creación de los container lo reservamos para el ejercicio 3.


## Ejercicio nº2.

El proceso de habilitar conectividad a internet al container es más peliagudo. Como no estoy utilizando lxc sobre ubuntu sino sobre debian en la [wiki](https://wiki.debian.org/LXC/SimpleBridge) y en [flockport](https://www.flockport.com/lxc-networking-guide/)  encontramos dos opciones a elegir:

* Bridge: el host como bridge lo cual implica que nuestro container tendrá una ip como cualquier otra máquina física conectada a nuestra red wlan0.

* NAT: La conexión a internet del container se realiza utilizando la ip que tiene la máquina host.

Yo opto por NAT para lo cual de manera resumida habilito  el "forwarding" de paquetes ipv4 en mi host y en la configuración de las interfaces de red con las que cuenta el host le añado la interfaz con la cual va ha hablar el container. Para eso edito /etc/network/interfaces y le añado lo siguiente:

```
auto lxc-bridge-nat
iface lxc-bridge-nat inet static
        bridge_ports none
        bridge_fd 0
        bridge_maxwait 0
        address 192.168.100.1
        netmask 255.255.255.0
        up iptables -t nat -A POSTROUTING -o wlan0 -j MASQUERADE
```

Tras lo cual reinicio el container y compruebo si tengo conectividad a internet en el container para lo cual ejecuto apt-get update porque no tiene ni ping instalado:
![img](https://ibin.co/39KuopLUb3Pc.png)


En el host puedo comprobar que se ha añadido una interfaz lxc-bridge-land y con la orden lxc-ls compruebo la ip que tiene el container:
![img](https://ibin.co/39KuaStqh2jY.png)

Cuando instalé el container sin hacer nada de todo esto de las interfaces de red al hacer ifconfig -a en el container solo tenía la interfaz lo y ahora tiene 2 nuevas interfaces una eth0 con la ip del host y otra con lxc-bridge-nat con lo cual uno se pregunta cual es la que usa. Viendo la salida de lxc-ls en el host pienso que obtiene la ip de eth0 hablando con el host a traves de lxc-bridge-nat  y pasa a contactar con internet a traves de eth0.

## Ejercicio nº3.

#### Contenedor basado en Debian.
Para crear un container utilizando lxc utilizamos la orden lxc-create:
```
 lxc-create -t debian -n lxc_debian
```
Lo cual inicia el proceso de creación del container tras el cual para poder loguearnos en el container es necesario en primer lugar inicializarlo, nosotros utilizamos la orden:
```
lxc-start -F -n lxc_debian
```
Y a continuación en otra terminal ejecutamos 

```
lxc-attach -n lxc_debian passwd
```

E introducimos la contraseña que tendrá el usuario root en el contaner lxc_debian pudiendo asi loguearnos en el container como root.


![img](http://i1339.photobucket.com/albums/o720/rand9882/2017-01-18_195822_zps1rai6uks.png)
<sup>Instalando lxc_debian.</sup>


Como se puede observar en el ejercicio nº2 la instalación se produce de manera satisfactoria.

#### Contenedor basado en Fedora.

A continuación pasamos a instalar un contenedor basado en otra distribución para lo cual elegimos Fedora.  Tras unos cuantos intentos fallidos nos damos cuenta de que nos da un fallo diciendonos que no encuentra yum asi que lo instalamos usando apt-get y volvemos a lanzar la instalación:


![img](http://i1339.photobucket.com/albums/o720/rand9882/lxc8_zpsv08eqpeo.png)
<sup>Instalando lxc_fedora.</sup>



La instalación finaliza de manera satisfactoria pero a la hora de inicializar el contenedor fedora nos da un error relacionado con la interfaz de de networking asi que editamos el fichero de configuración para el container fedora en: ``` /var/lib/lxc/lxc_fedora/config ```

poniendo como interfaz de red ninguna tras lo cual arranca satisfactoriamente.
![img](http://i1339.photobucket.com/albums/o720/rand9882/lxc12_zpsel3olfup.png)<sup>Editando fichero configuración container fedora e inicializando.</sup>


El fallo posiblemente esté relacionado con que no utilizamos el modo normal para que se conecten los container a la red es decir el modo bridge con las interfaces puente sino que utilizamos Nat.
Posiblemente si hiciéramos lo mismo que para el container de debian (vease ejercicio nº2) y configuraramos el container de fedora para que se conectara a nuestra red nat entonces muy posiblemente nuestro container fedora tendría libre acceso a internet.


## Ejercicio nº4.

Procedemos a instalar lxc-web utilizando el scrip que nos indican en la [web](http://lxc-webpanel.github.io/install.html) tras lo cual probamos abrimos el navegador web y nos conectamos al puerto 5000.

![img](http://i1339.photobucket.com/albums/o720/rand9882/lxc9_zpsqall8twr.png)
<sup>Tras instalar lxc-web accedemos a su interfaz web.</sup>


Podemos observar como nos muestra los diferentes contenedores que tenemos instalados en el sistema. Tambiés podemos observar que no nos muestra el de fedora pero esto es porque la captura fué realizada mientras se instaba el fedora.

A continuación pasamos a restringer los cpus que puede utilizar el container de fedora poniendo que solamente pueda utilzar los cores del 0 al 4 y restringiendo la memoria ram que puede utilizar a un tope de 888MB

![img](http://i1339.photobucket.com/albums/o720/rand9882/lxc14_zpsp9ujswhr.png)
<sup>Restringiendo recursos que puede utilizar  el container basado en fedora</sup>



## Ejercicio nº 6.

Instalamos docker siguiendo las instrucciones de la página web de docker.

![img](http://i1339.photobucket.com/albums/o720/rand9882/doc1_zpsplrmnd2z.png)
<sup>Mostramos salida hello-world tras la instalación de docker y los comandos empleados para su instalación.</sup>


## Ejercicio nº 7.

Procedemos con la instalación de un contenedor docker de Ubuntu para ello no tenemos más que ejecutar la orden:
```
 docker pull ubuntu
```
![img](http://i1339.photobucket.com/albums/o720/rand9882/doc2_zpszxyptn79.png)
<sup>Inslación de imagen Ubuntu y ejecución comando ls en ella.</sup>

Tras lo cual procedemos a instalar una imagén que no pertenezca a la familia de ubuntu asi que instalamos CentOS.

![img](http://i1339.photobucket.com/albums/o720/rand9882/doc6_zpstkohe2ew.png)
<sup>Inslación de imagen CentOS y ejecución comando ls en ella.</sup>

Fácil e indoloro comparando  con lxc el proceso de instalación de la imagenes podemos decir que docker es considerablemente más rápido, limpio y no hace falta intalar cosas adicionales como por ejemplo tuvimos que instalar yum en nuestro ordenador para poder instalar la imagen de CentOS en lxc.

También hay que destacar el tamaño de las imágenes la de lxc de CentOS ocupaba más de 500MB mientras que la de docker ocupa 200.  Hay que remarcar que se  tiene conectividad dentro del contenedor desde su creación. En total el proceso de instalación de las imagenes y la habilitación de internet hemos tardado 2 horas con docker que con lxc.Por tanto y a modo de resumen el uso de docker aparentemente require de la configuración de menos cosas y su uso es más amigable de cara al usuario. 



## Ejercicio nº 8.


Utilizamos el comando:

```
sudo docker run -i -t ubuntu /bin/bash
```

para iniciar una shell en el contenedor ubuntu desde ahí procedemos a ejecutar el comando:
```
useradd user-ubuntu
```
para crear al usuario user-ubuntu en el sistema le damos una contraseña utilizando el comando
```
passwd user-ubuntu
```
para poder loguearnos con él. Tras lo cual lo añadimos al grupo sudo, nos loguemos con el y procedemos a instalar nginx y curl.

![img](http://i1339.photobucket.com/albums/o720/rand9882/doc9_zpsrdqihdpj.png)
<sup>Mostramos historial para crear usuario y comprobamos la instalación correcta de Nginx</sup>


## Ejercicio nº 9.

Para poder hacer el commit de una imagen de ubuntu en primer lugar hace falta encontrar el id de dicho container para lo cual empleamos la orden:

```
docker ps -a
```
Que nos muestra todos los containers que hay junto con sus ids. Tras lo cual como queremos guardar una imagen original de ubuntu sin que le hayamos instalado el nginx seleccionamos la imagen que se ha generado con el comando ls que ejecutamos tras la instalación del container de Ubuntu:

![img](http://i1339.photobucket.com/albums/o720/rand9882/doc10_zps2huqao1m.png)
<sup>Mostramos todas las imágenes creadas de docker en nuestro sistema y hacemos commit sobre la de ubuntu resultado de ejecutar sobre ella el comando ls utilizando docker run.</sup>

Como se puede observar en la imagenhemos clonado la imagen correcta ya que esta no tiene en su interior un servidor Nginx ejecutandose.


## Ejercicio nº10.

Para poder construir una imagen docker necesitamos crear un archivo denominado Dockerfile:

```
FROM ubuntu:latest

MAINTAINER Luis Gil Guijarro <luisgguijarro9@gmail.com>


ARG TOKEN
ARG TOKEN_TASTEKID
ARG POSTGRES_DATABASE

ENV TOKEN=$TOKEN
ENV TOKEN_TASTEKID=$TOKEN_TASTEKID
ENV POSTGRES_DATABASE=$POSTGRES_DATABASE


RUN apt-get update
RUN apt-get install -y git ruby ruby-dev build-essential libpq-dev

RUN gem install bundler

RUN git clone https://github.com/LuisGi93/proyectoIV2016-2017
WORKDIR proyectoIV2016-2017
RUN bundle install

```
<sup>Dockerfile del proyecto en github: [Dockerfile](https://github.com/LuisGi93/proyectoIV2016-2017/blob/master/Dockerfile)</sup>


 y en el definir que sistema operativo queremos usar ```FROM ```, las variables de entorno que usa nuestra aplicación ```ENV```, instalar en el sistema operativo elegido los paquetes y gemas que utiliza nuestra aplicación ```RUN apt... ```,  clonar  el repositorio ```RUN git clone ...```,  definir el directorio de trabajo ```WORKDIR``` instalar dependendias ```RUN bundle install```.
 
 Este Dockerfile lo ponemos en el directorio de nuestro proyecto y ejecutamos el comando:
 
 ```
docker build -f Dockerfile -t queveobot .
 ```
 
 En su momento se me olvidó tirar captura pero se puede ver en la [documentación del proyecto](https://github.com/LuisGi93/proyectoIV2016-2017/blob/hito2/README.md) (apartado 4) como se constrouye el  proyecto correctametne en dockerhub y se puede probar la imagen utilizando un:
 
 ```
docker pull luisgi93/proyectoiv2016-2017

```



Tras lo cual accedemos al contenedor utilizando la orden:

```
docker run -e "TOKEN=xxx" -e "TOKEN_TASTEKID=xxx" -e "POSTGRES_DATABASE=xxx"  -i -t luisgi93/proyectoiv2016-2017 /bin/bash
```
