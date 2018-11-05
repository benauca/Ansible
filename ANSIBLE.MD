# ANSIBLE

![alt text](Ansible_Logo.png)

La Wikipedia nos dice lo sisguiente. Ansible Es una plataforma de software libre para configurar y administrar computadoras. Combina instalación multi-nodo, ejecuciones de tareas ad hoc y administración de configuraciones. Adicionalmente, Ansible es categorizado como una herramienta de orquestación.1​ Maneja nodos a través de SSH y no requiere ningún software remoto adicional (excepto Python 2.4 o posterior2​ para instalarlo). Dispone de módulos que trabajan sobre JSON y la salida estándar puede ser escrita en cualquier lenguaje. Nativamente utiliza YAML para describir configuraciones reusables de los sistemas.

Ansible hace fácil la automatización, nos permite automatizar infraestructuras y Aplicaciones. 

Desde Servidores
-   Linux
-   Windows

Pasando por Entornos Cloud
-   AWD
-   GCP 
-   OpenStack
-   WMWare

hasta dispositivos
-   Switchers
-   routers

Permite de una maneras sencilla la instalación y administración de aplicaciones.


Existen otras herramientas similares a Ansible, como CHEF, PUPPET o SALT.  La ventaja que ofrecen este tipo de herramientas es que
- No requiren de la instalación de agentes
- la comunicación entre el servidor Ansible y los clientes siguen los estándar SSH.

```
     _________        SSH        _________
        A          ==========>     Client1  
     _________                   _________
        \
         \ SSH
          \
     __________
      Client 2 
     __________    
```
- Tienen muy buen rendiemiento
- Y por último destacar que su programación es muy sencilla, a través de YML

En contra, habría que añadir

- No es una herramienta  como administrador de configuraciones
- Como trabaja con conexiones SSH, se requieren para grandes entornos configuraciones avanzadas.
- Gestión tardía de los Bugs




## INSTALACION

- Es necesario configurar el repositorio EPEL, que como sabemos es un repositorio de SW externo

````
sudo yum install epel-release
````

````
Loaded plugins: fastestmirror
Determining fastest mirrors
epel/x86_64/metalink                                                                                                                             |  20 kB  00:00:00
 * base: mirror.gadix.com
 * epel: mirror.airenetworks.es
 * extras: mirror.gadix.com
 * updates: mirror.gadix.com
base                                                                                                                                             | 3.6 kB  00:00:00
epel                                                                                                                                             | 3.2 kB  00:00:00
extras                                                                                                                                           | 3.4 kB  00:00:00
updates                                                                                                                                          | 3.4 kB  00:00:00
(1/7): base/7/x86_64/group_gz                                                                                                                    | 166 kB  00:00:00
(2/7): epel/x86_64/group_gz                                                                                                                      |  88 kB  00:00:00
(3/7): extras/7/x86_64/primary_db                                                                                                                | 204 kB  00:00:00
(4/7): epel/x86_64/updateinfo                                                                                                                    | 935 kB  00:00:03
(5/7): epel/x86_64/primary                                                                                                                       | 3.6 MB  00:00:04
(6/7): updates/7/x86_64/primary_db                                                                                                               | 6.0 MB  00:00:05
(7/7): base/7/x86_64/primary_db                                                                                                                  | 5.9 MB  00:00:07
epel                                                                                                                                                        12759/12759 Package epel-release-7-11.noarch already installed and latest version
Nothing to do
````

Una vez instalado el repositorio tendremos accesible la instalación de Ansible

````
[vagrant@centos7 ~]$ yum search ansible
Loaded plugins: fastestmirror
Determining fastest mirrors
 * base: mirror.gadix.com
 * epel: mirror.airenetworks.es
 * extras: mirror.airenetworks.es
 * updates: mirror.airenetworks.es
epel                                                                                                                                                        12759/12759 ========================================================================= N/S matched: ansible =========================================================================ansible-doc.noarch : Documentation for Ansible
ansible-inventory-grapher.noarch : Creates graphs representing ansible inventory
ansible-lint.noarch : Best practices checker for Ansible
ansible-openstack-modules.noarch : Unofficial Ansible modules for managing Openstack
ansible-review.noarch : Reviews Ansible playbooks, roles and inventory and suggests improvements
python2-ansible-runner.noarch : A tool and python library to interface with Ansible
python2-ansible-tower-cli.noarch : A CLI tool for Ansible Tower
ansible.noarch : SSH-based configuration management, deployment, and task execution system
kubernetes-ansible.noarch : Playbook and set of roles for seting up a Kubernetes cluster onto machines
loopabull.noarch : Event loop driven Ansible playbook execution engine
standard-test-roles.noarch : Standard Test Interface Ansible roles

  Name and summary matches only, use "search all" for everything.
  ````

````
  [vagrant@centos7 ~]$ sudo yum install ansible

  ---> Package python-ply.noarch 0:3.4-11.el7 will be installed
--> Finished Dependency Resolution

Dependencies Resolved

