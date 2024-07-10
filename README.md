Ansible Automation Platform Daily Demo for Windows
=========
A demo designed to showcase many of the use cases that people are looking for.  We are using the workflow visualizer to show how the various building blocks are put together and enable the delivery on demand of a custom website.  The playbooks call roles, the roles allow for ease of sharing the code and also allow for documentation of the various things needed in each role. The demo is designed to be integrated with an IT Service Management (ITSM) system.  Everything will be documented in ITSM system via the skillfull use of automation.  Check out the video below to see that "the art of the possible."

# The workflow

![alt text](https://github.com/ericcames/aap.dailydemo.windows/blob/main/images/windowsworkflow.png "Windows workflow")

**The playbooks**

[1. Create our network container](https://github.com/ericcames/aap.dailydemo.windows/blob/main/playbooks/create_vpc_01.yml "create_vpc_01.yml") <br>
[2. Create our virtual machine](https://github.com/ericcames/aap.dailydemo.windows/blob/main/playbooks/create_instance_02.yml "create_instance_02.yml")<br>
[3. Update our inventory](https://github.com/ericcames/aap.dailydemo.windows/blob/main/playbooks/add_inventory_03.yml "add_inventory_03.yml")<br>
- Custom Ansible Controller Credential
```
Input configuration

fields:
  - id: url
    type: string
    label: Controller URL
  - id: user
    type: string
    label: Controller Username
  - id: password
    type: string
    label: Controller Password
    secret: true
required:
  - url
  - user
  - password
```
```
Injector configuration

extra_vars:
  controller_url: '{{url}}'
  controller_user: '{{user}}'
  controller_passwd: '{{password}}'
```
[4. Gather instance information](https://github.com/ericcames/aap.dailydemo.windows/blob/main/playbooks/get_instance_info_04.yml "get_instance_info_04.yml")<br>
[5. Powershell Improvment](https://github.com/ericcames/aap.dailydemo.windows/blob/main/playbooks/powershell_improve_05.yml "powershell_improve_05.yml")<br>
[6. User access](https://github.com/ericcames/aap.dailydemo.windows/blob/main/playbooks/windows_account_create_06.yml "windows_account_create_06.yml")<br>
[6. Website deployment](https://github.com/ericcames/aap.dailydemo.windows/blob/main/playbooks/website_setup_06.yml "website_setup_06.yml")<br>
[7. Patching](https://github.com/ericcames/aap.dailydemo.windows/blob/main/playbooks/provision_user_access_07.yml "windows_patching_07.yml")<br>
[8. Send notification that the website is ready](https://github.com/ericcames/aap.dailydemo.windows/blob/main/playbooks/sendmail_10.yml "sendmail_10.yml")<br>
- Custom Mail Server credential
```
Input configuration

fields:
  - id: smtp_server
    type: string
    label: Mail Server
  - id: smtp_port
    type: string
    label: Mail Server Port
  - id: smtp_username
    type: string
    label: Mail Server Username
  - id: smtp_password
    type: string
    label: Mail Server Password
    secret: true
required:
  - smtp_server
  - smtp_port
  - smtp_username
  - smtp_password
```
```
Injector configuration

extra_vars:
  MAILHOST: '{{smtp_server}}'
  MAILHOST_PORT: '{{smtp_port}}'
  MAILHOST_PASSWORD: '{{smtp_password}}'
  MAILHOST_USERNAME: '{{smtp_username}}'
```
[Site Delete will clean everything up](https://github.com/ericcames/aap.dailydemo.windows/blob/main/playbooks/site_delete.yml "site_delete.yml")<br>

ServiceNow
========

**The playbooks**

[Create a CMDB record](https://github.com/ericcames/aap.dailydemo.windows/blob/main/playbooks/servicenow/create_ci.yml "create_ci.yml") <br>
[Create a CMDB relationship](https://github.com/ericcames/aap.dailydemo.windows/blob/main/playbooks/servicenow/create_cmdb_relationship.yml "create_cmdb_relationship.yml") <br>
[Create incident ticket](https://github.com/ericcames/aap.dailydemo.windows/blob/main/playbooks/servicenow/incident_create.yml "incident_create.yml") <br>
[Update requested item ticket](https://github.com/ericcames/aap.dailydemo.windows/blob/main/playbooks/servicenow/update_sn_req_itm.yml "update_sn_req_itm.yml") <br>

- ServiceNow credential
```
Input configuration

fields:
  - id: instance
    type: string
    label: Instance
  - id: username
    type: string
    label: username
  - id: password
    type: string
    label: password
    secret: true
required:
  - instance
  - username
  - password
```
```
Injector configuration

env:
  SN_HOST: '{{instance}}'
  SN_PASSWORD: '{{password}}'
  SN_USERNAME: '{{username}}'
```
- ServiceNow Ansible spoke setup

[Ansible spoke setup - Alex Dworjan](https://github.com/shadowman-lab/Ansible-SNOW/tree/master/SNOWSetup#servicenowaap-integration-instructions-using-ansible-spoke "Ansible spoke setup - Alex") <br>
[Ansible spoke youtube - Alex Dworjan](https://www.youtube.com/watch?v=DmPXiRHjgRY "Ansible spoke youtube - Alex Dworjan") <br>

- ServiceNow Ansible spoke setup additional Ansible controllers
```
Flow Designer -> Connections -> Add Connection

Connection Name: ericamesAAPalias
Connection URL: https://ericames.ddns.net
Credential Name: Eric Ames AAP Spoke Credentials
Application Registry Name: Eric Ames Spoke Registry
OAuth Client ID: %SECRETID%
OAuth Client Secret: %SECRETGOESHERE%
Oauth Entity Profile Name: Eric Ames Spoke Registry default_profile
OAuth Entity Scope: write
Authorization URL: https://ericames.ddns.net/api/o/authorize/
Token URL: https://ericames.ddns.net/api/o/token/
OAuth Redirect URL: https://ven05433.service-now.com/api/sn_ansible_spoke/ansible_oauth_redirect

```
- Automated incident management example

[Example Error Handling in support of incident enrichment](https://github.com/ericcames/aap.dailydemo.windows/blob/main/roles/instance_create_aws/tasks/main.yml "Example Error Handling") <br>
[Youtube video on Automated Incident enrichment](https://youtu.be/ieO-cbzNqjU?si=z28o3rpAgLTDqdnB "Youtube video on Automated Incident enrichment") <br>



```
- name: Adding incident management error handling
  block:

    PUT YOUR TASKS HERE

  rescue:

    - name: Capture the error message
      register: my_error
      ansible.builtin.set_stats:
        data:
          my_error: "{{ ansible_failed_result.msg }}"

    - name: Capture the Job ID
      register: my_job_id
      ansible.builtin.set_stats:
        data:
          my_job_id: "{{ tower_job_id }}"

    - name: Capture the Job Template name
      register: my_job_template_name
      ansible.builtin.set_stats:
        data:
          my_job_template_name: "{{ tower_job_template_name }}"

    - name: Fail the job even though the rescue worked
      ansible.builtin.fail:
        msg: failing so we create the incident ticket
```
# A youtube video of the demo

- [AAP Daily Demo Windows](https://youtu.be/RNwel6BeCVI?si=ruIwcDFp6dyyAkjO "AAP Daily Demo Windows")
