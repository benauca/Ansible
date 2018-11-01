# INVENTARIOS

Hay dos tipos de inventarios en Ansible
- Estático
- Dinámico

# Inventario Estático

Formato: INI
Nombre: Clave=Valor

En ansible podemos definir Grupos de servidores. Los grupos se especifican entre corchetes
[NOMBRE-GRUPO]
    server1
    server2

```
    # Ex 2: A collection of hosts belonging to the 'webservers' group
    [webservers]
        alpha.example.org
        beta.example.org
        192.168.1.100
        192.168.1.110
```
De esta manera podemos ejecutar comandos ansible a traves de la ip, ya sea fija o localhost
> ansible ip -m ping

o para todos los servidores de un mismo grupo

> ansible group -m ping

``` 
ansible local -m ping
localhost | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
```

Un mismo servidor puede estar asociado a distintos grupos. No son exclusivos.

Además dentro de un mismo grupo , pueden existir subgrupos.
[group:children]
 

Supongamos 
[debian]
    server1
[ubuntu]
    server2
[apt:childre]
    debian
    ubuntu

Además un grupo puede tener variables específicas.

[grupo:vars]
Las variables tienen la siguiente prioridad
    1- HOST
    2- SUBGRUPO
    3- Grupo Padre

> [apt: vars] \n
> ansible_become=true

Es posible separar las variables por ficheros.

> /etc/ansible/group_vars/group1
> /etc/ansible/host_vars/localhost

# Sintaxis
Como podemos ver en el fichero host por defecto que viene en la instalación de ansible, podemos usar la siguiente manera de referirnos a los grupos

```
# If you have multiple hosts following a pattern you can specify
# them like this:

## www[001:006].example.com
```

esto es similar que tener un grupo
www[001:006].example.com

    www001.example.com
    web002.example.com
    web003.example.com
    ..................
    ..................
    web006.example.com

Tambié es válida 
> docker-[a:d]benauca.com

En ansible, por defecto se trabaja con el inventario /etc/ansible/host

Sin embargo es posible trabajar con otro inventario con la opción "-i"

> ansible -i /home/vagrant/host 


# Parámetros
ansible_connection
    -ssh
        ansible_host
        ansible_port
        ansible_user
        ansible_ssh_private_key_file 
    -local

# ALIAS

En ansible se pueden crear alias para en lugar de trabajar con IPs, trabajar con alias. Basta con crear un alias asociado a la IP.

>[debian]
>alias ansible_host=192.168.33.6 ansible_user=vagrant

De esta manera podríamos ejecutar 
```ansible alias -m ping``` 

# INVENTARIO DINAMICO

Hasta ahora hemos visto como podemos trabajar con Máquinas fijas. Pero que pasa si queremos trabajar con distintos proveedores de cloud, ya sean AWS, GCP, open-shift.

Ansible provee de unos script desarrollados en Python alojados en Github https://github.com/ansible/ansible/contrib/inventory con lo s que podremos automizar los distintos entornos pero a traves de scripts.

Existen inventario para todo lo que necesitamos,
- docker
- open-shift
- azure
- ......

> ansible -i /gce.py all -ping


