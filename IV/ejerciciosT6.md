#Ejercicios. Tema 6.


## Ejercicio nº1.

### Instalar chef.
Instalamos por la via rápida chef:

```

curl -L https://www.opscode.com/chef/install.sh | bash
```

![img](https://i.sli.mg/vClNry.png)

## Ejercicio nº2.

Para poder instalar la aplicación de la asignatura utilizando chef necesitamos crear tres ficheros, el primero de ellos la receta donde especificamos las acciones que queremos que realiza chef:

```ruby
package 'git'
package 'ruby'
package 'ruby-dev'
package 'build-essential'
package 'libpq-dev'
gem_package 'bundler'

directory '/home/l/Documents/queveobot' do
	action :create
end


execute "clonar directorio" do
	cwd "/home/l/Documents/queveobot"
	command  "git clone https://github.com/LuisGi93/proyectoIV2016-2017.git"
	action :run
end

execute "instalamos dependencias bot" do
	cwd "/home/l/Documents/queveobot/proyectoIV2016-2017"
	command  "bundle  install"
	action :run
end

```

Esta receta instala los paquetes: git, ruby, ruby-dev, build-essential, pibpq-dev.., la gema bundler para a continuación clonar el repositorio del proyecto utilizando el comando git clone y para acabar instala todas las dependencias del proyecto utilizando el comando bundle install.

![img](https://i.sli.mg/AlHMbx.png)


## Ejercicio nº3.

En este ejercicio vamos a desplegar nuestra aplicación de DAI utilizando Ansible utilizando un playbook de Ansible. 
La aplicación de DAI consiste en una web que consta básicamente de tres partes:
* los ficheros que la componen 
* un servidor de aplicaciones
* un servidor web
* una base de datos mongo

El proceso de despliegue podemos dividirlo en los siguientes pasos:

1. Instalación de los paquetes del sistema operativo que necesita nuestra aplicación.
2. Crear entorno aislado para nuestra aplicación.
3. Instalar las librerias de python que utiliza nuestra aplicación.
4. Configurar el servidor web para que actue como proxy entre internet y Gunicorn.
5. Configurar la base de datos tipo mongo.

El playbook resultante es el siguiente:

```yaml
---
- hosts: vmDAI
  user: root
  tasks:
      
       
  -  name: Actualizamos el repositorio
     apt: update_cache=yes
  -  name: Instalamos  paquetes necesarios
     action: >
      {{ ansible_pkg_mgr }} name={{ item }} state=installed update_cache=yes
     with_items:
      - build-essential
      - libxml2-dev
      - python-dev
      - libxslt-dev
      - libffi-dev
      - libssl-dev
      - virtualenv
      - nginx
      - git
      - mongodb 
 

    
  -  name: creamos entorno virtualenv
     become: true  
     become_user: l   
     shell: virtualenv DAI   
     args:
       chdir: /home/l/     
       creates: /home/l/DAI


  -  name: clonamos repo de la web
     shell: git clone https://github.com/LuisGi93/DAI-2016-2017.git
     become: true  
     become_user: l    
     args:
       chdir: /home/l/DAI     
       creates: /home/l/DAI/DAI-2016-2017
       executable: /bin/bash
                                 
       
  -  name: Inslamos el proyecto
     become: true  
     become_user: l 
     shell: /home/l/DAI/bin/python /home/l/DAI/DAI-2016-2017/setup.py install 

     
         
              
  -  file:
        path: /etc/nginx/sites-available/criticas
        state: touch
        
  -  name: Creamos fichero de configuracion
     blockinfile:
       dest: /etc/nginx/sites-available/criticas
       block: |
        server { 
             listen 80 default_server;
             location / {
                include proxy_params;
                proxy_pass http://127.0.0.1:8000;
              }
         }        
    
  -  name: Habilitamos la web con nginx
     shell: ln -s /etc/nginx/sites-available/criticas /etc/nginx/sites-enabled/criticas
     args:
       creates: /etc/nginx/sites-enabled/criticas

  -  file:
        path: /home/l/DAI/DAI-2016-2017/start.sh 
        state: touch


  -  name: Creamos script de arranque
     blockinfile:
       dest: /home/l/DAI/DAI-2016-2017/start.sh 
       block: |
         #!/bin/bash
         export CONSUMER_KEY=--------;
         export CONSUMER_SECRET=------;
         export ACCESS_TOKEN=-----;  
         export ACCESS_TOKEN_SECRET=------;
         gunicorn --bind 0.0.0.0:8000 app:app
        



  -  name: Arrancamos la base de datos
     shell: "{{ item }} "
     with_items:
      - wget https://raw.githubusercontent.com/mongodb/docs-assets/primer-dataset/primer-dataset.json
     args:
       chdir: /home/l/DAI/DAI-2016-2017 
       creates: /home/l/DAI/DAI-2016-2017/primer-dataset.json
       
  -  name: Lanzamos la base de datos
     shell: "{{ item }} "
     with_items:
      -  mkdir -p log db         
      -  mongod --fork --dbpath db --smallfiles --logpath /home/l/DAI/DAI-2016-2017/log/mogodb.log    
       
     args:
       chdir: /home/l/DAI/DAI-2016-2017 
   

      
  -  name: Habilitamos la web con nginx
     shell: rm -f /etc/nginx/sites-enabled/default

              
  -  service:
       name: nginx
       state: restarted


     
     
```

De manera resumida realiza los 5 pasos descritos anteriormente utiliznado los siguiente módulos:

1. Módulo apt: actualiza el repositorio antes de instalar nada.
2. Módulo action con una lista de paquetes a instalar, los va instalando secuencialmente.

3. Módulo shell:  ejecuta literalmente el comando que tiene a su derecha utilizando este módulo para:
	* crear el entorno virtualenv.
	* clonar el repositorio
	* instalar las librerias de python en el entorno virtualenv utilizando setuptools de python.
	* Arrancar la base datos, descargarnos los datos de prueba que utiliza y rellenarla con ellos.
4. Módulo file: para crear los ficheros de configuración.
5. Módulo blockinfile: cuando deseamos que un fichero de configuración especificado por la etiqueta "dest" tenga un contenido determinado escribimos dentro del apartado "block: | " lo que queremos literalmente que contenga.
6. Módulo service: podemos controlar el estado que queremos que tengan los servicios del sistema operativo, por ejemplo  en el playbook especifico utilizando la etiqueta "state:restarted" que se reinicie el servidor Nginx. Esta etiqueta también es utilizada en el módulo file para especificar que queremos que se cree un fichero.

Algunas opciones que no he comando y que utlizo  son las ordenes become y beocme_user. Debido a que al comienzo del fichero especifico que quiero que se ejecuten todas las acciones del playbook utilizando al usuario root con become y become_user hago que para la tare en la que estan englobados los become se ejecute como el usuario "l" en lugar de como root. 

Por último comentar la opción "args" con la que defimos características para la tarea a la que pertenece como por ejemplo "chdir" que especifica el "path" en el que se quiere que se ejecute dicha tarea.

Previamente a ejecutar el playbook ha sido necesario  copiar la clave  rsa_pub de la máquina maestro a ~/.ssh/authorized_keys en las esclavos.


## Ejercicio nº4.



Para crear una máquina virtual Debian usando Vagrant no hay más que irse a un directorio cualquiera y ejecutar:

```
vagrant init debian/jessie64;
```
Esto creará un Vagrantfile pudiendo a continuación ejecutar ```vagrant up ````lo cual dará lugar al inicio de la máquina virtual Debian.


![img](https://ibin.co/39DsbFdbGbEb.png)


## Ejercicio nº5.

Seguimos el ejemplo del Vagrantfile que se crea al hacer vagrant up en el ejercicio anterior y lo dejamos asi:

 ```
 VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "debian/jessie64"

  config.vm.provision "shell", path: "nginx.sh"
end

 ```
 
 De tal forma que cuando hagamos vagrant provision vagrant ejecutará en la máquina debian las instrucciones shell contenidas en nnginx.sh. El contenido de nginx.sh para que instale nginx es el siguiente:
 
 ```
    apt-get update
     apt-get install -y nginx

```
Tras lo cual no queda más que ejecutar vagrant provision y ver si efectivamente instala nginx.

![img](https://ibin.co/39DsT8LHPONp.png)


 