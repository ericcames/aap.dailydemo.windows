windows_patching
=========
```
Patch my windows systems
```
Requirements
------------
```
Administrator access on the machines in the inventory
```
Role Variables
--------------
```
reboot_server: Yes
categories:
  - Application
  - Connectors
  - CriticalUpdates
  - DefinistionUpdates
  - DeveloperKits
  - FeaturePAcks Guidance
  - SecurityUpdates
  - ServicePacks
  - Tools
  - UpdateRollups
  - Updates
```
Dependencies
------------

Example Playbook
----------------
```
---
- name: Windows patching
  hosts: windemo

  roles:

    - role: windows_patching
```
License
-------

https://opensource.org/license/mit
