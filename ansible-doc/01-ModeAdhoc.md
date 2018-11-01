# Modo Adhoc

Permite realizar acciones y comprobar conexiones a los elementos del inventario.

## Sintaxis

>ansible [opciones] servidores(separados por ","):grupos:all (Palabra clave para indicar los elementos de un inventario) -m [modulo] -a [argumentos]
 
    Modulo: Opcional. Por defecto usa el "command"
    ArgumentoS: Si no hay modulos


Lo hace de 5 en 5 serviores.
````
    |
    |   --limit: filtrar a uno o varios servidores separados por ",". 
    |           Limita el numero de servidores donde se aplica la tarea.
    |   --user: (-u)  Usuario
    |   --become (-b)
    |   --f simultaneos. Por defecto 5.
````
Modulos
````
    | -- setup.     Obtiene información de un servidor ansible_favts
    | -- copy.      Copia ficheros desde el servidor local (Ansible) al resto de servidores.
    | -- yum/apt.   Modulo para la instalación de aplicaciones
    |       -name   Nombre de la aplicación a instalar. Por ejemplo name="vim"
    |       -state  Valores por defecto
    |            "present"   Indica que está presente en la instalación del sistema
    |            "latest"    Tiene que estar la última versión
    |            "absent"    El SW no está instalado dentro de un servidor
````

Opciones
    |   -L      Verifica si una tarea puede o no ser realizada
    |   -V      Verbose. 

## CONFIGURACION
Podemos cambiar la configuración global de ansible

    - Valores por defecto
    - Escalar permisos
    - Opciones OpenSSH
    - SELinux (security Linux)
    - Ansible Galaxy: repositorio de modulos de Roles.

Orden
====

- Variables de Entorno
- Ficheros. Ansible CFG. .ansible.cfg 
- Ficheros por defecto /etc/ansible/ansible.cfg


#WINDOWS
====

Si quisieramos administrar servidores Windowa, tendriamos que realizar tareas  tanto en los servidores ansible como en los propios servidores Windowa

Servidor Ansible
    - Es necesario instalar pyWinrm - pip install pywirm
    - definir conexion al valor winrm

Servidores Window
    - PowerShell 3.0 o superior
    - Configurar Control remoto
    - Habilitar puero 5986, Puerto para administrar Windows remotamente.

# COMBINAR INVENTARIOS
====

Es posible combinar inventarios independientemente de que sean estáticos o dinámicos.

Dentro de un mismo directorio podemos añadir los inventarios
-   host        [Inventario estático]
-   gce.py      [Inventario Dinámico]


Con la opción -i especificaremos el directorio

Podemos hacer referencia desde un inventario a otro

Es posible tener directorios organizados de la siguiente manera dentro de nuestro directorio de inventarios

-   ../group-vars/
-   ../host-vars/

Por todo esto siempre es recomendable el uso de --list -host