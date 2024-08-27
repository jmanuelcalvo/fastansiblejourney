
## Retos

![Parchado](images/reto1-parchado01.png)
infografia.png


Teniendo en cuenta que el primer reto se trata de realizar el parchado de máquinas Red Hat Linux versiones 8 o 9 a través de un proceso automatizado con Ansible y usando Ansible Automation Platform, Este reto debe resolver los siguientes puntos:

- Garantizar la conectividad de el **Ansible Automation Platform** con la máquina RHEL.
- Crear un playbook que se encargue de instalar actualizar los paquetes disponibles para la versión actual (no hacer actualización de versión, solo de paquetes de la versión actual).
- Subir el playbook a un repositorio de git
- Garantizar que el ambiente de Ansible Automation Platform se encuentre configurado para poder ejecutar los playbooks (proyectos, credenciales, inventarios, plantillas).
- Ejecutar el playbook para que se ejecute sobre la máquina virtual RHEL.
- Verificar que el sistema operativo se encuentre actualizado.

## Bonus:
Tareas adicionales:
- Garantizar que template se ejecute de forma desasistida o programada
- Garantizar que el template puede ser utilizado por un usuario que no sea el admin de **Ansible Automation Platform**.
- Garantizar que el playbook está escrito utilizando las mejores prácticas de Ansible. (ansible-lint)
- Configurar la plataforma para que se conecte con algún sistema de notificación, como por ejemplo slack o correo y se envíe una notificación en caso que la tarea falle.


