# Tema 1

## Ejercicio 1. Consultar en el catálogo de alguna tienda de informática el precio de un ordenador tipo servidor y calcular su coste de amortización a cuatro y siete años.

El servidor elegido es un HP ProLiant ML310e G8 XE E3-1220/8GB/2TB vendido por
tienda online ![PcComponentes](https://www.pccomponentes.com/hp-proliant-ml310e-g8-xe-e3-1220-8gb-2tb).

Aquí cito sus especificaciones:


    Procesador
        Velocidad de reloj* 3.1 GHz
        Modelo del procesador* Intel Xeon (4 núcleos, 3,1 GHz, 8 MB, 69 W)
        Número de núcleos de procesador* 4
        Número de procesadores instalados* 1
        L2 cache 8 MB
    Medios de almacenaje
        Dos unidades de 1TB (2x1TB)
    Reguladores del almacenaje
        Controlador de disco duro (1) Dynamic Smart Array B120i / ZM;
    Memoria
        Memoria interna* UDIMM de 8 GB
        Memoria interna máxima* 32 GB
        Tipo de memoria interna* 2R x8 PC3L-10600E-9
        Ranuras de memoria* 4 ranuras DIMM; Máximo
    Impulsión óptica
        Tipo de unidad óptica Unidad DVD-ROM JackBlack SATA de media altura
    Red
        Tarjetas de red Adaptador Ethernet 330i de 1 Gb y 2 puertos por controlador
    Conectividad
        Ranuras de expansión (4) máximo: para obtener una descripción detallada, consulte QuickSpec
    Control de energía
        Tipo de fuente de alimentación (1) kit de fuente de alimentación integrada de fábrica de 350 W con varias salidas
    Sistema operativo/software
        Software de gestión remota iLO estándar, Insight Control (opcional)
    Seguridad
        Funciones de protección de datos ECC sin búfer; ECC
    Detalles técnicos
        Tipo de chasis * Torre
        Factor de forma Rack (4U)
        Conexión de almacenamiento estándar SATA de 3,5 pulgadas LFF sin conexión en caliente; SAS de 3,5 pulgadas LFF sin conexión en caliente; SAS de 2,5 pulgadas SFF con conexión en caliente; SATA de 2,5 pulgadas SFF con conexión en caliente; SSD de 2,5 pulgadas SFF con conexión en caliente
        Unidad de disco (4) SAS/SATA/SSD LFF o; (8) SAS/SATA/SSD SFF; Conexión en caliente y/o sin conexión en caliente, según modelo
    Otras características
        Dimensiones (Ancho x Profundidad x Altura) 175 x 475.2 x 368.2 mm
        Peso 18.96 kg


El precio del servidor elegido en PcComponentes es de 769 €. Dicho precio lo usaremos
para calcular el coste de amortización en 4 y en 7 años.
El coeficiente máximo anual que pondemos amortizar es del 25% del precio sin IVA
por lo que lo realizaremos de esta forma.

## Amortización a 4 años

El coste sin IVA es de 635,54€. Por tanto pagando un 25% cada año pagaríamos un
coste de 158,89€ de forma anual.


## Amortización a 7 años

El coste sin IVA es de 635,54€. Pagándolo con igual porcentaje cada año hasta
amortizarlo completamente deberemos aportar 90,80€ cada año.


## Ejercicio 2. Usando las tablas de precios de servicios de alojamiento en Internet y de proveedores de servicios en la nube, Comparar el coste durante un año de un ordenador con un procesador estándar (escogerlo de forma que sea el mismo tipo de procesador en los dos vendedores) y con el resto de las características similares (tamaño de disco duro equivalente a transferencia de disco duro) en el caso de que la infraestructura comprada se usa sólo el 1% o el 10% del tiempo.

Para la comparación se usará un servidor dedicado de la empresa OVH que poseen
precios bastantes contenidos y el servicio Microsoft Azure.

Las características del dedicado de OVH son las siguientes:

    Procesador: 	Intel Xeon
    E5-2630v3
    8c/16t 2.4 GHz/3.2 GHz
    RAM: 	64 GB DDR4 ECC 1866 MHz
    Discos: 	2x 4 TB SAS

Este producto tiene un coste de 219,99€ al mes más IVA.


A su vez éstas son las características del servicio que nos ofrece Azure:

    Procesador: 	Intel Xeon
    E5-2673 v3
    8c 2.0 GHz/3.2 GHz
    RAM: 	56 GB
    Discos: 	605GB

En este caso el coste es de €0,7421/h IVA includo.

### Si lo usamos un 1% del tiempo

En el caso del servidor dedicado de OVH no nos importa el uso del servidor ya que
su coste es fijo. Por tanto su coste sería.

((219,99 x 0,21 ) + 219,99) x 12 = 3194,26€

Con Microsoft Azure tendríamos un coste de:

0,01 x (0,7421 x 24 x 365)= 65,01€

### Si lo usamos un 10% del tiempo

En el servidor dedicado nos ocurre como en el caso anterior por lo que se pagarían
3194,26€.

Con Microsoft Azure tendríamos un coste de:

0,10 x (0,7421 x 24 x 365)= 650,1€

Con esto llegamos a la conclusión que si necesitamos usar un servidor momentos puntuales
sale mucho mas rentable un proveedor de servicios en la nube. Si funcionase las
24h del día un servidor dedicado sería más rentable.

## Ejercicio 3
    ¿Qué tipo de virtualización usarías en cada caso? Comentar en el foro

    Crear un programa simple en cualquier lenguaje interpretado para Linux,
    empaquetarlo con CDE y probarlo en diferentes distribuciones  

https://github.com/JJ/IV16-17/issues/1


Lo primero que necesitamos es instalar CDE. Esto lo realizaremos mediante sudo apt-get install cde.

Creamos un pequeño código como el siguiente que imprime los 20 primeros números naturales.

![alt tag](http://i65.tinypic.com/s1su15.png)

Para ejecutar el programa usaríamos  cde python test.py.

Tras esto debe haberse generado un directorio llamado cde-package y un fichero cde-options.

![alt tag](http://i66.tinypic.com/fvbuch.png)

Accedemos a ese directorio y ejecutamos el fichero.

```sh
francisco@francisco-Lenovo-IdeaPad-Y510P:~/Escritorio/cde-package/cde-root/home/francisco/Escritorio$ ./python.cde test.py
1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19 20
francisco@francisco-Lenovo-IdeaPad-Y510P:~/Escritorio/cde-package/cde-root/home/francisco/Escritorio$

```

## Ejercicio 4. Comprobar si el procesador o procesadores instalados tienen estos flags. ¿Qué modelo de procesador es? ¿Qué aparece como salida de esa orden?

Para obtener los flags realizamos en nuestra terminal egrep '^flags.\*(vmx|svm)' /proc/cpuinfo.

![alt tag](http://i66.tinypic.com/2hq6iok.png)

Para saber el modelo de procesador hacemos un cat /proc/cpuinfo.

![alt tag](http://i64.tinypic.com/9knot5.png)


## Ejercicio 5
    Comprobar si el núcleo instalado en tu ordenador contiene este módulo del
    kernel usando la orden kvm-ok.

    Instalar un hipervisor para gestionar máquinas virtuales, que más adelante
    se podrá usar en pruebas y ejercicios.

Para ejecutar la orden kvm-ok debemos tener instalado cpu-checker. Lo llevamos a cabo
con apt-get install.

Lo ejecutamos y en mi caso aparece lo siguiente:

```sh
francisco@francisco-Lenovo-IdeaPad-Y510P:~$ kvm-ok
INFO: /dev/kvm exists
KVM acceleration can be used
```
Por tanto si se dispone de ese módulo.

Como hipervisor ya dispongo de VirtualBox junto con Vagrant para instalar imágenes de SO
de forma sencilla. Si queremos instalar KVM simplemente deberíamos introducir lo siguiente:

```sh
    sudo apt-get install qemu-kvm libvirt-bin bridge-utils virt-manager
```
