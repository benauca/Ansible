Modulos De gestion De Paquetes
===

Dentro de los distintos modulos existentes vamos a resaltar los siguientes

- bower: Administrador de paquetes de desrrollo web
- bundler: Administrador de paquetes o Gemas Ruby
- composer: Gestiona librerías y dependencias PHP
- cpamm: Gestión de Modulos perl [repositorio Online] (cpan).
- easy-instlla: Gestiona Modulos de python
- gem: Gemas Ruby
- maven-artifact: Modulos maven
- npm: Gestión de Modulos o paquetes Node
- pear: Gestión de paquetes pear/pcl [php]
- pip: Gestiona librerías/modulos de python
- apk: paquetes android
- dnf: Paquetes Fedora
- package: Gestión de Paquetes Genéricosque llamará a los módulos necesario según Sistema Operativo
- yum: Al igual que los tres anteriores son gestores de Paquetes de Sistema Operativo. En este caso permite la gestión de paquetes yum para distribuciones CENTOS Red Hat


Veamos algunos paquetes en detalle

`MODULO MAVEN-ARTIFACT`
---
> MAVEN_ARTIFACT    (/usr/lib/python2.7/site-packages/ansible/modules/packaging/language/maven_artifact.py)

Downloads an artifact from a maven repository given the maven coordinates provided to the module. Can retrieve snapshots or release versions of the artifact and will resolve the latest available version if one is not available.


```
EXAMPLES:                                                                                                    
# Download the latest version of the JUnit framework artifact from Maven Central                             
- maven_artifact:                                                                                            
    group_id: junit                                                                                          
    artifact_id: junit                                                                                       
    dest: /tmp/junit-latest.jar                                                                              
                                                                                                             
# Download JUnit 4.11 from Maven Central                                                                     
- maven_artifact:                                                                                            
    group_id: junit                                                                                          
    artifact_id: junit                                                                                       
    version: 4.11                                                                                            
    dest: /tmp/junit-4.11.jar                                                                                
                                                                                                             
# Download an artifact from a private repository requiring authentication                                    
- maven_artifact:                                                                                            
    group_id: com.company                                                                                    
    artifact_id: library-name                                                                                
    repository_url: 'https://repo.company.com/maven'                                                         
    username: user                                                                                           
    password: pass                                                                                           
    dest: /tmp/library-name-latest.jar                                                                       
```


`MODULE YUM`

> YUM    (/usr/lib/python2.7/site-packages/ansible/modules/packaging/os/yum.py)

        Installs, upgrade, downgrades, removes, and lists packages and groups with the `yum' package manager. This module only works
        on Python 2. If you require Python 3 support see the [dnf] module.

  * note: This module has a corresponding action plugin.
```
EXAMPLES:
- name: install the latest version of Apache
  yum:
    name: httpd
    state: latest

- name: ensure a list of packages installed
  yum:
    name: "{{ packages }}"
  vars:
    packages:
    - httpd
    - httpd-tools

- name: remove the Apache package
  yum:
    name: httpd
    state: absent

- name: install the latest version of Apache from the testing repo
  yum:
    name: httpd
    enablerepo: testing
    state: present

- name: install one specific version of Apache
  yum:
    name: httpd-2.2.29-1.4.amzn1
    state: present

```


`MODULE EASY_INSTALL`

> EASY_INSTALL    (/usr/lib/python2.7/site-packages/ansible/modules/packaging/language/easy_install.py)

Installs Python libraries, optionally in a `virtualenv'

```
EXAMPLES:
# Examples from Ansible Playbooks
- easy_install:
    name: pip
    state: latest

# Install Bottle into the specified virtualenv.
- easy_install:
    name: bottle
    virtualenv: /webapps/myapp/venv
```

`MODULE YUM_REPOSITORY`

> YUM_REPOSITORY    (/usr/lib/python2.7/site-packages/ansible/modules/packaging/os/yum_repository.py)

        Add or remove YUM repositories in RPM-based Linux distributions. If you wish to update an existing repository definition use
        [ini_file] instead.


```
EXAMPLES:
- name: Add repository
  yum_repository:
    name: epel
    description: EPEL YUM repo
    baseurl: https://download.fedoraproject.org/pub/epel/$releasever/$basearch/

- name: Add multiple repositories into the same file (1/2)
  yum_repository:
    name: epel
    description: EPEL YUM repo
    file: external_repos
    baseurl: https://download.fedoraproject.org/pub/epel/$releasever/$basearch/
    gpgcheck: no

- name: Add multiple repositories into the same file (2/2)
  yum_repository:
    name: rpmforge
    description: RPMforge YUM repo
    file: external_repos
    baseurl: http://apt.sw.be/redhat/el7/en/$basearch/rpmforge
    mirrorlist: http://mirrorlist.repoforge.org/el7/mirrors-rpmforge
    enabled: no
```