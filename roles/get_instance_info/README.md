get_instance_info
=========
```
This role will gather ec2 info.
```
Requirements
------------
```
Amazon Web Console Account
Amazon Web Services Credential in Ansible Automation Platform
```
Role Variables
--------------
```
ansible_python_interpreter: /usr/bin/python3
```
Dependencies
------------

Example Playbook
----------------
```
---
- name: Gather machine info
  hosts: localhost
  connection: local

  roles:

    - role: get_instance_info
```
License
-------

https://opensource.org/license/mit
