# VAGRANT

## Instalación Vagrant

## Añadir Una Caja

```
Usage: vagrant box <subcommand> [<args>]

Available subcommands:
     add
     list
     outdated
     prune
     remove
     repackage
     update

For help on any individual subcommand run `vagrant box <subcommand> -h`
```

```
$ vagrant box add generic/centos7

```


Una vez tenemos nuestra caja descagarda, podremos crear nuestro primer fichero VagrantFile y arrancar una máquina virtual con ese Centos7 donde podremos instalar nuestro ansible, y comenzar a jugar


## VagrantFile

Una vez descargada nuestra primera imagen, tan sólo necesitamos arrancar nuestra primera virtualización

Para ello deberemos crear nuestro primer fichero VagrantFile

````
Vagrant.configure("2") do |config|
  config.vm.box = "generic/centos7"
end
````


Y para iniciarlo basta con ejecutar

```` 

$> vagrant init generic/centos7
$> vagrant up

````

## Conexión SSH
 Una vez iniciada la máquina ya podemos conectarnos. Basta ejecutar el comando

 ````
$> vagrant ssh
````

