#Ejercicios. Tema 4.


## Ejercicio nº1.

Para la inslación de lxc utilizo el paquete contenido de lxc contenido por defecto en mi máquina virtual de Debian-Jessie. Tras la instalación procedo a hacer un lxc-checkconfig cruzando los dedos para que no haya nada en rojo:

![img](https://i.sli.mg/q8hk84.png)<sup>Instalando lxc.</sup>

![img](https://i.sli.mg/Yde9hF.png)<sup>Salida checkconfig.</sup>


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
![img](https://i.sli.mg/oXpVVs.png)


En el host puedo comprobar que se ha añadido una interfaz lxc-bridge-land y con la orden lxc-ls compruebo la ip que tiene el container:
![img](https://i.sli.mg/jgIWV2.png)

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


![img](https://i.sli.mg/sZpXTj.png)<sup>Instalando lxc_debian.</sup>


Como se puede observar en el ejercicio nº2 la instalación se produce de manera satisfactoria.

#### Contenedor basado en Fedora.

A continuación pasamos a instalar un contenedor basado en otra distribución para lo cual elegimos Fedora.  Tras unos cuantos intentos fallidos nos damos cuenta de que nos da un fallo diciendonos que no encuentra yum asi que lo instalamos usando apt-get y volvemos a lanzar la instalación:


![img](https://i.sli.mg/MgxCdU.png)
<sup>Instalando lxc_fedora.</sup>



La instalación finaliza de manera satisfactoria pero a la hora de inicializar el contenedor fedora nos da un error relacionado con la interfaz de de networking asi que editamos el fichero de configuración para el container fedora en: ``` /var/lib/lxc/lxc_fedora/config ```

poniendo como interfaz de red ninguna tras lo cual arranca satisfactoriamente.
![img](https://i.sli.mg/QIdnN3.png)<sup>Editando fichero configuración container fedora e inicializando.</sup>


El fallo posiblemente esté relacionado con que no utilizamos el modo normal para que se conecten los container a la red es decir el modo bridge con las interfaces puente sino que utilizamos Nat.
Posiblemente si hiciéramos lo mismo que para el container de debian (vease ejercicio nº2) y configuraramos el container de fedora para que se conectara a nuestra red nat entonces muy posiblemente nuestro container fedora tendría libre acceso a internet.


## Ejercicio nº4.

Procedemos a instalar lxc-web utilizando el scrip que nos indican en la [web](http://lxc-webpanel.github.io/install.html) tras lo cual probamos abrimos el navegador web y nos conectamos al puerto 5000.

![img](https://i.sli.mg/DE79ED.png)
<sup>Tras instalar lxc-web accedemos a su interfaz web.</sup>


Podemos observar como nos muestra los diferentes contenedores que tenemos instalados en el sistema. Tambiés podemos observar que no nos muestra el de fedora pero esto es porque la captura fué realizada mientras se instaba el fedora.

A continuación pasamos a restringer los cpus que puede utilizar el container de fedora poniendo que solamente pueda utilzar los cores del 0 al 4 y restringiendo la memoria ram que puede utilizar a un tope de 888MB

![img](https://i.sli.mg/5Vwj7d.png)
<sup>Restringiendo recursos que puede utilizar  el container basado en fedora</sup>

