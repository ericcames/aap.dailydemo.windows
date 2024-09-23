dot_net_patch_info
=========
```
How to determine which .NET Framework security updates and hotfixes are installed
```
https://learn.microsoft.com/en-us/dotnet/framework/migration-guide/how-to-determine-which-net-framework-updates-are-installed

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
- name: .Net Patch Report
  hosts: windemo
  connection: local

  roles:

    - role: dot_net_patch_info
```
License
-------

https://opensource.org/license/mit
