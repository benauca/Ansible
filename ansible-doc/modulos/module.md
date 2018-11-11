Modulos
===

Módulos: Componentes que realizan las acciones sobre los servidores
- Toda Tarea está asociada a un módulo
- Estos reciben argumentos que pueden ser 
- - OPCIONALES
- - OBLIGATORIOS

Los módulos se agrupan por cateegorías

| Lista    |de  | Modulos  | Disponibles  |
| :---: | :---: | :---: | :---: |
|Cloud|Files|Network|Storage|
|Clustering|Identity|Notification|System|
|Command|Inventory|Packaging|Utility|
|Crypto|Messaging|Remote Manage|Web|
|Database|Monitoring|Soure Control|Windows|



```
## Listar Módulos disponibles en nuestro servidor ansible
ansible-doc -l 
```


Para acceder a la información de un módulo determinado, basta con ejecutar el comando para acceder a la ayuda del comando.

```
    ansible -doc ${Nombre_Modulo}
```
