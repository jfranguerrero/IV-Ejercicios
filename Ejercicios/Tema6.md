# Tema 6

## Ejercicio 1. Instalar chef en la máquina virtual que vayamos a usar

Instalar chef es muy simple. Solo debemos introducir ```sudo gem install ohai chef```.




## Ejercicio 3. Desplegar la aplicación de DAI con todos los módulos necesarios usando un playbook de Ansible.

En la dicumentación del proyecto se ha explicado el desarrollo. [Aquí](https://github.com/jfranguerrero/IV/blob/master/ansible.yml) se enlaza al playbook generado para el proyecto.

## Ejercicio 4. Instalar una máquina virtual Debian usando Vagrant y conectar con ella.

Para instalar Debian Jessie usando Vagrant mediante VirtualBox debemos introducir lo siguiente:

```
vagrant box add debian/jessie64
```

Nos pedirá que le indiquemos si es para libvirt, lxc o virtualbox. Elegimos este último y comenzará a descargarse.

Cuando está descargado para inicializarla le introducimos ```vagrant init debian/jessie64``` y para arrancarla ```vagrant up```.

Para acceder a la máquina ya solo nos queda introducirle ```vagrant ssh```.

![alt text](http://i68.tinypic.com/2mxlb38.png)

En nuestro caso vagrant ya estaba instalado, si se necesita simplemente es ```sudo apt-get install vagrant```.

## Ejercicio 5. Crear un script para provisionar nginx o cualquier otro servidor web que pueda ser útil para alguna otra práctica

Para instalar nginx podemos modificar el vagrantfile introduciendo lo siguiente:

```
Vagrant.configure(2) do |config|
  config.vm.box = "bento/ubuntu-16.04"
  config.vm.provision "shell", path: "nginx.sh"
end
```

El fichero nginx.sh llevará lo necesario para instalar y lanzar nginx.

```
#!/bin/bash

sudo apt-get update
sudo apt-get install -y nginx
sudo service nginx start

```


## Ejercicio 6. Configurar tu máquina virtual usando vagrant con el provisionador chef

Vamos a configurar vagrant con chef. Este lo usaremos para instalar y usar nodejs y mongodb. Deberemos introducir en el vagrantfile lo siguiente:

```

config.vm.provision :shell, :inline => "sudo apt-get update -y"
config.vm.provision :shell, :inline => "sudo apt-get install curl -y"
config.vm.provision :shell, :inline => "curl -L https://www.opscode.com/chef/install.sh | sudo bash"

config.vm.provision :chef_solo do |chef|
¡  chef.cookbooks_path = ["cookbooks"]
  chef.add_recipe 'nodejs'
  chef.add_recipe 'mongodb::default'
  chef.add_recipe 'git'
end

```
Para que funcione deberemos descargar los cookbooks de los paquetes que deseamos instalar, en este caso los indicamos arriba.
