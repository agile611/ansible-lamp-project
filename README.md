# Agile611 Ansible Lamp Project
[![Agile611](https://www.agile611.com/wp-content/uploads/2020/09/cropped-logo-header.png)](http://www.agile611.com/)

**Ejemplo completo** y listo para usar de cómo desplegar en la arquitectura (arquitectura-ansible.pdf) con **Ansible**, usando:

- **Apache2** y **PHP** en el webserver (**192.168.11.40**)  
- **MySQL** en database (**192.168.11.20**)  
- **Nginx** como balanceador en loadbalancer (**192.168.11.30**)  
- **WordPress** instalado y configurado  
- **Ansible** ejecutándose desde control (**192.168.11.10**, solo para lanzar los playbooks)

---
Clone this repository with:

```shell
git clone https://github.com/agile611/ansible-lamp-project.git
```

## Configuración inicial

* Para iniciar el entorno, vamos a necesitar 4 VMs de Ubuntu (ansible, loadbalancer, database y webserver)

```shell
vagrant up 
vagrant ssh ansible
```

* Iniciamos el espacio de trabajo y conectamos las máquinas desde la arquitectura

```shell
vagrant@ansible$ ssh-keygen
vagrant@ansible$ ssh-copy-id vagrant@192.168.11.20
vagrant@ansible$ ssh-copy-id vagrant@192.168.11.30
vagrant@ansible$ ssh-copy-id vagrant@192.168.11.40
```

---

# 📁 Estructura de directorios

```
lamp-ansible/
├── group_vars/
│   ├── all.yml
│   ├── dbservers.yml
│   └── webservers.yml
├── inventory.ini
├── playbooks/
│   └── site.yml
├── roles/
│   ├── apache/
│   │   ├── handlers/
│   │   │   └── main.yml
│   │   ├── tasks/
│   │   │   └── main.yml
│   │   └── templates/
│   │       └── wordpress.conf.j2
│   ├── mysql/
│   │   └── tasks/
│   │       └── main.yml
│   ├── php/
│   │   └── tasks/
│   │       └── main.yml
│   ├── nginx/
│   │   ├── handlers/
│   │   │   └── main.yml
│   │   ├── tasks/
│   │   │   └── main.yml
│   │   └── templates/
│   │       └── nginx.conf.j2
│   └── app/
│       ├── tasks/
│       │   └── main.yml
│       └── templates/
│           └── wp-config.php.j2
├── vault/
│   └── secrets.yml
└── README.md
```

---

# 🗂️ Inventario

`inventory.ini`
```
[dbservers]
database ansible_host=192.168.11.20

[webservers]
webserver ansible_host=192.168.11.40

[loadbalancer]
loadbalancer ansible_host=192.168.11.30
```

---

# 🚦 ¿Qué instala cada máquina?

- **database**: MySQL, base de datos y usuario para WordPress
- **webserver**: Apache2, PHP y módulos, WordPress instalado y configurado
- **loadbalancer**: Nginx como reverse proxy hacia webserver
- **control**: Solo Ansible y tus playbooks/roles

---

# 🚀 ¿Cómo usarlo?

1. Prepara las 4 máquinas con Ubuntu Server y acceso SSH.
2. Clona este repositorio de Ansible en la máquina control.
3. Rellena las IPs en `inventory.ini` y ajusta variables según tus necesidades.
4. Cifra los secretos (opcional para que pase a ser productivo):
   ```sh
   ansible-vault encrypt vault/secrets.yml
   ```
5. Ejecuta el playbook:
   ```sh
   ansible-playbook -i inventory.ini playbooks/site.yml
   ```
6. Accede a `http://192.168.11.30` y deberías ver el instalador de WordPress.

---
## Soporte

Este tutorial ha sido liberado al dominio público por [Agile611](http://www.agile611.com/) bajo la licencia **Creative Commons Attribution-NonCommercial 4.0 International**.

[![Licencia: CC BY-NC 4.0](https://img.shields.io/badge/License-CC_BY--NC_4.0-lightgrey.svg)](https://creativecommons.org/licenses/by-nc/4.0/)

Este archivo README fue escrito originalmente por [Guillem Hernández Sola](https://www.linkedin.com/in/guillemhs/) y también ha sido liberado al dominio público.

Por favor, contacta con Agile611 para más detalles.

* [Agile611](http://www.agile611.com/)
* Laureà Miró 309  
* 08950 Esplugues de Llobregat (Barcelona)