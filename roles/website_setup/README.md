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
iis_sites:
  - name: 'AnsibleDemo'
    port: '80'
    path: 'C:\sites\dailydemo'
iis_test_message: "Hello World!  My Demo IIS Server"
sales_email_id: mickey
tech_email_id: goofy
sales_person_name: "Mickey Mouse"
tech_person_name: "Goofy"
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
