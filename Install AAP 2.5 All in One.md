## Alistamiento de la maquina previo a la instalacion

Requerimientos minimos para la instalacion
RAM: 16 GB

En una maquina con RHEL 9.2 ejecute los siguietnes pasos:

1. Alistamiento de la maquina:
  - Cree un usuario llamado aap

```  
[root@localhost ~]# useradd aap
[root@localhost ~]# passwd aap
Changing password for user aap.
New password:
Retype new password:
passwd: all authentication tokens updated successfully.
```
2. Adicione privilegios de sudo al usuario aap
```
[root@localhost ~]# visudo
```

En la ultima linea del archivo adicionar:
```
%aap    ALL=(ALL)       NOPASSWD: ALL
```

3. Pruebe que la linea de sudo funcione correctamente, para ello, ingrese con usuario aap a la maquina y ejecute el siguiente comando:
```
[aap@localhost ~]$ sudo whoami
root
```
El resultado debe ser la salida del usuario root

4. Suscriba la maquina con un usuario valido de Red Hat
```
[aap@localhost ~]$ sudo subscription-manager register
Registering to: subscription.rhsm.redhat.com:443/subscription
Username: jcalvo@redhat.com
Password:
The system has been registered with ID: fac22ae7-d5c2-4688-906f-a6a6fbbc1678
The registered system name is: localhost.localdomain
```
Una vez registrada se debe asignar una suscripcion, para facilidad, puede asignar una suscripcion de forma automatica
```
[aap@localhost ~]$ sudo subscription-manager attach --auto
Installed Product Current Status:
Product Name: Red Hat Enterprise Linux for ARM 64
Status:       Subscribed
```

Para validar que tiene las suscripciones, revise los repositorios habilitados
```
[aap@localhost ~]$ dnf repolist
Not root, Subscription Management repositories not updated
repo id                                                       repo name
rhel-9-for-aarch64-appstream-rpms                             Red Hat Enterprise Linux 9 for ARM 64 - AppStream (RPMs)
rhel-9-for-aarch64-baseos-rpms                                Red Hat Enterprise Linux 9 for ARM 64 - BaseOS (RPMs)
```
### Nota: Es muy importante que la instalacion se realize con un usuario que NO sea root y tampoco que se pase de usuario con sudo, se debe generar un ingreso directo por la pantalla de login o por ssh directamente con el usuario

## Pasos de instalacion 
Continuar con la documentacion oficial o siga estos pasos que son tomados de la documentacion:

```
[aap@localhost ~]$ tar xvzf  ansible-automation-platform-containerized-setup-bundle-*

[aap@localhost ~]$ hostname
aap26.jmanuelcalvo.com

[aap@localhost ~]$ sudo vi /etc/hosts
127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4  aap26.jmanuelcalvo.com


[aap@localhost ~]$ sudo dnf install -y ansible-core git wget rsync
Updating Subscription Management repositories.
Red Hat Enterprise Linux 9 for ARM 64 - AppStream (RPMs)                                                           25 MB/s |  40 MB     00:01
Red Hat Enterprise Linux 9 for ARM 64 - BaseOS (RPMs)                                                              30 MB/s |  36 MB     00:01
Dependencies resolved.
==================================================================================================================================================
 Package                              Architecture          Version                         Repository                                       Size
==================================================================================================================================================
Installing:
 ansible-core                         aarch64               1:2.14.14-1.el9                 rhel-9-for-aarch64-appstream-rpms               2.6 M
...
...
```

Configuracion del archivo de inventario de Ansible Automation Platform

```
[aap@localhost ~]$ tar xvfzp ansible-automation-platform-containerized-setup-bundle-2.4-2-aarch64.tar.gz
[aap@localhost ~]$ cd ansible-automation-platform-containerized-setup-bundle-2.4-2-aarch64
[aap@localhost ~]$ cp inventory-growth inventory
[aap@localhost ansible-automation-platform-containerized]$ sed -i 's/aap.example.org/aap26.jmanuelcalvo.com/g' inventory
[aap@localhost ansible-automation-platform-containerized]$ sed -i 's/<set your own>/redhat/g' inventory
[aap@localhost ansible-automation-platform-containerized]$ vim inventory

Descomentarear en la linea 49 client_request_timeout para evitar fallos en la instalacion

[all:vars]
# Ansible
ansible_connection=local
client_request_timeout=240



```

Y posteriormente ejecutar el comando de instalacion

