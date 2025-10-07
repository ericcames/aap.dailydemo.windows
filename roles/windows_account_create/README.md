windows_account_create
=========
```
This role will create user accounts
```
Requirements
------------
```
Administrator access on the machines in the inventory
Vault Credential in Ansible Automation Platform
Vaulted file in the format of files/user_list_example.yml
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
- name: Windows account creation
  hosts: windemo

  roles:

    - role: windows_account_create
```
License
-------

https://opensource.org/license/mit
