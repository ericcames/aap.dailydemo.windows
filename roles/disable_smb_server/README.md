disable_smb_server
=========
```
disable the SMB Server Configuration
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
- name: Disable the SMB Server
  hosts: windemo
  connection: local

  roles:

    - role: disable_smb_server
```
License
-------

https://opensource.org/license/mit
