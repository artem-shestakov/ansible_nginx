Ansible Nginx role
=========
![Build](https://github.com/artem-shestakov/ansible_nginx/actions/workflows/ci.yml/badge.svg)

Ansible role to install Nginx on Debian/Red Hat OS family.

Requirements
------------
None

## Variables
--------------
This role uses three type of variables. First and second depends on installation type. Third is general variables regardless of the choice of the installation type.

### General variables
* **install_from** - The type of Nginx instalation. [source | repo]. Default: `repo`
* **nginx_http_add** - List of additioanl params that will be added to the http section of Nginx configuration file. Default: []
* **nginx_stream_add** - List of additioanl params that will be added to the stream section of Nginx configuration file. Default: []

### Web server
* **nginx_virtual_servers** - list of virtual servers. Default: []
  * **listen** - specify the IP address and port (or Unix domain socket and path) on which the server listens for requests
  * **server_name** - list of names of a virtual server
  * **locations** - list of [locations](https://nginx.org/en/docs/http/ngx_http_core_module.html#location)
    * **location** - configuration depending on a request URI.
    * **params** - sets parameters for location. For example `proxy_pass`, `return`, `rewrite` and etc.

### TCP and UDP Load Balancing
* **tcp_udp_nlb** List of dictionaries of (upstreams) backend servers for TCP and UDP load balancing with fields
  * **name** - upstream's name
  * **listen** - port for listining incoming traffic on nginx proxy
  * **servers** - list of backend's servers
* **stub_status** - If true role will setting Nginx monitoring tool by [ngx_http_stub_status_module](https://nginx.org/libxslt/en/docs/http/ngx_http_stub_status_module.html?_ga=2.119409609.1948393560.1630311972-884386294.1625301122). Default: `false`
* **stub_status_settings** Dictionary of stub_status settings with fields
  * **listen** - Sets the address and port for IP, or the path for a UNIX-domain socket on which the server will accept requests. Default: `127.0.0.1:80`
  * **server_name** - Sets names of a virtual server. Default: `127.0.0.1`
  * **location** - Sets configuration depending on a request URI. Default: `/nginx_status`

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
          listen: 
            - 192.168.1.1:443
            - 192.168.1.2:443
          servers:
            - 10.79.1.196:443
        - name: example2
          listen: 
            - 10002
          servers:
            - 10.79.1.203:5601

```

License
-------
BSD, MIT


Author Information
------------------
Artem Shestakov (artem.s.shestakov@gmail.com)