======================================================================================================================================================================== Package                                                     Arch                           Version                                Repository                      Size ========================================================================================================================================================================Installing:
 ansible                                                     noarch                         2.7.0-1.el7                            epel                            11 M Installing for dependencies:
 PyYAML                                                      x86_64                         3.10-11.el7                            base                           153 k  libtomcrypt                                                 x86_64                         1.17-26.el7                            extras                         224 k  libtommath                                                  x86_64                         0.42.0-6.el7                           extras                          36 k  libyaml                                                     x86_64                         0.1.4-11.el7_0                         base                            55 k  python-babel                                                noarch                         0.9.6-8.el7                            base                           1.4 M  python-backports                                            x86_64                         1.0-8.el7                              base                           5.8 k  python-backports-ssl_match_hostname                         noarch                         3.5.0.1-1.el7                          base                            13 k  python-cffi                                                 x86_64                         1.6.0-5.el7                            base                           218 k  python-enum34                                               noarch                         1.0.4-1.el7                            base                            52 k  python-httplib2                                             noarch                         0.9.2-1.el7                            extras                         115 k  python-idna                                                 noarch                         2.4-1.el7                              base                            94 k  python-ipaddress                                            noarch                         1.0.16-2.el7                           base                            34 k  python-jinja2                                               noarch                         2.7.2-2.el7                            base                           515 k  python-keyczar                                              noarch                         0.71c-2.el7                            epel                           218 k  python-markupsafe                                           x86_64                         0.11-10.el7                            base                            25 k  python-paramiko                                             noarch                         2.1.1-4.el7                            extras                         268 k  python-ply                                                  noarch                         3.4-11.el7                             base                           123 k  python-pycparser                                            noarch                         2.14-1.el7                             base                           104 k  python-setuptools                                           noarch                         0.9.8-7.el7                            base                           397 k  python-six                                                  noarch                         1.9.0-2.el7                            base                            29 k  python2-crypto                                              x86_64                         2.6.1-15.el7                           extras                         477 k  python2-cryptography                                        x86_64                         1.7.2-2.el7                            base                           502 k  python2-jmespath                                            noarch                         0.9.0-3.el7                            extras                          39 k  python2-pyasn1                                              noarch                         0.1.9-7.el7                            base                           100 k  sshpass                                                     x86_64                         1.06-2.el7                             extras                          21 k

Transaction Summary
========================================================================================================================================================================Install  1 Package (+25 Dependent packages)
````

Para verificar que la instalación ha sido correcta, sólo tenemos que ejecutar

```` 
[vagrant@centos7 ~]$ ansible --version

ansible 2.7.0
  config file = /etc/ansible/ansible.cfg
  configured module search path = [u'/home/vagrant/.ansible/plugins/modules', u'/usr/share/ansible/plugins/modules']
  ansible python module location = /usr/lib/python2.7/site-packages/ansible
  executable location = /usr/bin/ansible
  python version = 2.7.5 (default, Jul 13 2018, 13:06:57) [GCC 4.8.5 20150623 (Red Hat 4.8.5-28)]

````

Para ver la lista de modulos ansible disponibles
```` 
[vagrant@centos7 ~]$ ansible-doc --list
````

# Primeros pasos

Si todo hai ido bien, ansible se ha instalado en la carpeta /etc/ansible

````

[vagrant@centos7 ansible]$ pwd
/etc/ansible
[vagrant@centos7 ansible]$ ls -l
total 24
-rw-r--r--. 1 root root 20269 Oct  8 18:34 ansible.cfg
-rw-r--r--. 1 root root  1016 Oct  8 18:34 hosts
drwxr-xr-x. 2 root root     6 Oct  8 18:34 roles

````
Ansible se instala en un único servidor para administrar multiples máquinas.

*host*
El Fichero host, es el fichero en donde se van a describir todos los servidores o dispositivos que vamos a administrar. Por ejemplo

````
# This is the default ansible 'hosts' file.
#
# It should live in /etc/ansible/hosts
#
#   - Comments begin with the '#' character
#   - Blank lines are ignored
#   - Groups of hosts are delimited by [header] elements
#   - You can enter hostnames or ip addresses
#   - A hostname/ip can be a member of multiple groups

# Ex 1: Ungrouped hosts, specify before any group headers.

## green.example.com
## blue.example.com
## 192.168.100.1
## 192.168.100.10

# Ex 2: A collection of hosts belonging to the 'webservers' group

## [webservers]
## alpha.example.org
## beta.example.org
## 192.168.1.100
## 192.168.1.110

# If you have multiple hosts following a pattern you can specify
# them like this:

## www[001:006].example.com

# Ex 3: A collection of database servers in the 'dbservers' group

## [dbservers]
##
## db01.intranet.mydomain.net
## db02.intranet.mydomain.net
## 10.25.1.56
## 10.25.1.57

# Here's another example of host ranges, this time there are no
# leading 0s:

## db-[99:101]-node.example.com
````



*ansible.cfg*
Fichero donde se definen las configuraciones globales a usar con ansible

