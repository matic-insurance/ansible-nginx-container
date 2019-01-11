nginx-container
=========
[![Build Status](https://travis-ci.org/matic-insurance/ansible-nginx-container.svg?branch=master)](https://travis-ci.org/matic-insurance/ansible-nginx-container)

Role used to download, configure and run nginx-alpine container

Requirements
------------

Ubuntu 14.04 is tested.

This role uses Ansible's docker module, so requirements are [the same](https://docs.ansible.com/ansible/docker_image_module.html#requirements-on-host-that-executes-module).

Role Variables
--------------

Here is the list of Required variables with default values:
```yaml
# List of configuration files to be rendered as templates into /etc/nginx/ folder
# - src: './files/default.conf.j2'
#   dst: 'conf.d/default.conf'
#   mode: 'ro' # optional
nginx_configuration_files: []
# List of folders to be mounted to the nginx from host:
# - src: '/var/www/static_site/'
#   dst: '/etc/static'
#   mode: 'ro' # optional
nginx_mounted_folders: []
# List of nginx ports to be mapped
nginx_ports_mapping: ['80:80']
```
These variables are optional and can be changed if needed
```yaml
#Name of the container when it's running
nginx_container_name: nginx
#Settings path where rendered nginx files will be located
nginx_settings_path: '{{ ansible_user_dir }}/{{ nginx_container_name }}'
#Container memory limit
container_memory_limit: 256m
# Docker repository image and tag
nginx_docker_image: nginx
nginx_docker_image_tag: 1-alpine
```

Dependencies
------------

No dependencies

Example Playbook
----------------

Simpliest playbook can be following:

```yaml
- hosts: webservers
  roles:
    - role: nginx-container
      nginx_configuration_files:
        - src: './files/default.conf.j2'
          dst: 'conf.d/default.conf'
```

This playbook will run nginx container with name `nginx` on webserver hosts 
and will render default.conf.j2 file from `./files/ngingx` into `/etc/nginx/conf.d` folder on container
container will expose only 80 port
 
Advanced playbook with certificate files and multiple ports:
```yaml
- hosts: webservers
  roles:
    - role: nginx-container
      nginx_container_name: 'application-nginx'
      nginx_ports_mapping: 
        - '80:80'
        - '443:443'
        - '88:80'
      nginx_configuration_files:
        - src: './files/default.conf.j2'
          dst: 'conf.d/default.conf'
      nginx_mounted_folders:
        - src: './files/certs/'
          dst: '/etc/certs'
          mode: 'ro'
```


License
-------

MIT

Author Information
------------------

Matic is a communication platform that connects lenders and borrowers originating a new home loan. A borrower now knows where they are in the loan process and what they need to do to complete the loan.
