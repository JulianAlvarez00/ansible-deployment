# ansible-deployment
DevOps Jr Project 1

Proyecto de Automatización de Implementaciones con Ansible
Descripción
Este proyecto utiliza Ansible para automatizar la configuración y el despliegue de un servidor web Nginx en una máquina local. Es una excelente introducción a la automatización de despliegues y la gestión de configuraciones.

Requisitos
Linux (Debian/Ubuntu)
Ansible
Git
Nginx
Estructura del Proyecto
plaintext
Copiar código
ansible-deployment/
├── hosts
└── site.yml
hosts: Archivo de inventario que define los hosts en los que se ejecutarán los playbooks.
site.yml: Playbook de Ansible que contiene las tareas para configurar y desplegar Nginx.
Instalación
Paso 1: Clonar el Repositorio
bash
Copiar código
git clone https://github.com/TU_USUARIO/ansible-deployment.git
cd ansible-deployment
Paso 2: Instalar Ansible
bash
Copiar código
sudo apt update
sudo apt install ansible -y
Paso 3: Configurar el Archivo hosts
Asegúrate de que el archivo hosts contenga:

ini
Copiar código
[webservers]
localhost ansible_connection=local
Paso 4: Configurar el Playbook site.yml
Asegúrate de que el archivo site.yml contenga:

yaml
Copiar código
- hosts: webservers
  become: yes
  tasks:
    - name: Install Nginx
      apt:
        name: nginx
        state: present

    - name: Start Nginx
      service:
        name: nginx
        state: started

    - name: Create index.html
      copy:
        dest: /var/www/html/index.html
        content: "<h1>Hola Mundo desde Ansible!</h1>"
Ejecución
Paso 5: Ejecutar el Playbook
bash
Copiar código
ansible-playbook -i hosts site.yml --ask-become-pass
Verificación
Abre un navegador web y navega a http://localhost. Deberías ver el mensaje "Hola Mundo desde Ansible!".

Diagramas
Diagrama de Flujo
mermaid
Copiar código
graph LR
  A[Clonar Repositorio] --> B[Instalar Ansible]
  B --> C[Configurar Archivo hosts]
  C --> D[Configurar Playbook site.yml]
  D --> E[Ejecutar Playbook]
  E --> F[Verificar Despliegue en Navegador]
Arquitectura
mermaid
Copiar código
graph TD
  Client -->|HTTP Request| Nginx[Servidor Nginx]
  Nginx -->|Serve Content| Browser[Navegador]
Solución de Problemas
Error: bind() to 0.0.0.0:80 failed
Si recibes un error indicando que el puerto 80 está en uso, puedes:

Identificar y detener el proceso que usa el puerto 80:

bash
Copiar código
sudo lsof -i :80
sudo systemctl stop <nombre_del_proceso>
Cambiar el puerto de Nginx a uno diferente, como 8080:

bash
Copiar código
sudo nano /etc/nginx/sites-available/default
# Cambia listen 80; a listen 8080;
sudo systemctl restart nginx
Error: Unable to start service nginx
Verifica la configuración de Nginx:

bash
Copiar código
sudo nginx -t
Revisa los logs para más detalles:

bash
Copiar código
sudo journalctl -xeu nginx.service
Contribuciones
¡Las contribuciones son bienvenidas! Si tienes sugerencias o mejoras, por favor abre un issue o un pull request.

Licencia
Este proyecto está bajo la licencia MIT. Consulta el archivo LICENSE para más detalles.
