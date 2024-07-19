auditme
=========
```
Audit my windows systems
```
Requirements
------------
```
Administrator access on the machines in the inventory
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
- name: Audit Windows system
  hosts: windemo
  gather_facts: true

  roles:

    - role: auditme
```
License
-------

https://opensource.org/license/mit
