Modulo de tratados de Ficheros Y OPEN SSL
===

Permiten trabajar con 
- Ficheros
- PLantillas
- Directorios

`Files`
-----------

|  |  |  |
| :---: | :---: | :---: |
| acl | file | stat |
| archive | find | synchronize |
| asamble | inifile | tempfile |
| blockinfile | lincinfile | templatetat |
| copy | patch | unarchive |
| fetch | replace | xattr |

`open-ssl`
-------------

- openssl-privatekey
- openssl-publickey

Modulo Copy
---
COPY    (/usr/lib/python2.7/site-packages/ansible/modules/files/copy.py)

        The `copy' module copies a file from the local or remote machine to a location on the remote machine. 
        
        Use the [fetch] module to copy files from remote locations to the local box. If you need variable interpolation in copied files, 
        
        use the [template] module. 
        
        For Windows targets, use the [win_copy] module instead.

  * note: This module has a corresponding action plugin.

`EXAMPLES:`
``` 
- name: example copying file with owner and permissions
  copy:
    src: /srv/myfiles/foo.conf
    dest: /etc/foo.conf
    owner: foo
    group: foo
    mode: 0644

- name: The same example as above, but using a symbolic mode equivalent to 0644
  copy:
    src: /srv/myfiles/foo.conf
    dest: /etc/foo.conf
    owner: foo
    group: foo
    mode: u=rw,g=r,o=r

- name: Another symbolic mode example, adding some permissions and removing others
  copy:
    src: /srv/myfiles/foo.conf
    dest: /etc/foo.conf
    owner: foo
    group: foo
    mode: u+rw,g-wx,o-rwx

- name: Copy a new "ntp.conf file into place, backing up the original if it differs from the copied version
  copy:
    src: /mine/ntp.conf
    dest: /etc/ntp.conf
    owner: root
    group: root
    mode: 0644
    backup: yes

- name: Copy a new "sudoers" file into place, after passing validation with visudo
  copy:
    src: /mine/sudoers
    dest: /etc/sudoers
    validate: /usr/sbin/visudo -cf %s

- name: Copy a "sudoers" file on the remote machine for editing
  copy:
    src: /etc/sudoers
    dest: /etc/sudoers.edit
    remote_src: yes
    validate: /usr/sbin/visudo -cf %s

- name: Copy using the 'content' for inline data
  copy:
    content: '# This file was moved to /etc/other.conf'
    dest: /etc/mine.conf'
```

Modulo Templates
---

> TEMPLATE    (/usr/lib/python2.7/site-packages/ansible/modules/files/template.py)

Templates are processed by the Jinja2 templating language (http://jinja.pocoo.org/docs/) - documentation on the template formatting can be found in the Template Designer Documentation [http://jinja.pocoo.org/docs/templates/]. 

Six additional variables can be used in templates:
- `ansible_managed` (configurable via the `defaults` section of `ansible.cfg`) contains a string which can be used to describe the template name, host, modification time of the template file and the owner uid.
- `template_host` contains the node name of the template's machine. `template_uid' is the numeric user id of the owner.
- `template_path` is the path of the template. 
- `template_fullpath` is the absolute path of the template. 
- `template_run_date` is the date that the template was rendered.

  * note: This module has a corresponding action plugin.

`EXAMPLES`
```
# Example from Ansible Playbooks
- template:
    src: /mytemplates/foo.j2
    dest: /etc/file.conf
    owner: bin
    group: wheel
    mode: 0644

# The same example, but using symbolic modes equivalent to 0644
- template:
    src: /mytemplates/foo.j2
    dest: /etc/file.conf
    owner: bin
    group: wheel
    mode: "u=rw,g=r,o=r"

# Create a DOS-style text file from a template
- template:
    src: config.ini.j2
    dest: /share/windows/config.ini
    newline_sequence: '\r\n'

# Copy a new "sudoers" file into place, after passing validation with visudo
- template:
    src: /mine/sudoers
    dest: /etc/sudoers
    validate: '/usr/sbin/visudo -cf %s'

# Update sshd configuration safely, avoid locking yourself out
- template:
    src: etc/ssh/sshd_config.j2
    dest: /etc/ssh/sshd_config
    owner: root
    group: root
    mode: '0600'
    validate: /usr/sbin/sshd -t -f %s
    backup: yes
```