## SSH
Supongamos que modificamos el fichero host para trabajar directamente con el servidor local, e intentamos hacer ping a través del módulo ansible m

ansible [ servidores: grupo(/s) ] -m ping
    -m => Para indicar el nombre del Modulo, En este caso ping
ansible [ servidores: grupo(/s) ] -a "uptime"
    -a => Para ejecutar comandos directamente en el servidor
        ansible [ servidores: grupo(/s) ] -a "hostname"


````
[vagrant@centos7 ~]$ ansible localhost -m ping
The authenticity of host 'localhost (127.0.0.1)' can't be established.
RSA key fingerprint is SHA256:ZE4uznUB4WSGcjNRjrqP7ljGrEHOutAOAzZh/tZ9LUM.
RSA key fingerprint is MD5:46:b1:b2:f7:88:b3:61:12:b4:40:b4:42:90:4a:ae:3e.
Are you sure you want to continue connecting (yes/no)? yes
localhost | UNREACHABLE! => {
    "changed": false,
    "msg": "Failed to connect to the host via ssh: Warning: Permanently added 'localhost' (RSA) to the list of known hosts.\r\nPermission denied (publickey,gssapi-keyex,gssapi-with-mic).\r\n",
    "unreachable": true
}
[vagrant@centos7 ~]$
````

Si intentamos acceder al servidor local tendremos un error controlado de seguridad. Es necesario incluir la clave publica

vi /etc/ansible/host
    localhost ansible_connection=local
Y ahora con el mismo comando obtenemos el resultado esperado, y ya podremos ejecutar comandos directamente en el servidor local
````
[vagrant@centos7 ~]$ ansible localhost -m ping
localhost | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
````

En el servidor Ansible, necesitaremos hacer uso del usuario "vagrant". En caso de querer trabajar con este usuario será necesario generar su clav ssh.

> ssh-keygen -o -t rsa -C "benauca@gmail.com" -b 4096

Obtener su clave  

> cat ~/.ssh/id_rsa.pub

E incluirla en la lista de authorized-keys


> ansible 192.168.33.6 -u vagrant -m ping

Recordar que para ello habremos debido incluir el servidor  192.168.336.6 en el fichero host

En caso de querer ejecutar el Modulo en todos los servidores bastará con ejecutar el commando con el parámetro "all"

> ansible all -u vagrant -m ping


En caso de tener inventariados dos servidores con distinto usuario.
- Ubuntu --- ubuntu
- Centos7--- vagrant

el comando anterior fallará al establecer la conexión con el usuario vagrant en ubuntu.
Para solventar este error, lo mejor es modificar el fichero de inventario

```
192.168.33.6    ansible_user=vagrant
192.168.33.105  ansible_user=ubuntu
```
y ejecutar el comando sin necesidad de pasar el usuario. 

> ansible all -m ping

# Usuario No Root

--become

Supngamos el caso de uso Añadir Un usuario a los distintos servidores.

> ansible all -m user -a "name=benauca state=present"

````
localhost | FAILED! => {
    "changed": false,
    "cmd": "/usr/sbin/useradd -m benauca",
    "msg": "[Errno 13] Permission denied",
    "rc": 13
}
````

este comando intentará crer el usuario benauca en todas las máquinas, pero fallará si no tenemos permisos de administrador para los usuarios vagrant, y ubuntu de modificar el fichero /etc/passw

> ansible all -m user -a "name=benauca state=present" --become

```
localhost | CHANGED => {
    "changed": true,
    "comment": "",
    "create_home": true,
    "group": 1001,
    "home": "/home/benauca",
    "name": "benauca",
    "shell": "/bin/bash",
    "state": "present",
    "system": false,
    "uid": 1001
}
```

Con la opción --become cambiamos a root y podremos ejecutar las acciones. Es similar al comando "sudo". 

> ansible all --become -a "id"

Nos Mostrará que el comando se ha ejecutado con el usuario root

```
[vagrant@centos7 ~]$ ansible all --become -a "id"
localhost | CHANGED | rc=0 >>
uid=0(root) gid=0(root) groups=0(root) context=unconfined_u:unconfined_r:unconfined_t:s0-s0:c0.c1023

```
> ansible all --become -a "id"

El mismo comando sin la opción --become, nos mostrará que el comando se ha ejcutado con el usuario vagrant

```
[vagrant@centos7 ~]$ id
uid=1000(vagrant) gid=1000(vagrant) groups=1000(vagrant) context=unconfined_u:unconfined_r:unconfined_t:s0-s0:c0.c1023
```

> ansible all  -a "id"

```
[vagrant@centos7 ~]$ ansible all  -a "id"
localhost | CHANGED | rc=0 >>
uid=1000(vagrant) gid=1000(vagrant) groups=1000(vagrant) context=unconfined_u:unconfined_r:unconfined_t:s0-s0:c0.c1023

```

Por todo esto, es necesario tener correctamente configurado SUDO para ejecutar tareas de administrador.