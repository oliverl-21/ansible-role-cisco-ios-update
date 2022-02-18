ansible-role-cisco-ios-update
=============================

Cisco IOS Update role - Work in Progress
---------------------

use at your own risk!

### Current Tasks

1. get current version and cleanup
2. copy image from server to device
3. unpack image in install mode
4. save and reboot
5. wait for device to be online again
6. verify version and commit update

### ToDo

support further devices

Requirements
------------

Firmware Image on a for the device accesible server

Role Variables
--------------

- ansible_network_cli_ssh_type: paramiko
- updver: 17.07.01a
- updfile: c8000v-universalk9.17.07.01a.SPA.bin
- updserver: <Server:Port>
- updmethod: http
- updpath: /
- ansible_command_timeout: 1800

Dependencies
------------

Roles: none

Collection:

- cisco.ios
- ansible.netcommon

Example Playbook
----------------

TODO

```yaml
---

- hosts: 8kv*
  roles:
    - cisco-ios-update
  vars:
    ansible_network_cli_ssh_type: paramiko
    updver: 17.07.01a
    updfile: c8000v-universalk9.17.07.01a.SPA.bin
    updserver: 198.18.168.3:8000
    updmethod: http
    updpath: /
    ansible_command_timeout: 1800


```

License
-------

GPL-3.0-or-later

Author Information
------------------

oliverl-21
