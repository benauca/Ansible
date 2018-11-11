`TAGS`
--- 
Las etiquetas `tags` nos permiten especificar selectivamente que tareas se van a ejecutar o se omitiran


Sintaxis
---
- module: acciones
- tags: etiqueta1 o conjunto de etiquetas [tag2, tag2]


Ejemplo:

```
- name: Ejemplo de etiquetas
  hosts: all
  tasks:
    - debug: msg="Tarea con etiqueta produccion PROD"
      tags: prod
    - debug: msg="Tarea con etiqueta Desarrollo DEV"
      tags: dev
    - debug: msg="Tarea con varios tags QA y PROD
      tags: [qa, prod]
```

```
[vagrant@centos7 ~]$ ansible-playbook tags-playbook.yml

PLAY [Ejemplo de etiquetas] ********************************************************************************************************************************************

TASK [Gathering Facts] *************************************************************************************************************************************************ok: [localhost]

TASK [debug] ***********************************************************************************************************************************************************ok: [localhost] => {
    "msg": "Tarea con etiqueta produccion PROD"
}

TASK [debug] ***********************************************************************************************************************************************************ok: [localhost] => {
    "msg": "Tarea con etiqueta Desarrollo DEV"
}

TASK [debug] ***********************************************************************************************************************************************************ok: [localhost] => {
    "msg": "Tarea con varios tags QA y PROD"
}

PLAY RECAP *************************************************************************************************************************************************************localhost                  : ok=4    changed=0    unreachable=0    failed=0
```

Para ejecutar las tareas con la etiqueta PROD basta con incluir en la lista de parámetros de la consola de parámetros

```
[vagrant@centos7 ~]$ ansible-playbook tags-playbook.yml --tags dev
```


```
[vagrant@centos7 ~]$ ansible-playbook tags-playbook.yml --tags dev

PLAY [Ejemplo de etiquetas] ********************************************************************************************************************************************

TASK [Gathering Facts] *************************************************************************************************************************************************ok: [localhost]

TASK [debug] ***********************************************************************************************************************************************************ok: [localhost] => {
    "msg": "Tarea con etiqueta Desarrollo DEV"
}

PLAY RECAP *************************************************************************************************************************************************************localhost                  : ok=2    changed=0    unreachable=0    failed=0
```


En el caso de querer saltar alguna etiqueta `--skip-tags`

[vagrant@centos7 ~]$ ansible-playbook --skip-tags qa tags-playbook.yml

```
PLAY [Ejemplo de etiquetas] ********************************************************************************************************************************************

TASK [Gathering Facts] *************************************************************************************************************************************************ok: [localhost]

TASK [debug] ***********************************************************************************************************************************************************ok: [localhost] => {
    "msg": "Tarea con etiqueta produccion PROD"
}

TASK [debug] ***********************************************************************************************************************************************************ok: [localhost] => {
    "msg": "Tarea con etiqueta Desarrollo DEV"
}

PLAY RECAP *************************************************************************************************************************************************************localhost                  : ok=3    changed=0    unreachable=0    failed=0
```

Existe una etiqueta especial `always` que se ejecutará siempre.

---------------------

LOOKUP
===
La directiva lookup no permite obtener datos desde el servidor que está ejecutando el playbook
Ejemplo
---

"{{ lookup ('file', '/etc/mod')}}"


ANSIBLE_VAULT
===
 El comando ansible-vault nos permite cifrar ficheros con una contraseña para proteger datos sensibles. 

> ansible-vault create => Crea el fichero
> ansible-vault edit => Edita  el fichero
> ansible-vault encrypt => Cifra el fichero existente
> ansible-vault decrypt => Desencripta el fichero existente
> ansible-vault view => Visualiza el fichero

El comando ansible-playbook incluye dos opciones para indicar la clave 
- `--ask-vault-pass=`
- `--vault-password-file=`

```
Usage: ansible-vault [create|decrypt|edit|encrypt|encrypt_string|rekey|view] [options] [vaultfile.yml]

encryption/decryption utility for Ansible data files

Options:
  --ask-vault-pass      ask for vault password
  -h, --help            show this help message and exit
  --new-vault-id=NEW_VAULT_ID
                        the new vault identity to use for rekey
  --new-vault-password-file=NEW_VAULT_PASSWORD_FILE
                        new vault password file for rekey
  --vault-id=VAULT_IDS  the vault identity to use
  --vault-password-file=VAULT_PASSWORD_FILES
                        vault password file
  -v, --verbose         verbose mode (-vvv for more, -vvvv to enable
                        connection debugging)
  --version             show program's version number and exit

 See 'ansible-vault <command> --help' for more information on a specific
 ```

 > ansible-vault create fichero.yml [ Crea un fichero encriptado. Pedirá la contraseña que queremos usar.]

 El contenido del fichero estará encriptado

> vi test.yml

```
 $ANSIBLE_VAULT;1.1;AES256
36346265323330356665353537643764303738666433353962393231383261643530333739613561
3364396536303762313763303332303837323765373135350a393132393431343165336130666263
33616562616433393564313465653765313333656264396532613666313836616165306535383437
6331306439346639350a303065613264353939656232643233663266393230373566373938393166
3537
```
Si queremos editarlo, usaremos la opcion `edit`

> [vagrant@centos7 ~]$ ansible-vault edit file.yml
```
Vault password:
```

TAREAS ASINCRONAS
===

Cuando realizamos una tarea en un servidor a través de un modulo, la conexión permanece abierta esperando su fianlización. Ansible nos da la opción realizar tareas en segundo lano y consultar estado periódicamente


Sintaxis
--- 
Metodo 1: 

- modulo: argumentos
  async: Tiempo máximo
  poll: Tiempo de consulta

Metodo 2:

- modulo: argumentos
  async: Tiempo max
  poll: 0
  register: tarea_async

- async-status: jid= {{ tarea.async.ansible.job.id }}
  register: estado
  until: estado.finished
  retries: 30 


Ejemplo
---

```
- name: Ejemplo de tareas asincronas
  hosts: all
  tasks:
    - command: sleep 15
      async: 60
      poll: 0
      register: estado
    - debug: msg="Otra tarea"
    - async_status:
        jid: "{{ estado.ansible_job_id }}"
    register: new_estado
    until: new_estado.finished
    retries: 30
```

Salida

```
[vagrant@centos7 ~]$ ansible-playbook async-main.yml

PLAY [Ejemplo de tareas asincronas] ************************************************************************************************************************************

TASK [Gathering Facts] *************************************************************************************************************************************************ok: [localhost]

TASK [command] *********************************************************************************************************************************************************changed: [localhost]

TASK [debug] ***********************************************************************************************************************************************************ok: [localhost] => {
    "msg": "Otra tarea"
}

TASK [async_status] ****************************************************************************************************************************************************
FAILED - RETRYING: async_status (30 retries left).
FAILED - RETRYING: async_status (29 retries left).
FAILED - RETRYING: async_status (28 retries left).
changed: [localhost]

PLAY RECAP *************************************************************************************************************************************************************localhost                  : ok=4    changed=2    unreachable=0    failed=0

```