```
[aap@localhost ansible-automation-platform-containerized]$ ansible-playbook -i inventory ansible.containerized_installer.install
```


## Troubleshooting
En caso que falle la implementacion por temas de timeout en postgres, recuerde descomentariar las linea de postgresql en el archivo de inventory
https://access.redhat.com/solutions/7052692


# -----------------------------------------------------


En caso de presentarte un error con IPv6 y el nombre del host, Ensure automation gateway hostname is not IPv6 when automation hub is present]
Yo hice este pequeño hack
```
TASK [ansible.containerized_installer.preflight : Ensure automation gateway hostname is not IPv6 when automation hub is present] *************************************************************************************
fatal: [localhost]: FAILED! => {"msg": "The conditional check '_gateway_hostname is search(\"\\\\.\")' failed. The error was: error while evaluating conditional (_gateway_hostname is search(\"\\\\.\")): {{ _gateway_url | urlsplit('hostname') }}: {{ gateway_main_url | default('http://' + (_raw_hostname | ansible.containerized_installer.ipv6_wrap) + ':443') }}: {{ hostvars[groups['automationgateway'][0]].routable_hostname | default(hostvars[groups['automationgateway'][0]].ansible_host) }}: 'ansible.vars.hostvars.HostVarsVars object' has no attribute 'ansible_host'. 'ansible.vars.hostvars.HostVarsVars object' has no attribute 'ansible_host'. {{ hostvars[groups['automationgateway'][0]].routable_hostname | default(hostvars[groups['automationgateway'][0]].ansible_host) }}: 'ansible.vars.hostvars.HostVarsVars object' has no attribute 'ansible_host'. 'ansible.vars.hostvars.HostVarsVars object' has no attribute 'ansible_host'. {{ gateway_main_url | default('http://' + (_raw_hostname | ansible.containerized_installer.ipv6_wrap) + ':443') }}: {{ hostvars[groups['automationgateway'][0]].routable_hostname | default(hostvars[groups['automationgateway'][0]].ansible_host) }}: 'ansible.vars.hostvars.HostVarsVars object' has no attribute 'ansible_host'. 'ansible.vars.hostvars.HostVarsVars object' has no attribute 'ansible_host'. {{ hostvars[groups['automationgateway'][0]].routable_hostname | default(hostvars[groups['automationgateway'][0]].ansible_host) }}: 'ansible.vars.hostvars.HostVarsVars object' has no attribute 'ansible_host'. 'ansible.vars.hostvars.HostVarsVars object' has no attribute 'ansible_host'. {{ _gateway_url | urlsplit('hostname') }}: {{ gateway_main_url | default('http://' + (_raw_hostname | ansible.containerized_installer.ipv6_wrap) + ':443') }}: {{ hostvars[groups['automationgateway'][0]].routable_hostname | default(hostvars[groups['automationgateway'][0]].ansible_host) }}: 'ansible.vars.hostvars.HostVarsVars object' has no attribute 'ansible_host'. 'ansible.vars.hostvars.HostVarsVars object' has no attribute 'ansible_host'. {{ hostvars[groups['automationgateway'][0]].routable_hostname | default(hostvars[groups['automationgateway'][0]].ansible_host) }}: 'ansible.vars.hostvars.HostVarsVars object' has no attribute 'ansible_host'. 'ansible.vars.hostvars.HostVarsVars object' has no attribute 'ansible_host'. {{ gateway_main_url | default('http://' + (_raw_hostname | ansible.containerized_installer.ipv6_wrap) + ':443') }}: {{ hostvars[groups['automationgateway'][0]].routable_hostname | default(hostvars[groups['automationgateway'][0]].ansible_host) }}: 'ansible.vars.hostvars.HostVarsVars object' has no attribute 'ansible_host'. 'ansible.vars.hostvars.HostVarsVars object' has no attribute 'ansible_host'. {{ hostvars[groups['automationgateway'][0]].routable_hostname | default(hostvars[groups['automationgateway'][0]].ansible_host) }}: 'ansible.vars.hostvars.HostVarsVars object' has no attribute 'ansible_host'. 'ansible.vars.hostvars.HostVarsVars object' has no attribute 'ansible_host'"}
```

En el archivo de inventario puse lo siguiente
```
[aap@localhost ansible-automation-platform-containerized]$ vim inventory

[all]
aap26.jmanuelcalvo.com ansible_host=127.0.0.1
``` 
