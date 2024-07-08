powershell_improvement
=========
```
Windows Performance
```
https://docs.ansible.com/ansible/latest/os_guide/windows_performance.html

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
- name: Improve the way powershell works
  hosts: windemo
  connection: local

  roles:

    - role: powershell_improvement
```
License
-------

https://opensource.org/license/mit
