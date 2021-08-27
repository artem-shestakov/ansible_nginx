Ansible Nginx role
=========

Ansible role to install Nginx on Debian/Red Hat OS family.


Requirements
------------
None

Role Variables
--------------

* `mode` describes the operating mode the sync daemons. Has two value `master-master` or `master-backup`. Now work only `master-master`.
* `vip` List of VIP adresses of the cluster.
* `global_defs`:
  - `notification_email` - List of emails to notify from `Keepalived`.
  - `notification_email_from` - Email from address that will be in the header.
  - `smtp_server` - Remote SMTP server used to send notification email.
  - `smtp_connect_timeout` - SMTP server connection timeout in seconds.
* `vrrp_instance`
  -  `authentication`
      - `auth_type` - PASS - Simple password (suggested). AH - IPSEC (not recommended)). Default `PASS`.
      - `auth_pass` - Password for accessing vrrpd. Only the first eight (8) characters are used. If `auth_pass` has not specified, `authentication` will be missing.


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
