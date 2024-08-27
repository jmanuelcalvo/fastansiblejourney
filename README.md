# Fast Ansible Journey
![Arquitectura](images/arquitectura_aap.png)


## Alistamiento de la arquitectura

<details open>
<summary>Despliegue Opcion 1</summary>
<br>
Descargar maquina virtual en formato qcow2 y convertirla para su virtualizador preferido

La imagen lista con el AAP instalado y contenerizado se puede descargar en el siguiente link:
<br><br>
<pre>
<a href="https://www.dropbox.com/scl/fo/369gn1gj3onyfofbxie89/AMX9KU0Zpnv5ndZxhdL08K0?rlkey=o0hka6wp1wfhcsurjgb6cteux&st=olsw8sbd&dl=0" target="_blank">Imagen QCOW2 de AAP</a>
</pre>

Los datos de conexion a la maquina son:
```
root/ABCde123.

aap/ABCde123.
```  

Tenga en cuenta el formato de imagen qcow2 funciona sobre KVM, si usted utiliza otro virtualizador debe convertirla al formato de su virtualizador.

Ejemplo: Busque en google como convertir qcow2 a vmdk.

</details>

<details open>
<summary>Despliegue Opcion 2</summary>
<br>
Desplegar la maquina completa y realizar la instalacion del Ansible Automation Platform desde cero, para ellos se puede seguir esta guia de implementacion de la solucion contenerizada todo en uno:
<br><br>
<pre>
<a href="https://developers.redhat.com/articles/2023/11/30/install-containerized-ansible-automation-platform-rhel-92#">Instrucciones Ansible Platfom Contenerized</a>
</pre>
</details>

<details open>
<summary>Alistamiento Ansible Automation Platform</summary>
<br>
<h3> Prerrequisitos </h3>

<li>Contar con su maquinas Ansible Automation Platform desplegado sobre su plataforma de virtualizacion</li>
<li>Contar con un usuario en el portal de <a href="https://developers.redhat.com/register">Developers</a></li>
<li>Contar al menos con una maquina virtual adicional con sistema operativo Red Hat Enterprise Linux 8 o 9</li>
<li>Solicitar suscripcion de trial de Ansible Automation Platform
<li>Contar con un usuario y repositiorio publico o privado en Github para almacenar los playbooks</li>
<li>Suscribir Ansible Automation Platform con el trial <a href="https://XXXX/register">Ver Instrucciones</a></li>
<li>Crear <b>Inventario</b>I de maquinas de Ansible Automation Platform <a href="https://XXXX">Ver Instrucciones</a></li>
<li>Crear <b>Proyecto</b> y enlazaro con su repositorio de GIT <a href="https://XXXX">Ver Instrucciones</a></li>
<li>Crear <b>Credenciales</b> de maquina dentro de Ansible Automation Platform <a href="https://XXXX">Ver Instrucciones</a></li>
<br><br>

</details>


## RETOS
![Arquitectura](images/ansible_journey.png)

[Reto 1 - Parchado de maquinas](RETO1.md)

[Reto 2 - Aplicar reglas de compliance](RETO2.md)

[Reto 3 - Instalar y configurar agentes en Windows](RETO3.md)