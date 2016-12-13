#Ejercicios. Tema 4.


## Ejercicio nº1.

Instalamos kvm tal cual  se nos indica en el enlace del ejercicio.

![img](https://i.sli.mg/0zFvQx.png)

Instalamos los paqueters y añadimos a nuestro usuario al grupo de kvm y al libvirt.



## Ejercicio nº2.

### Máquinas virtuales con QEMU.
Creamos un disco tipo QCOW2 ejecutando la orden:

```

qemu-img create -f qcow2 ~/Downloads/disco-sli.qcow2 4.5G
```

Y a continuación arrancamos la instalación del sistema con la orden:

```
qemu-system-x86_64 -hda ~/Downloads/disco-sli.qcow2 -cdrom path_del_iso_a_instalar.iso -m 1G
```

Esta orden indica a qemu un disco y un cd para que cuando se inicie los utilize, en concreto presupongo que si le indicamos esta orden tal cual intenrá arrancar primero del cd y después del disco siendo visibles el disco y el cd entre ellos. Tras el arranque de qemu  instalamos el SO contenido en el .iso. En mi caso he instalado DML:

![img](https://i.sli.mg/uiph6I.png)

Una vez completada la instlación accedemos al sistema para comprobar si se ha instalado correctamente para ello arrancamos con la orden:

```
qemu-system-x86_64 -boot order=c -drive file=disco-sli.qcow2 -m 1G

```
que básicamente le indica a quemu que se quiere arrancar desde ese disco:

![img](https://i.sli.mg/ilpcux.png)
![img](https://i.sli.mg/LBYOaa.png)

### Máquinas virtuales con Virtualbox.
A continuación repetimos el proceso de crear maquinas virtuales utilizando virtualbox. En primer lugar abrimos virtualbox y seleccionamos la pestaña que pone "New"


![img](https://i.sli.mg/3jiwd8.png)


Le damos nombre a nuestra máquina virtual y a continuación seleccionamso en un slider el tope  maximo de RAM que queremos que consuma y pasamos a crear el disco en el cual se va a instalr el sistema:


![img](https://i.sli.mg/B8T37s.png)

Seleccionamos el tamaño que deseamos que tenga el disco y le damos un nombre para identificarlo. A conitnuación ya se habrá creado una máquina virtual pero al arrancar la máquina como el disco recien creado no tiene nada instalado no sabrá como arancar. Asi que nos vamos al menu de virtualbox selccionamos settings sobre nuestra máquina virtual reciente creada y nos vamos a la pestaña denominada "storage" alli le añadimos la .iso que nos hemos descargado en mi cao el .iso de GALPon Minimo, Todo tiene que quedar asi:

![img](https://i.sli.mg/Krd6zV.png)
A continuación arrancamos y seguimos los pasos habitulaes para instalar un sistema operativo tipo Debian.


![img](https://i.sli.mg/uTFeBI.png)



## Ejercicio nº3.

Creamos un benchmark que en primer lugar crea un directorio si este no existe y en el crea 50 ficheros de texto con 10^5 líneas de texto llegando a ocupar cada fichero unos 2.5mb, creandose por tanto en total 250MB:


```
require 'benchmark'
require 'fileutils'



n = 50
m= 10**5
Benchmark.bm do |x|
  x.report("benchmark io:")   {
    FileUtils.mkdir_p 'dir_benchmark'
    Dir.chdir('dir_benchmark')
    for i in 1..n;
      file= File.open("prueba#{i}.txt", 'w')
        for j in 1..m;
          file.write("¿Cuántos alces hay en Canada?")
          file.write("\n")
        end
    end
    file.close;

  }

end

```

En primer lugar lo utililizamos sobre una máquina Lubuntu corriendo sobre Qemu sin utilizar paravirtualización con la orden:

```
qemu-system-x86_64 -boot order=c -drive file=disco-lubuntu.qcow2 -m 1G
```

![img](https://i.sli.mg/MY6Dyo.png)
<sup>Se puede observar el contenido del fichero de benchmarking asi como de la sálida que produce.</sup>
El benchmark reporta el tiempo en espacio de usuario, el tiempo en modo kernel, la suma de los dos y finalmente el tiempo total que le ha llevado al benchmark ejecutarse. Tomamos como buena la suma de tiempo usuario+kernel que sin paravirtualización y utilizando Qemu en Lubuntu tarda:
225s en ejecutarse el benchmark.

![img](https://i.sli.mg/FIjZaj.png)
<sup>Sálida un poco más detallada en la que se puede ver el espacio que ocupta el directorio creado, que ficheros contiene este y el contenido de estos ficheros</sup>

A continuación ejecutamos el benchmark en la misma máquina virtual Lubuntu pero ahora ejecutamos Qemu indicandole que se use paravirtualización:

![img](https://i.sli.mg/Oj1MLD.png)

Ahora el benchmark tarda unos 235s es decir 10 segundos más


Viendo esta salida se puede decir que en principio parece poco relevante si se utiliza paravirtualización o no ya que 10s es relativamente poco sencillamente implica que utilizando le ejecución del benchmark utilizando paravirtualización es un 4% más lenta que sin ella pero para sacar resultados más concluyentes habría que ejecutar el benchmark con con cantidades mayores (por ejemplo que creara un directorio de 1GB) y hacer múltimples ejecuciones para verificar si este 4% se mantiene.

## Ejercicio nº4.

Nos descargamos e instalamos Lubuntu distribución de linux con LXCE como entorno de escritorio. Le ponemos el techo de RAM a 512MB:

![img](https://i.sli.mg/kkCbSs.png)

Seguimos los pasos típicos de instalación y una vez que está instalada le tenemos que modificar la configuración de red que utiliza ya que por defecto no hay conectividad maquina guest-host y viceversa. 
En mi ordenador en particular siempre he tenido problemas con la configuración de red para las máquinas virtuales asi que para poder instalar lo necesario para vnc y ssh temporalmente se la pongo como bridged (lo cual significa que la máquina virtual aparece en la red wifi como si fuera otro ordenador más de la casa) si no se la pusiera bridge tendría que ponersela a NAT o similar y después poner a mano en los ficheros de configuración una ip estática, máscara, red...:

![img](https://i.sli.mg/TuwGyb.png)

Una vez hecho esto compruebo su ip con ifconfig, instalo openssh-server utilizando el comando:
```
sudo apt-get install opengssh-server
```

Y en el host si no tuviera un par de claves privada/pública las genero con la orden:

```
ssh-keygen -t rsa -b 4096 -C "loquesea"
```

y le paso la clave pública del host al guest para que este las guarde en su almacen de claves ssh autorizadas a iniciar sesión en el sistema con la orden:

```
ssh-copy-id -i .ssh/miclave.pub  usuario_remoto@ip_remota

```

y nos conectamos utilizando:

```
ssh usuario_remoto@ip_remota

```

![img](https://i.sli.mg/CVFse6.png)


Para utilizar VNC necesitamos en el host un cliente VNC y en el guest un servidor VNC, como cliente VNC he utilizado vinagre:

```

sudo apt-get install vinagre
```

en el host instalamos un servidor VNC:

```
sudo apt-get install x11vnc
```

a continuación ejecutamos x11vnc en nuestra máquina virtual Lubuntu y en el host ejecutamos vinagre, introduciendo la IP de la máquina Lubuntu:

![img](https://i.sli.mg/sKH1GL.png)

Al principio pensé que el acceso directo a ver el escritorio remoto de la máquina virtual sin pedir contraseña ni nada se debía a que teníamos permiso para loguearnos vía public key utilizando ssh en la máquina Lubuntu. Tras comprobar en otro ordenador diferente que podía conectarme a la máquina virtual Lubuntu sin tener la clave pública autorizada para ese ordenador ni nada solo necesitando la IP de la máquina Lubuntu vi que la sesión vnc de la máquina virtual Lubuntu estaba "abierta al mundo" (si tuviera los puertos del router abiertos claro).
 Posiblmente haya un mecanismo para al igual que con ssh solo deje pasar clientes ssh con una clave pública autorizada en VNC tiene que haber algún mecanismo similar.
## Ejercicio nº6.


Nos descargamos el .iso de la página oficial de Linux Mint y creamos con Virtualbox tal cual hemos hecho en el segundo ejercicio para lubuntu, es decir le damos nombre, seleccionamos el tope de RAM queremos que consuma, el tamaño de disco...

![img](https://i.sli.mg/GPjWVk.png)
<sup>Tras finalizar la instalación</sup>
Seguimos los pasos que nos indican en la instalción y reiniciamos.


![img](https://i.sli.mg/PzArPG.png)

