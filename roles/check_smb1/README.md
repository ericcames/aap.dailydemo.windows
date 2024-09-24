check_smb1
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

  roles:

    - role: check_smb1
```
License
-------

https://opensource.org/license/mit
