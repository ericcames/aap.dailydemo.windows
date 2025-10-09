inventory
=========
```
This role will update the inventory on the Controller
```
Requirements
------------
```
Admin Account on your Ansible Controller
```
Role Variables
--------------
```
inventory_name: AAP Managed Inventory
ansible_python_interpreter: /usr/bin/python3
#
# These variables are set in their role
# vm_name: ''
# vpc_region: ''
#
# Credential Types needed for this role
# Amazon Web Services
# Red Hat Ansible Automation Platform
```
Dependencies
------------

Example Playbook
----------------
```
---
- name: Add a VM to your inventory
  hosts: localhost
  connection: local

  tasks:

    - name: Include the inventory role
      tags:
        - create
      ansible.builtin.include_role:
        name: inventory

or

---
- name: Remove a vm from our inventory
  hosts: localhost
  connection: local

  tasks:

    - name: Include the vm role
      tags:
        - remove
      ansible.builtin.include_role:
        name: inventory
```
License
-------

https://opensource.org/license/mit