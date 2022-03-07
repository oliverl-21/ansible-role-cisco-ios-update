# Cisco IOS Update role - Work in Progress

use at your own risk!

Ansible role to update Cisco IOS-XE devices

Supported Devices:

- Catalyst 8000v
- Catalyst 9800-CL, WiP

## Current Tasks

1. get current version and compare to target version
2. clean uncommited and unused firmware
3. copy image from server to device
4. unpack image in install mode on device
5. save and reboot
6. wait for device to be online again
7. verify version, commit update and cleanup

## ToDo

- support further devices
- error handling
- conditional tasks, pyats currently not supported for Apple m1 

## Requirements

Firmware Image on a for the device accesible server

## Role Variables

- ansible_network_cli_ssh_type: paramiko
- updserver: <Server:Port>
- updmethod: http
- updpath: /
- ansible_command_timeout: 900

### Catalyst 8000v variables

- updver8k: 'version'
- updfile8k: 'filename'

### Catalyst 9800 variables

- updverc9: 'filename'
- updfilec9: 'version'

## Dependencies

Roles: none
Collection:

- cisco.ios
- ansible.netcommon

## Example Playbook

ToDo

```yaml
---

- hosts: 8kv*
  roles:
    - cisco-ios-update
  vars:
    ansible_network_cli_ssh_type: paramiko
    updver8k: 17.07.01a
    updfile8k: c8000v-universalk9.17.07.01a.SPA.bin
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
