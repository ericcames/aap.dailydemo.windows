update_imdsv2_setting
=========
```
This role sets the value of the IMDSv2.
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
vpc_name: aap_containerized_deployment
user_name: mickey.mouse
subnet_name: "{{ vpc_name }}_Subnet"
image: ami-06127bea7af8a9ad8
count: 1
region: us-west-1
assign_public_ip: yes
alwaysup: false
instance_type: m5.xlarge
ec2_security_group_name: "{{ vpc_name }}_SECGRP"
ec2_ansible_group: "{{ user_name }}"
my_email_address: "{{ user_name }}@redhat.com"
servername: Windows Daily Demo
http_tokens_value: required
```
Dependencies
------------

Example Playbook
----------------
```
---
- name: Setting the IDMSv2 setting
  hosts: localhost
  connection: local

  roles:

    - role: update_imdsv2_setting
```
License
-------

https://opensource.org/license/mit
