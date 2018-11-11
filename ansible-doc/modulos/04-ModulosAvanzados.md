MODULOS AVANZADOS
===

`MODULE DEBUG`
---
El modulo `debug` nos ayuda a visualizar el contendio de variables o de las plantillas

Sintaxis
---

> DEBUG    (/usr/lib/python2.7/site-packages/ansible/modules/utilities/logic/debug.py)

This module prints statements during execution and can be useful for debugging variables or expressions without necessarily halting the playbook. 

Useful for debugging together with the 'when:' directive. This module is also supported for Windows
targets.

```
OPTIONS (= is mandatory):
- msg
        The customized message that is printed. If omitted, prints a generic message.
        [Default: Hello world!]
- var
        A variable name to debug.  Mutually exclusive with the 'msg' option.
        [Default: (null)]
- verbosity
        A number that controls when the debug is run, if you set to 3 it will only run debug when -vvv or above
        [Default: 0]
        version_added: 2.1
```



```
EXAMPLES:
# Example that prints the loopback address and gateway for each host
- debug:
    msg: "System {{ inventory_hostname }} has uuid {{ ansible_product_uuid }}"

- debug:
    msg: "System {{ inventory_hostname }} has gateway {{ ansible_default_ipv4.gateway }}"
  when: ansible_default_ipv4.gateway is defined

- shell: /usr/bin/uptime
  register: result

- debug:
    var: result
    verbosity: 2

- name: Display all variables/facts known for a host
  debug:
    var: hostvars[inventory_hostname]
    verbosity: 4
```


vi debug-playbook.yml

```
- name: Ejemplo debug
  hosts: all
  tasks:
     - debug: var=ansible_host
     - debug: msg="Mi IP es {{ ansible_host }}"
```
