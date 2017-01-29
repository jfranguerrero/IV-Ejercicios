# Tema 5

## Ejercicio 1. Instalar los paquetes necesarios para usar KVM.

Para instalar KVM debemos introducir lo siguiente ```sudo apt-get install qemu-kvm libvirt-bin```.

Para comprobar que nuestro ordenador es válido haremos sudo kvm-ok.
Si es válido como en nuestro caso nos devolvera:

```
INFO: /dev/kvm exists
KVM acceleration can be used

```


## Ejercicio 2.

    1. Crear varias máquinas virtuales con algún sistema operativo libre tal como Linux o BSD. Si se quieren distribuciones que ocupen poco espacio con el objetivo principalmente de hacer pruebas se puede usar CoreOS (que sirve como soporte para Docker) GALPon Minino, hecha en Galicia para el mundo, Damn Small Linux, SliTaz (que cabe en 35 megas) y ttylinux (basado en línea de órdenes solo).

    2. Hacer un ejercicio equivalente usando otro hipervisor como Xen, VirtualBox o Parallels.


Vamos a instalar CoreOS. Para ello lo primero que hacemos es crear un disco duro. Con 8GB será suficiente.

```
qemu-img create -f qcow2 coreos.qcow2 8G

```

Descargamos una versión oficial de CoreOS. La descargada pesa 267MB.

Realizamos su instalación con:

```
qemu-system-x86_64 -machine accel=kvm -hda coreos.qcow2 -cdrom coreos_production_iso_image.iso -m 1G -boot d

```

Automáticamente comenzará a instalarse. Concluida la instalación se ejecutará.

![alt text](http://i63.tinypic.com/o6ip2f.png)

La siguiente máquina que instalaremos será Debian. Como en el caso anterior crearemos un disco duro.
```
qemu-img create -f qcow2 debian.qcow 2G
```

Descargaremos la imagen de Debian del siguiente enlace http://cdimage.debian.org/cdimage/daily-builds/daily/arch-latest/i386/iso-cd/

Concluido esto crearemos la máquina con:
```
qemu-system-x86_64 -machine accel=kvm -hda debian.qcow -cdrom debian-testing-i386-netinst.iso -boot d -m 256
```
Aparecerá la ventana de instalación.

![alt text](http://www.subeimagenes.com/img/captura-de-pantalla-de-2017-01-19-00-13-49-1696316.png)

Deberemos seguir los pasos de la instación de la misma forma que si se tratase de una máquina física.

Lanzaremos la máquina mediante:
```
qemu-system-x86_64 -hda debian.qcow -m 256
```
![alt text](http://www.subeimagenes.com/img/captura-de-pantalla-de-2017-01-19-00-32-33-1696340.png)


Ahora instalaremos ttylinux en VirtualBox. Descargamos la imagen de http://osarchive.sda1.eu/ttylinux.

Vamos a la opción de crear la máquina virtual y le indicamos que es Linux y 64 bits.

![alt text](http://www.subeimagenes.com/img/captura-de-pantalla-de-2017-01-19-01-11-16-1696366.png)

Le introducimos el tamaño de memoria RAM necesario para la máquina.

![alt text](http://www.subeimagenes.com/img/captura-de-pantalla-de-2017-01-19-01-12-06-1696368.png)

Creamos un disco virtual para almacenar la información.

![alt text](http://www.subeimagenes.com/img/captura-de-pantalla-de-2017-01-19-01-13-09-1696375.png)

Desde la configuración de la máquina seleccionamos la ISO descargada para poder instalarla.

![alt text](http://www.subeimagenes.com/img/captura-de-pantalla-de-2017-01-19-01-25-40-1696382.png)

Arrancamos la máquina y ya la tenemos lista para trabajar.

![alt text](http://www.subeimagenes.com/img/captura-de-pantalla-de-2017-01-19-01-29-00-1696384.png)



## Ejercicio 4. Crear una máquina virtual Linux con 512 megas de RAM y entorno gráfico LXDE a la que se pueda acceder mediante VNC y ssh.

Se va a instalar Linux Mint con LXDE. Lo descargaremos de la siguiente url: https://www.linuxmint.com/edition.php?id=98

Creamos un disco duro para instalar Linux Mint mediante:
```
qemu-img create -f qcow2 mint.qcow 3G

```
Crearemos la máquina indicándole la imagen, el disco duro y el tamaño de memoria RAM.
```
qemu-system-x86_64 -hda mint.qcow -cdrom linuxmint-12-lxde-cd-32bit.iso -m 512M
```

Se nos instalará automáticamente y nos llevará a la terminal.

![alt text](http://www.subeimagenes.com/img/captura-de-pantalla-de-2017-01-19-01-44-55-1696398.png)

Para acceder por VNC podemos usar Vinagre. Lo instalamos con ```sudo apt-get install vinagre```.

Lanzaremos la máquina de forma que tenga VNC activado.

```
qemu-system-x86_64 -hda mint.qcow -cdrom linuxmint-12-lxde-cd-32bit.iso -m 512M -vnc :1
```

Ya solo nos queda acceder a la máquina de forma remota con ```vinagre localhost:1```.

![alt text](http://www.subeimagenes.com/img/captura-de-pantalla-de-2017-01-19-02-01-26-1696426.png)

Para poder conectar mediante ssh deberemos arrancar la máquina con esa opción activada mediante:
```
qemu-system-x86_64 -hda mint.qcow -cdrom linuxmint-12-lxde-cd-32bit.iso -m 512M -redir tcp:2222:22
```

Algunas versiones traen ssh instalado, si no es el caso deberemos introducir en la mñaquina virtual ```sudo apt-get install ssh```.

Ya podremos entrar desde el sistema anfitrión mediante ssh indicando el puerto 2222.


## Ejercicio 6. Instalar una máquina virtual con Linux Mint para el hipervisor que tengas instalado.

Vamos a aprovechar la imagen de Linux Mint que teníamos descargada en un ejercicio anterior. La instalaremos en VirtualBox que es uno de los hipervisores que tenemos en nuestro sistema.

Vamos a la opción de nueva máquina y elegimos que es Linux y 32bits.

![alt text](http://www.subeimagenes.com/img/captura-de-pantalla-de-2017-01-19-02-16-49-1696454.png)

Le indicamos la memoria RAM que deseamos.

![alt text](http://www.subeimagenes.com/img/captura-de-pantalla-de-2017-01-19-02-18-34-1696458.png)

Para finalizar la creación de la máquina creamos un disco duro donde almacenar la información.

![alt text](http://www.subeimagenes.com/img/captura-de-pantalla-de-2017-01-19-02-19-42-1696462.png)

Desde la configuración seleccionamos la ISO descargada.

![alt text](http://www.subeimagenes.com/img/captura-de-pantalla-de-2017-01-19-02-29-52-1696476.png)

Arrancamos la máquina, seguimos los pasos de la instalación y ya tendremos la máquina lista para funcionar.

![alt text](http://www.subeimagenes.com/img/captura-de-pantalla-de-2017-01-19-03-05-36-1696499.png)