`Module File`
---
Tratamiento de permisos.

> FILE    (/usr/lib/python2.7/site-packages/ansible/modules/files/file.py)

Sets attributes of files, symlinks, and directories, or removes f iles/symlinks/directories. 

Many other modules support the same options as the `file` module including [copy], [template], and [assemble]. 
 For Windows targets, use the [win_file]
module instead.


```
EXAMPLES:
# change file ownership, group and mode
- file:
    path: /etc/foo.conf
    owner: foo
    group: foo
    # when specifying mode using octal numbers, add a leading 0
    mode: 0644
- file:
    path: /work
    owner: root
    group: root
    mode: 01777
- file:
    src: /file/to/link/to
    dest: /path/to/symlink
    owner: foo
    group: foo
    state: link
- file:
    src: '/tmp/{{ item.src }}'
    dest: '{{ item.dest }}'
    state: link
  with_items:
    - { src: 'x', dest: 'y' }
    - { src: 'z', dest: 'k' }

# touch a file, using symbolic modes to set the permissions (equivalent to 0644)
- file:
    path: /etc/foo.conf
    state: touch
    mode: "u=rw,g=r,o=r"

# touch the same file, but add/remove some permissions
- file:
    path: /etc/foo.conf
    state: touch
    mode: "u+rw,g-wx,o-rwx"

# touch again the same file, but dont change times
# this makes the task idempotents
- file:
    path: /etc/foo.conf
    state: touch
    mode: "u+rw,g-wx,o-rwx"
    modification_time: "preserve"
    access_time: "preserve"

# create a directory if it doesn't exist
- file:
    path: /etc/some_directory
    state: directory
    mode: 0755

# updates modification and access time of given file
- file:
    path: /etc/some_file
    state: file
    mode: 0755
    modification_time: now
    access_time: now

```

`Module Stat`
---

Comando Stat linux. Muestra información detallada de un fichero. Para guardarlos se suele usar guardandolo en una variable

> STAT    (/usr/lib/python2.7/site-packages/ansible/modules/files/stat.py)

Retrieves facts for a file similar to the linux/unix `stat` command. 

For Windows targets, use the [win_stat] module instead.
```
EXAMPLES:
# Obtain the stats of /etc/foo.conf, and check that the file still belongs
# to 'root'. Fail otherwise.
- stat:
    path: /etc/foo.conf
  register: st
- fail:
    msg: "Whoops! file ownership has changed"
  when: st.stat.pw_name != 'root'

# Determine if a path exists and is a symlink. Note that if the path does
# not exist, and we test sym.stat.islnk, it will fail with an error. So
# therefore, we must test whether it is defined.
# Run this to understand the structure, the skipped ones do not pass the
# check performed by 'when'
- stat:
    path: /path/to/something
  register: sym

- debug:
    msg: "islnk isn't defined (path doesn't exist)"
  when: sym.stat.islnk is not defined

- debug:
    msg: "islnk is defined (path must exist)"
  when: sym.stat.islnk is defined

- debug:
    msg: "Path exists and is a symlink"
  when: sym.stat.islnk is defined and sym.stat.islnk

- debug:
    msg: "Path exists and isn't a symlink"
  when: sym.stat.islnk is defined and sym.stat.islnk == False


# Determine if a path exists and is a directory.  Note that we need to test
# both that p.stat.isdir actually exists, and also that it's set to true.
- stat:
    path: /path/to/something
  register: p
- debug:
    msg: "Path exists and is a directory"
  when: p.stat.isdir is defined and p.stat.isdir

# Don't do checksum
- stat:
    path: /path/to/myhugefile
    get_checksum: no

# Use sha256 to calculate checksum
- stat:
    path: /path/to/something
    checksum_algorithm: sha256
```

`Module Fetch`
---

Comando para copiar desde un servidor remoto a un servidor local ansible

- flat      Copiar o no la estructura de directorios

> FETCH    (/usr/lib/python2.7/site-packages/ansible/modules/files/fetch.py)

This module works like [copy], but in reverse. 
It is used for fetching files from remote machines and storing them locally in a file tree, organized by hostname. 
This module is also supported for Windows targets.

