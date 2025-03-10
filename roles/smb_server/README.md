smb_server
=========
```
Check the SMB Server Configuration
```
https://learn.microsoft.com/en-us/security-updates/securitybulletins/2017/ms17-010

Requirements
------------
```
Windows Machine Credential
```
Role Variables
--------------
```
```
Dependencies
------------

Example Playbook
----------------
```
---
- name: Check the SMB Server Config
  hosts: windemo
  connection: local

  tasks:

    - name: Include the smb_server role
      tags:
        - check
      ansible.builtin.include_role:
        name: smb_server

or

---
- name: Disable the samba server
  hosts: windemo
  connection: local

  tasks:

    - name: Include the smb_server role
      tags:
        - disable
      ansible.builtin.include_role:
        name: smb_server
```
License
-------

https://opensource.org/license/mit
