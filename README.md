Ansible Nginx role
=========

Ansible role to install Nginx on Debian/Red Hat OS family.


Requirements
------------
None

TCP and UDP Load Balancing
--------------

* **tcp_udp_nlb** List of dictionaries of (upstreams) backend servers for TCP and UDP load balancing with fields
  * **name** - upstream's name
  * **listen** - port for listining incoming traffic on nginx proxy
  * **servers** - list of backend's servers


Example Playbook
----------------
Example of setup MASTERs on two nodes cluster with IP 10.0.0.1-2 and VIP 10.0.0.3-4.

```yaml
---
- name: Install Nginx app
  hosts: all
  remote_user: vagrant
  become: yes
  roles:
    - artem_shestakov_nginx
  vars:
    - tcp_udp_nlb:
        - name: dns
          listen: 53
          servers:
            - 1.2.3.4:53
            - 5.6.7.8:53
        - name: ntp
          listen: 123 udp
          servers:
            - 10.11.12.13:123
            - 14.15.16.17:123

```
Inventory file:
```ini
[proxy]
10.0.3.31
10.0.3.32
```

License
-------
BSD, MIT


Author Information
------------------
Artem Shestakov (artem.s.shestakov@gmail.com)
