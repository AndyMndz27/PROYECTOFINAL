Proyecto DevOps - InnovaSys 
Este proyecto automatiza la configuración de un servidor interno para la
startup ficticia InnovaSys utilizando Ansible.  
El servidor cumple dos funciones principales:
Intranet de bienvenida (Apache): muestra un mensaje
personalizado con el nombre de la empresa.
Repositorio de archivos compartido (Samba): recurso accesible
solo para el grupo de desarrolladores.
---
Requisitos previos
Ubuntu Server 24.04 (servidor gestionado).
Linux Lite u otra distro con Ansible instalado (nodo de control).
Conexión en red entre ambas máquinas.
---
Instalación de Ansible en el nodo de control
``` bash
sudo apt update
sudo apt install software-properties-common -y
sudo add-apt-repository --yes --update ppa:ansible/ansible
sudo apt install ansible -y

# Verificar instalación
ansible --version
```
---
Estructura del proyecto
``` bash
ansible/
└── innovasys/
    ├── inventory.ini
    ├── variables.yml
    ├── playbook.yml
    └── roles/
        ├── apache/
        │   ├── tasks/main.yml
        │   ├── handlers/main.yml
        │   └── templates/index.html.j2
        └── samba/
            ├── tasks/main.yml
            └── handlers/main.yml
```
---
Configuración
Inventario (`inventory.ini`)
``` ini
[servidores]
servidor-innovasys ansible_host=192.168.56.101 ansible_user=operador
```
Variables (`variables.yml`)
``` yaml
nombre_empresa: "InnovaSys"
usuarios:
  - devuser1
grupo: desarrolladores
directorio_samba: /srv/samba/proyectos
contrasena_samba: "Innova.2025"
mensaje_bienvenida: "<h1>Bienvenidos a la Intranet de {{ nombre_empresa }}</h1>"
```
---
Ejecución del playbook
``` bash
# Ejecución normal
ansible-playbook -i inventory.ini playbook.yml

# Ejecución con privilegios de superusuario
ansible-playbook -i inventory.ini playbook.yml --become --ask-become-pass
```
---
Configuración de Git
``` bash
git config --global user.name "Tu Nombre"
git config --global user.email "tu.email@example.com"
```
---
Verificación final
Acceder a `http://<IP_SERVIDOR>` desde el navegador y ver la página
personalizada.
Conectarse al recurso compartido en `smb://<IP_SERVIDOR>/Proyectos`
usando el usuario `devuser1` y la contraseña configurada.