```
EXAMPLES:
# Store file into /tmp/fetched/host.example.com/tmp/somefile
- fetch:
    src: /tmp/somefile
    dest: /tmp/fetched

# Specifying a path directly
- fetch:
    src: /tmp/somefile
    dest: /tmp/prefix-{{ inventory_hostname }}
    flat: yes

# Specifying a destination path
- fetch:
    src: /tmp/uniquefile
    dest: /tmp/special/
    flat: yes

# Storing in a path relative to the playbook
- fetch:
    src: /tmp/uniquefile
    dest: special/prefix-{{ inventory_hostname }}
    flat: yes
```

`Module UNARCHIVE`
---

> UNARCHIVE    (/usr/lib/python2.7/site-packages/ansible/modules/files/unarchive.py)

The `unarchive` module unpacks an archive. 
By default, it will copy the source file from the local system to the target before unpacking. 
Set `remote_src=yes` to unpack an archive which already exists on the target. 

For Windows targets, use the [win_unzip] module instead.
If checksum validation is desired, use [get_url] or [uri] instead to fetch the file and set `remote_src=yes`.

```
EXAMPLES:
- name: Extract foo.tgz into /var/lib/foo
  unarchive:
    src: foo.tgz
    dest: /var/lib/foo

- name: Unarchive a file that is already on the remote machine
  unarchive:
    src: /tmp/foo.zip
    dest: /usr/local/bin
    remote_src: yes

- name: Unarchive a file that needs to be downloaded (added in 2.0)
  unarchive:
    src: https://example.com/example.zip
    dest: /usr/local/bin
    remote_src: yes

- name: Unarchive a file with extra options
  unarchive:
    src: /tmp/foo.zip
    dest: /usr/local/bin
    extra_opts:
    - --transform
    - s/^xxx/yyy/
```

`Module LINEINFILE`
---
> LINEINFILE    (/usr/lib/python2.7/site-packages/ansible/modules/files/lineinfile.py)

This module ensures a particular line is in a file, or replace an existing line using a back-referenced regular expression.

This is primarily useful when you want to change a single line in a file only. 

See the [replace] module if you want to change multiple, similar lines or check [blockinfile] if you want to insert/update/remove a block of lines in a file. 

For other cases, see the [copy] or [template] modules.

```
EXAMPLES:
# Before 2.3, option 'dest', 'destfile' or 'name' was used instead of 'path'
- lineinfile:
    path: /etc/selinux/config
    regexp: '^SELINUX='
    line: 'SELINUX=enforcing'

- lineinfile:
    path: /etc/sudoers
    state: absent
    regexp: '^%wheel'

# Searches for a line that begins with 127.0.0.1 and replaces it with the value of the 'line' parameter
- lineinfile:
    path: /etc/hosts
    regexp: '^127\.0\.0\.1'
    line: '127.0.0.1 localhost'
    owner: root
    group: root
    mode: 0644

- lineinfile:
    path: /etc/httpd/conf/httpd.conf
    regexp: '^Listen '
    insertafter: '^#Listen '
    line: 'Listen 8080'

- lineinfile:
    path: /etc/services
    regexp: '^# port for http'
    insertbefore: '^www.*80/tcp'
    line: '# port for http by default'

# Add a line to a file if the file does not exist, without passing regexp
- lineinfile:
    path: /tmp/testfile
    line: '192.168.1.99 foo.lab.net foo'
    create: yes

# Fully quoted because of the ': ' on the line. See the Gotchas in the YAML docs.
- lineinfile:
    path: /etc/sudoers
    state: present
    regexp: '^%wheel\s'
    line: '%wheel ALL=(ALL) NOPASSWD: ALL'

# Yaml requires escaping backslashes in double quotes but not in single quotes
- lineinfile:
    path: /opt/jboss-as/bin/standalone.conf
    regexp: '^(.*)Xms(\\d+)m(.*)$'
    line: '\1Xms${xms}m\3'
    backrefs: yes

# Validate the sudoers file before saving
- lineinfile:
    path: /etc/sudoers
    state: present
    regexp: '^%ADMIN ALL='
    line: '%ADMIN ALL=(ALL) NOPASSWD: ALL'
    validate: '/usr/sbin/visudo -cf %s'
```


En nuestro ejemplo vamos a editar el fichero /etc/selinux/config 

