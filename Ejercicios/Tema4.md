# Tema 4

## Ejercicio 1. Instala LXC en tu versión de Linux favorita. Normalmente la versión en desarrollo, disponible tanto en GitHub como en el sitio web está bastante más avanzada; para evitar problemas sobre todo con las herramientas que vamos a ver más adelante, conviene que te instales la última versión y si es posible una igual o mayor a la 1.0.

Realizamos un ```sudo apt-get install lxc```


![alt text](http://i65.tinypic.com/155ihkn.png)


## Ejercicio 2. Comprobar qué interfaces puente se han creado y explicarlos.

Abrimos el contenedor creado anteriormente con ```sudo lxc-start -n nubecilla```.

Realizamos un ifconfig -a y obtenemos lo siguiente:

```
eth0      Link encap:Ethernet  HWaddr 00:16:3e:c7:75:b1  
          inet addr:10.0.3.67  Bcast:10.0.3.255  Mask:255.255.255.0
          inet6 addr: fe80::216:3eff:fec7:75b1/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:84 errors:0 dropped:0 overruns:0 frame:0
          TX packets:65 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:15119 (15.1 KB)  TX bytes:6846 (6.8 KB)

lo        Link encap:Local Loopback  
          inet addr:127.0.0.1  Mask:255.0.0.0
          inet6 addr: ::1/128 Scope:Host
          UP LOOPBACK RUNNING  MTU:65536  Metric:1
          RX packets:16 errors:0 dropped:0 overruns:0 frame:0
          TX packets:16 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0
          RX bytes:1296 (1.2 KB)  TX bytes:1296 (1.2 KB)

```
Son las típicas que encontramos en cualquer sistema. Si lo hacemos en el anfitrión si encontraremos un interfaz específica para conectar con estos contenedores:

```
lxcbr0    Link encap:Ethernet  direcciónHW fe:9c:f4:c6:f1:c8  
          Direc. inet:10.0.3.1  Difus.:10.0.3.255  Másc:255.255.255.0
          Dirección inet6: fe80::ecfd:f8ff:feaa:42b5/64 Alcance:Enlace
          ACTIVO DIFUSIÓN FUNCIONANDO MULTICAST  MTU:1500  Métrica:1
          Paquetes RX:139 errores:0 perdidos:0 overruns:0 frame:0
          Paquetes TX:195 errores:0 perdidos:0 overruns:0 carrier:0
          colisiones:0 long.colaTX:0
          Bytes RX:12561 (12.5 KB)  TX bytes:37164 (37.1 KB)

```

## Ejercicio 3.

    Crear y ejecutar un contenedor basado en Debian.

    Crear y ejecutar un contenedor basado en otra distribución, tal como Fedora. Nota En general, crear un contenedor basado en tu distribución y otro basado en otra que no sea la tuya. Fedora, al parecer, tiene problemas si estás en Ubuntu 13.04 o superior, así que en tal caso usa cualquier otra distro.


Crearemos un contenedor basado en Debian con la orden ```sudo lxc-create -t debian -n debian_c```.

Para accede a él tan solo necesitaremos las siguientes órdenes:

```
sudo lxc-start -n debian_c -d
sudo lxc-console -n debian_c
```

Ahora crearemos un contenedor basado en CentOS. Para ello usaremos ```sudo lxc-create -t centos -n centos_c```. Decir que se necesita tener yum instalado para que no nos aparezca ningún error. La forma de inciarlo sería igual que en Debian.

## Ejercicio 4.

    Instalar lxc-webpanel y usarlo para arrancar, parar y visualizar las máquinas virtuales que se tengan instaladas.

    Desde el panel restringir los recursos que pueden usar: CPU shares, CPUs que se pueden usar (en sistemas multinúcleo) o cantidad de memoria.

Para instalar lxc-webpanel simplemente introduciremos ```wget https://lxc-webpanel.github.io/tools/install.sh -O - | bash```.

Para acceder a él introduciremos en nuestro navegador ```http://localhost:5000/```. El usuario es admin y la contraseña también. No aparecerá lo siguiente:

![alt text](http://i68.tinypic.com/290zejd.png)

Para restringir recursos pinchamos sobre el contenedor y nos aparecerán diversas opciones para modificar sus parámetros.

![alt text](http://i64.tinypic.com/34ep282.png)


## Ejercicio 5.

    Comparar las prestaciones de un servidor web en una jaula y el mismo servidor en un contenedor. Usar nginx.

Para la comparación nos es necesario usar debootstrap. Esto lo hacemos mediante ```sudo apt-get install debootstrap```.

La jaula la crearemos mediante ```sudo debootstrap --arch=amd64 xenial /home/jaula_ubuntu http://archive.ubuntu.com/ubuntu```.

Accedemos a la jaula con ```sudo chroot /home/jaula_ubuntu```. Necesitaremos wget lo cual lo conseguiremos con ```apt-get -y install wget```.

Descargamos la clave de nginx con ```wget http://nginx.org/keys/nginx_signing.key```.

Añadimos la clave ```apt-key add nginx_signing.key```.

Ahora añadimos el repositorio:

```
echo "deb http://nginx.org/packages/ubuntu/ raring nginx" >> /etc/apt/sources.list echo "deb-src http://nginx.org/packages/ubuntu/ raring nginx" >> /etc/apt/sources.list
```

Ahora ya podemos instalar nginx y curl ```apt-get install nginx curl```.

Probablemente el puerto que usa nginx esté en uso y debemos modificarlo en el fichero /etc/nginx/sites-enabled/default.

Si hacemos un curl 127.0.0.1:8080 nos debe aparecer lo siguiente:

```
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
    body {
        width: 35em;
        margin: 0 auto;
        font-family: Tahoma, Verdana, Arial, sans-serif;
    }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>

```

Ahora accedemos a nuestro contenedor lxc (nubecilla por ejemplo) e instalamos nginx.

Instalamos Siege en el contenedor y en la jaula con ```sudo apt-get install siege```.

Realizamos ```siege -b -c 1000 -t 20S 127.0.0.1``` indicando el puerto 8080 en la jaula.

En la jaula obtenemos:

```
Transactions:		      328242 hits
Availability:		       99.99 %
Elapsed time:		       19.91 secs
Data transferred:	      120.21 MB
Response time:		        0.02 secs
Transaction rate:	    16486.29 trans/sec
Throughput:		        6.04 MB/sec
Concurrency:		      348.13
Successful transactions:      328242
Failed transactions:	          36
Longest transaction:	       18.78
Shortest transaction:	        0.00


```

Mientras tanto en el contenedor tenemos que:
```
Transactions:		      569449 hits
Availability:		      100.00 %
Elapsed time:		       19.64 secs
Data transferred:	      208.54 MB
Response time:		        0.03 secs
Transaction rate:	    28994.35 trans/sec
Throughput:		       10.62 MB/sec
Concurrency:		      821.39
Successful transactions:      569449
Failed transactions:	           0
Longest transaction:	        4.61
Shortest transaction:	        0.00


```

Como conclusión obtenemos que el contenedor puede procesar más peticiones por segundo y que no nos muestra problema alguno en ninguna de ellas.

## Ejercicio 6.

    Instalar docker.

Lo primero que debemos hacer es introducir ``` sudo apt-get install docker.io```.

Para evitar introducir sudo indicamos lo siguiente: ```sudo usermod -aG docker $(whoami)```.

Activamos el demonio con ```sudo nohup docker daemon -H tcp://0.0.0.0:2375 -H unix:///var/run/docker.sock &```

Para probar su funcionamiento probamos con docker run hello-world. Obtenemos:

```
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world

c04b14da8d14: Pull complete
Digest: sha256:0256e8a36e2070f7bf2d0b0763dbabdd67798512411de4cdcf9431a1feb60fd9
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker Hub account:
 https://hub.docker.com

For more examples and ideas, visit:
 https://docs.docker.com/engine/userguide/

```


## Ejercicio 7.

    Instalar a partir de docker una imagen alternativa de Ubuntu y alguna adicional, por ejemplo de CentOS.  

    Buscar e instalar una imagen que incluya MongoDB.

Para usar Ubuntu simplemente introducimos ```sudo docker pull ubuntu```.

Si queremos usar CentOS sería ```sudo docker pull centos:7```.

Para usar MongoDB buscamos la biblioteca con ```sudo docker search mongo```.

A continuación descagamos la biblioteca ```sudo docker pull mongo```.

## Ejercicio 8.

    Crear un usuario propio e instalar nginx en el contenedor creado de esta forma.

Iniciamos el contenedor con Ubuntu con ```sudo docker run -i -t ubuntu```.

Ahora debemos crear un usuario en el contenedor. Esto lo llevamos a cabo con ```useradd -d /home/usuario -m usuario```. Le indicamos su contraseña con passwd, lo hacemos administrador e iniciamos sesión con él.

En Ubuntu 16.04 no nos funcionará sudo por lo que debemos activarlo previamente con root mediante ```apt-get update && apt-get install -y sudo && rm -rf /var/lib/apt/lists/*```.

Ya podemos instalar nginx con ```sudo apt-get install nginx```.

```
usuario@fb3673f1d170:~$ curl localhost
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
    body {
        width: 35em;
        margin: 0 auto;
        font-family: Tahoma, Verdana, Arial, sans-serif;
    }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
usuario@fb3673f1d170:~$
```

## Ejercicio 9.

    Crear a partir del contenedor anterior una imagen persistente con commit.

Lo primero que hacemos es buscar el id con ```sudo docker ps -a```.

![alt text](http://oi63.tinypic.com/2wpnm2v.jpg)

Comprobamos que existe la id con ```sudo docker inspect fb3673f1d170```. Nos debe mostrar lo siguiente:

```
[
    {
        "Id": "fb3673f1d170bd254ff704aee6e4590e59b6fcf30d83c11a21336a18a43f6976",
        "Created": "2016-11-29T15:54:06.63701729Z",
        "Path": "/bin/bash",
        "Args": [],
        "State": {
            "Status": "running",
            "Running": true,
            "Paused": false,
            "Restarting": false,
            "OOMKilled": false,
            "Dead": false,
            "Pid": 20125,
            "ExitCode": 0,
            "Error": "",
            "StartedAt": "2016-11-29T15:54:08.084698668Z",
            "FinishedAt": "0001-01-01T00:00:00Z"
        },
        "Image": "sha256:e4415b714b624040f19f45994b51daed5cbdb00e0eb9a07221ff0bd6bcf55ed7",
        "ResolvConfPath": "/var/lib/docker/containers/fb3673f1d170bd254ff704aee6e4590e59b6fcf30d83c11a21336a18a43f6976/resolv.conf",
        "HostnamePath": "/var/lib/docker/containers/fb3673f1d170bd254ff704aee6e4590e59b6fcf30d83c11a21336a18a43f6976/hostname",
        "HostsPath": "/var/lib/docker/containers/fb3673f1d170bd254ff704aee6e4590e59b6fcf30d83c11a21336a18a43f6976/hosts",
        "LogPath": "/var/lib/docker/containers/fb3673f1d170bd254ff704aee6e4590e59b6fcf30d83c11a21336a18a43f6976/fb3673f1d170bd254ff704aee6e4590e59b6fcf30d83c11a21336a18a43f6976-json.log",
        "Name": "/romantic_turing",
        "RestartCount": 0,
        "Driver": "aufs",
        "MountLabel": "",
        "ProcessLabel": "",
        "AppArmorProfile": "",
        "ExecIDs": null,
        "HostConfig": {
            "Binds": null,
            "ContainerIDFile": "",
            "LogConfig": {
                "Type": "json-file",
                "Config": {}
            },
            "NetworkMode": "default",
            "PortBindings": {},
            "RestartPolicy": {
                "Name": "no",
                "MaximumRetryCount": 0
            },

        "NetworkSettings": {
            "Bridge": "",
            "SandboxID": "c3bce85010962c0720f34cd1b936d93cd694c7061e5da84dbabc6bb3077f63c8",
            "HairpinMode": false,
            "LinkLocalIPv6Address": "",
            "LinkLocalIPv6PrefixLen": 0,
            "Ports": {},
            "SandboxKey": "/var/run/docker/netns/c3bce8501096",
            "SecondaryIPAddresses": null,
            "SecondaryIPv6Addresses": null,
            "EndpointID": "cc7aae89215fc0d047e50ae369d870ff39a61705352074e0a2f834cc0e43c8f1",
            "Gateway": "172.17.0.1",
            "GlobalIPv6Address": "",
            "GlobalIPv6PrefixLen": 0,
            "IPAddress": "172.17.0.2",
            "IPPrefixLen": 16,
            "IPv6Gateway": "",
            "MacAddress": "02:42:ac:11:00:02",
            "Networks": {
                "bridge": {
                    "IPAMConfig": null,
                    "Links": null,
                    "Aliases": null,
                    "NetworkID": "e11cb76e734be848a553f1babe135ee0a68fcfebbd1c1443155e4f9ca36a8966",
                    "EndpointID": "cc7aae89215fc0d047e50ae369d870ff39a61705352074e0a2f834cc0e43c8f1",
                    "Gateway": "172.17.0.1",
                    "IPAddress": "172.17.0.2",
                    "IPPrefixLen": 16,
                    "IPv6Gateway": "",
                    "GlobalIPv6Address": "",
                    "GlobalIPv6PrefixLen": 0,
                    "MacAddress": "02:42:ac:11:00:02"
                }
            }
        }
    }
]
```
Cogemos el id ```fb3673f1d170bd254ff704aee6e4590e59b6fcf30d83c11a21336a18a43f6976```. Lo introducimos de la siguiente forma ```sudo docker commit fb3673f1d170bd254ff704aee6e4590e59b6fcf30d83c11a21336a18a43f6976 commit_ubuntu```.


## Ejercicio 10.

    Crear una imagen con las herramientas necesarias para el proyecto de la asignatura sobre un sistema operativo de tu elección.

Este ejercicio está enlazado en el siguiente enlace: [Entorno de pruebas](https://github.com/jfranguerrero/IV/blob/Documentacion/README.md#hito-4-entorno-de-pruebas)
