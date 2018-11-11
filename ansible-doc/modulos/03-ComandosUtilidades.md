MODULOS COMANDOS UTILIDADES
===
Lista de algunos de los modulos

- command: Ejecuta comandos en los distintos servidores
- expect: ejcuta un comando que responde a la ejecución de datos
- raw: envía comandos sin filtar por ssh
- script: Transfiere y ejecuta un script indicado
- shell

`MODULE COMMAND`
---

> COMMAND    (/usr/lib/python2.7/site-packages/ansible/modules/commands/command.py)

The `command` module takes the command name followed by a list of space-delimited arguments. The given command will be executed on all selected nodes. It will not be processed through the shell, so variables like `$HOME' and operations like "<", ">", "|", ";" and "&" will not work (use the [shell] module if you need these  features).

For Windows targets, use the [win_command] module instead.

```
OPTIONS (= is mandatory):

- argv
        Allows the user to provide the command as a list vs. a string.  Only the string or the list form can be provided, not both.
        One or the other must be provided.
        [Default: (null)]
        version_added: 2.6

- chdir
        Change into this directory before running the command.
        [Default: (null)]
        version_added: 0.6

- creates
        A filename or (since 2.0) glob pattern. If it already exists, this step *won't* be run.
        [Default: (null)]

= free_form
        The command module takes a free form command to run.  There is no parameter actually named 'free form'. See the examples!


- removes
        A filename or (since 2.0) glob pattern. If it already exists, this step *will* be run.
        [Default: (null)]
        version_added: 0.8

- stdin
        Set the stdin of the command directly to the specified value.
        [Default: (null)]
        version_added: 2.4

- warn
        If command_warnings are on in ansible.cfg, do not warn about this particular line if set to `no'.
        [Default: yes]
```

EXAMPLES:
```
- name: return motd to registered var
  command: cat /etc/motd
  register: mymotd

- name: Run the command if the specified file does not exist.
  command: /usr/bin/make_database.sh arg1 arg2
  args:
    creates: /path/to/database

# You can also use the 'args' form to provide the options.
- name: This command will change the working directory to somedir/ and will only run when /path/to/database doesn't exist.
  command: /usr/bin/make_database.sh arg1 arg2
  args:
    chdir: somedir/
    creates: /path/to/database

- name: use argv to send the command as a list.  Be sure to leave command empty
  command:
  args:
    argv:
      - echo
      - testing

- name: safely use templated variable to run command. Always use the quote filter to avoid injection issues.
  command: cat {{ myfile|quote }}
  register: myoutput
```

`MODULE EXPECT`
---

> EXPECT    (/usr/lib/python2.7/site-packages/ansible/modules/commands/expect.py)

The `expect` module executes a command and responds to prompts. The given command will be executed on all selected nodes. It will not be processed through the shell, so variables like `$HOME` and operations like `"<"`, `">"`, `"|"`, and `"&"` will not work.

```
EXAMPLES:
- name: Case insensitive password string match
  expect:
    command: passwd username
    responses:
      (?i)password: "MySekretPa$$word"
  # you don't want to show passwords in your logs
  no_log: true

- name: Generic question with multiple different responses
  expect:
    command: /path/to/custom/command
    responses:
      Question:
        - response1
        - response2
        - response3
```
vi mypalybook.yml
```
- name: Instalar pexpect
  yum: name=pexpect state=latest
- name: cambiar password usuario
  expect:
     command: passwd usuario
     response:
        (?i)password: "supersecreta"
```



