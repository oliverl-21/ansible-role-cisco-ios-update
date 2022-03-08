# Cisco IOS Update role - Work in Progress

use at your own risk!

Ansible role to update Cisco IOS-XE Devices

tested Devices:

- Catalyst 8000v
- Catalyst 9800-CL, WiP

## Current Tasks

- get current Version and compare to target Version
- clean uncommited and unused firmware if the device is an IOS-XE Device, else skip
- assert that the Device is an IOS-XE and runs at least 16.10.01
- if true all following tasks will run only if current version and target version are different
  - copy image from server to device
  - unpack image in install mode on device
  - save and reboot
  - wait for device to be online again
  - verify version, commit update and cleanup

## ToDo

- test further Devices
- error handling
- conditional tasks, pyats currently not supported for Apple m1
- implement old xe mechanism prior to v16.10.01

## Requirements

Firmware Image on a for the device accesible server (e.g. TFTP, HTTP, FTP)

## Role Variables

- ansible_network_cli_ssh_type: paramiko
- updserver: <Server:Port>
- updmethod: http/ftp/tftp
- updpath: /
- ansible_command_timeout: 900

### Update File Variables

- updver: 'version' # define your version here like 17.06.02

> **Note:**
> Filename is build from updver and Ansible Facts net_model.
> Be aware of lowercase and uppercase letters. In most tested Scenarios the facts are upper but some files are lowercase.

```yml
# defaults/main.yml
---
updfile: '{{ ansible_facts.net_model }}-universalk9.{{ updver }}.SPA.bin'
```

## Dependencies

Roles: none
Collection:

- cisco.ios
- ansible.netcommon

## Example Playbook

ToDo

```yaml
# playbook.yml
---

- hosts: 8kv*
  roles:
    - cisco-ios-update
  vars:
    ansible_network_cli_ssh_type: paramiko
    updver: 17.07.01a
    updserver: 198.18.168.3:8000
    updmethod: http
    updpath: /
    ansible_command_timeout: 900
```

to provide an update repository for the devices you can start a simple web server with ```pyhton3 -m http.server```. There is also the option for ```net_put``` from ansible-netcommon but scp is slow due to default copp policies on the Cisco devices. In my tests transfering a 1GB file took 9min via SCP versus 2min over HTTP.

## License

GPL-3.0-or-later

## Author Information

oliverl-21
