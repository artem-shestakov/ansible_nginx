Ansible Nginx role
=========
Ansible role to install Nginx on Debian/Red Hat OS family.

Requirements
------------
None

## Variables
--------------
This role uses three type of variables. First and second depends on installation type. Third is general variables regardless of the choice of the installation type.

### General variables
* **install_from** - The type of Nginx instalation. [source | repo]. Default: repo
* **tcp_udp_nlb** List of dictionaries of (upstreams) backend servers for TCP and UDP load balancing with fields
  * **name** - upstream's name
  * **listen** - port for listining incoming traffic on nginx proxy
  * **servers** - list of backend's servers

### Install from repository variables

### Install from source variables
* **nginx_with_modules** - List of not default Nginx modules. List of modules is [here](https://docs.nginx.com/nginx/admin-guide/installing-nginx/installing-nginx-open-source/#sources). Default: []

Example Playbook
----------------
```yaml
---
- name: Install Nginx app
  hosts: all
  remote_user: vagrant
  become: true
  roles:
    - artem_shestakov.nginx
  vars:
    - install_from: source
    - nginx_with_modules:
        - --with-file-aio 
        - --with-ipv6 
        - --with-http_ssl_module
        - --with-http_v2_module 
        - --with-http_realip_module 
        - --with-http_addition_module 
        - --with-http_xslt_module=dynamic  
        - --with-http_image_filter_module=dynamic
        - --with-http_sub_module 
        - --with-http_dav_module 
        - --with-http_flv_module 
        - --with-http_mp4_module 
        - --with-http_gunzip_module 
        - --with-http_gzip_static_module 
        - --with-http_random_index_module 
        - --with-http_secure_link_module 
        - --with-http_degradation_module 
        - --with-http_slice_module 
        - --with-http_stub_status_module 
        - --with-http_perl_module=dynamic 
        - --with-http_auth_request_module 
        - --with-mail=dynamic 
        - --with-mail_ssl_module  
        - --with-stream=dynamic
        - --with-stream_ssl_module 
        - --with-debug
    - tcp_udp_nlb:
        - name: example
          listen: 10001
          servers:
            - 10.79.1.196:8500
        - name: example2
          listen: 10002
          servers:
            - 10.79.1.203:5601

```

License
-------
BSD, MIT


Author Information
------------------
Artem Shestakov (artem.s.shestakov@gmail.com)