``` 
line-playbook.yml
---
- name: Test Line in File
  hosts: all
  become: true
  tasks:
    - name: Test
      lineinfile:
        path: /etc/selinux/config
        regexp: '^SELINUX='
        line: 'SELINUX= disabled'
...
```

Fichero origina
```
# This file controls the state of SELinux on the system.
# SELINUX= can take one of these three values:
#     enforcing - SELinux security policy is enforced.
#     permissive - SELinux prints warnings instead of enforcing.
#     disabled - No SELinux policy is loaded.
SELINUX=enforcing
# SELINUXTYPE= can take one of three two values:
#     targeted - Targeted processes are protected,
#     minimum - Modification of targeted policy. Only selected processes are protected.
#     mls - Multi Level Security protection.
SELINUXTYPE=targeted
```

```
# This file controls the state of SELinux on the system.                                         
# SELINUX= can take one of these three values:                                                   
#     enforcing - SELinux security policy is enforced.                                           
#     permissive - SELinux prints warnings instead of enforcing.                                 
#     disabled - No SELinux policy is loaded.                                                    
SELINUX= disabled                                                                                
# SELINUXTYPE= can take one of three two values:                                                 
#     targeted - Targeted processes are protected,                                               
#     minimum - Modification of targeted policy. Only selected processes are protected.          
#     mls - Multi Level Security protection.                                                     
SELINUXTYPE=targeted
```

`MODULE BLOCKINFILE`
---
 Similar a lineinfile pero trabajando con bloques de texto

 > BLOCKINFILE    (/usr/lib/python2.7/site-packages/ansible/modules/files/blockinfile.py)

This module will insert/update/remove a block of multi-line text surrounded by customizable marker lines.

```
EXAMPLES:
# Before 2.3, option 'dest' or 'name' was used instead of 'path'
- name: insert/update "Match User" configuration block in /etc/ssh/sshd_config
  blockinfile:
    path: /etc/ssh/sshd_config
    block: |
      Match User ansible-agent
      PasswordAuthentication no

- name: insert/update eth0 configuration stanza in /etc/network/interfaces
        (it might be better to copy files into /etc/network/interfaces.d/)
  blockinfile:
    path: /etc/network/interfaces
    block: |
      iface eth0 inet static
          address 192.0.2.23
          netmask 255.255.255.0

- name: insert/update configuration using a local file and validate it
  blockinfile:
    block: "{{ lookup('file', './local/ssh_config') }}"
    dest: "/etc/ssh/ssh_config"
    backup: yes
    validate: "/usr/sbin/sshd -T -f %s"

- name: insert/update HTML surrounded by custom markers after <body> line
  blockinfile:
    path: /var/www/html/index.html
    marker: "<!-- {mark} ANSIBLE MANAGED BLOCK -->"
    insertafter: "<body>"
    content: |
      <h1>Welcome to {{ ansible_hostname }}</h1>
      <p>Last updated on {{ ansible_date_time.iso8601 }}</p>

- name: remove HTML as well as surrounding markers
  blockinfile:
    path: /var/www/html/index.html
    marker: "<!-- {mark} ANSIBLE MANAGED BLOCK -->"
    content: ""

- name: Add mappings to /etc/hosts
  blockinfile:
    path: /etc/hosts
    block: |
      {{ item.ip }} {{ item.name }}
    marker: "# {mark} ANSIBLE MANAGED BLOCK {{ item.name }}"
  with_items:
    - { name: host1, ip: 10.10.1.10 }
    - { name: host2, ip: 10.10.1.11 }
    - { name: host3, ip: 10.10.1.12 }
```

`MODULOE OPENSSL-PRIVATEKEY`
---

> OPENSSL_PRIVATEKEY    (/usr/lib/python2.7/site-packages/ansible/modules/crypto/openssl_privatekey.py)

This module allows one to (re)generate OpenSSL private keys. 

It uses the pyOpenSSL python library to interact with openssl.

One can generate either RSA or DSA private keys. Keys are generated in PEM format.

Hay que instalar modulo de python requerido para crear claves en los sistemas Debian.

```
- name: python-pyopenSSL
  apt:
    name: python-openssl
    state: latest
- name: Generar Clave
  openssl_privatekey:
     path: /etc/ssl/private/
```