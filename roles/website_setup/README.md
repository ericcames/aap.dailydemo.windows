website_setup
=========
```
Create our custom website
```
Requirements
------------
```
Windows Machine Credential
```
Role Variables
--------------
```
website_setup_message: "This website is running on Microsoft Windows Server 2022 Datacenter"
ansible_python_interpreter: /usr/bin/python3
```
Dependencies
------------

Example Playbook
----------------
```
---
- name: Create our custom website
  hosts: windemo

  roles:

    - role: website_setup
```
License
-------

https://opensource.org/license/mit
