[![Release](https://github.com/oliverl-21/ansible-role-cisco-ios-update/actions/workflows/release.yml/badge.svg)](https://github.com/oliverl-21/ansible-role-cisco-ios-update/actions/workflows/release.yml)

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

- ansible_network_cli_ssh_type: ```paramiko```
- updserver: ```Server:Port```
- updmethod: ```http, ftp, tftp```
- updpath: ```/path/to/image```
- ansible_command_timeout: ```900```

To Send a Webex a notification based on the state of the task the following can be configured. If the ```webex_Recipient_id``` is empty the tasks are skipped.

- webex_token: ```webex api token```
- webex_recipient_type: ```toPersonEmail```
- webex_recipient_id: ```Email of Recipient```

further options: [Webex Module Documentation](https://docs.ansible.com/ansible/latest/collections/community/general/cisco_webex_module.html)

### Update File Variables

- updver: ```version number``` define your version here like 17.06.02

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
- community.general.cisco_webex

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
    webex_token: # your Webex API token
    webex_recipient_id: # leave empty if webex shouldn't be used
```

to provide an update repository for the devices you can start a simple web server with ```pyhton3 -m http.server```. There is also the option for ```net_put``` from ansible-netcommon but scp is slow due to default copp policies on the Cisco devices. In my tests transfering a 1GB file took 9min via SCP versus 2min over HTTP on a 1Gbit/s connection.

## Sample Output

![C9800-CL Sample](c9800-cl-sample.png)

## License

GPL-3.0-or-later

## Author Information

oliverl-21
