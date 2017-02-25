mm0.informatica
===============

[![Build Status](https://travis-ci.org/mm0/ansible-role-informatica.svg?branch=master)](https://travis-ci.org/mm0/ansible-role-informatica) [![Galaxy](https://img.shields.io/badge/galaxy-mm0.informatica-blue.svg?style=flat)](https://galaxy.ansible.com/mm0/informatica)


An Ansible Role that configures and silently installs Informatica.

Informatica will be installed at `/informatica/{{ environment_name }}`

Requirements
------------

* RHEL 6.8 (not compatible 7+)
* Accessible License Key File Path (`informatica.key_file`)
* DB credentials (`informatica.db`)
* Environment Name of your choosing (`environment_name`)
* Domain credentials, (`informatica.domain`)
* Timezone (`informatica.timezone`)
* Password (`informatica.keystore_password`)
* Unique Nodename ( Defaults to `nodename: "node1_{{ environment_name }}`")

* ~ 40GB of free space (used temporarily during installation as well as to extract the installer and hold the installer)
* If you have a second volume, you can set `informatica_temp_directory` to have the installation process use a temp directory on a separate drive
* Alternatively, you can set `informatica_root_directory` to the path to a directory with enough free space.
* Installer Archive Present on server (`informatica.archive` | `"/mnt/nfs/ansible/informatica/961HF3_Server_Installer_linux-x64.tar"`)

Role Variables
--------------

*All variables*

| Name              | Default Value       | Description          |
|-------------------|---------------------|----------------------|
| `environment_name` | `Dev1` | Self Explanatory |
| `informatica.group` | `"ifadmin"` | App Group |
| `informatica.user ` | `"ifadmin"` | App User |
| `informatica.users ` | `[{user: ifadmin,group: ifadmin}, { user: ifuser,group: ifuser}]` | App Users |
| `informatica.db.type` | `"Oracle"` | One of Oracle/MSSQLServer/DB2/Sybase |
| `informatica.db.username` | `"test"` | DB Credentials |
| `informatica.db.password` | `"test"` | DB Credentials |
| `informatica.db.address` | `"db.domain.com:1521"` | DB Address |
| `informatica.db.servicename` | `"servicename"` | DB Servicename |
| `informatica.domain.name` | `"{{ environment_name }}"` | DB Domain name |
| `informatica.domain.hostname` | `"testserver"` | DB Domain hostname |
| `informatica.domain.port` | `6005` | DB Domain port |
| `informatica.domain.user` | `user` | DB Domain User |
| `informatica.domain.password` | `password` | DB Domain Password |
| `informatica.domain.cnfrm_password` | `password` | DB Domain Password Confirm|
| `informatica.keystore_password` | `Password1` | Informatica Keystore Password |
| `informatica.serves_as_gateway` | `1` | Boolean, should serve as gateway node |
| `informatica.nodename` | `node1_test1` | Name of node |
| `informatica.base_directory` | `"{{ informatica_root_directory }}/{{ environment_name }}"` | Base directory for installation of Informatica |
| `informatica.archive` | `"/mnt/nfs/ansible/informatica/961HF3_Server_Installer_linux-x64.tar"` | Location of Installer archive |
| `informatica.key_file` | `"/mnt/nfs/ansible/informatica/my.key"` | Location of Key File |
| `informatica.timezone` | `"America/Los_Angeles"` | Timezone for app |
| `informatica.silentinput_properties_file` | `"SilentInput.properties.j2"` | Name of silentinput.properties template file to use for silent install |
| `informatica_directory` | `"{{ informatica.base_directory }}/sw"` | Software directory |
| `informatica_install_directory` | `"{{ informatica.base_directory }}/install_dir"` | Install directory |
| `informatica_enable_usage_collection` | `1` | Required for silent install |
| `informatica_encryption_key_directory` | `"{{ informatica_directory }}/isp/config/keys"` | Location of keys |
| `informatica_temp_directory` | `"/tmp"` | Pointer to temp directory incase /tmp is full |
| `informatica_root_directory` | `"/informatica"` | Root dir for all different environment installations of Informatica |


Dependencies
------------
- RHEL 6.8

Example Playbook
----------------

```yaml
- hosts: "{{ target }}"
  gather_facts: true
  vars:
    environment_name: Dev1
    informatica:
      key_file: /mnt/nfs/ansible/informatica/my.key
      nodename: "node_{{ environment_name }}"
  vars_files:
  - vault_files/informatica.yml
  roles:
  - {
      role: "mm0.informatica"
    }
```

vault_files/informatica.yml should contain:
```yaml
informatica:
  db:
    type: Oracle # Oracle/MSSQLServer/DB2/Sybase
    username: username
    password: password
    address: sub.hostname.com:1521
    servicename: gwif
  domain:
    name: Dev1
    hostname: test1
    port: 6005
    user: repo
    password: repo
    cnfrm_password: repo
  keystore_password: repo
```

License
-------

BSD

Author Information
------------------

[Matt Margolin](mailto:matt.margolin@gmail.com)

[mm0](https://github.com/mm0) on github