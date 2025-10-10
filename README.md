Ansible Automation Platform Daily Demo for Windows
=========
A demo designed to showcase many of the use cases that people are looking for.  We are using the workflow visualizer to show how the various building blocks are put together and enable the delivery on demand of a custom website.  The playbooks call roles, the roles allow for ease of sharing the code and also allow for documentation of the various things needed in each role. The demo is designed to be integrated with an IT Service Management (ITSM) system.  Everything will be documented in ITSM system via the skillfull use of automation.  Check out the video below to see that "the art of the possible."

Notes
=========
1. This demo is designed to work with the Red Hat Demo Platform. Please see the aap.as.code repo below. [aap.as.code](https://github.com/ericcames/aap.as.code "aap.as.code")
2. This demo works with Amazon currently.
3. This demo works with ServiceNow.

Day 0 - Configuration as code (CAC) a repeatable build process for this demo
=========
Configuration as code give you an easy way to recover/move your ansible related artifacts to a new platform.  That includes your hardcoded credentials.  The hardcoded credentials can be safely vaulted in an ansible vault file.  Check out the setup_demo.yml for the configurations for setting up this demo using configuration as code.

[Setup - Windows Daily Demo - CAC](https://github.com/ericcames/aap.dailydemo.windows/blob/main/playbooks/setup_demo.yml "Setup - Windows Daily Demo - CAC")<br>

Variables used in the setup template
```
my_organization: AmesCO
timezone_id: America/Phoenix
my_vault: Eric Ames
my_aap_credential: Controller Credential
my_aws_credential: AWS
my_snow_caller_name: service.ansible
my_username: service.ansible
aap_configuration_async_retries: 60
my_remote_vault: >-
  https://raw.githubusercontent.com/ericcames/sourcefiles/refs/heads/main/vault_ames.yml
my_remote_ssh_pub_key: >-
  https://raw.githubusercontent.com/ericcames/sourcefiles/refs/heads/main/id_rsa.pub
```

Day 1 - Run workflow for the Windows Daily Demo
=========

![alt text](https://github.com/ericcames/aap.dailydemo.windows/blob/main/images/windowswf.png "Start of workflow")

**The playbooks**

![alt text](https://github.com/ericcames/aap.dailydemo.windows/blob/main/images/ddwtemps.png "The job templates")

[Site Delete will clean everything up](https://github.com/ericcames/aap.dailydemo.windows/blob/main/playbooks/site_delete.yml "site_delete.yml")<br>

ServiceNow
========

**The playbooks**

[Create a CMDB record](https://github.com/ericcames/aap.dailydemo.windows/blob/main/playbooks/servicenow/create_ci.yml "create_ci.yml") <br>
[Create a CMDB relationship](https://github.com/ericcames/aap.dailydemo.windows/blob/main/playbooks/servicenow/create_cmdb_relationship.yml "create_cmdb_relationship.yml") <br>
[Create incident ticket](https://github.com/ericcames/aap.dailydemo.windows/blob/main/playbooks/servicenow/incident_create.yml "incident_create.yml") <br>
[Update requested item ticket](https://github.com/ericcames/aap.dailydemo.windows/blob/main/playbooks/servicenow/update_sn_req_itm.yml "update_sn_req_itm.yml") <br>

ServiceNow credential<br>
Input configuration
```
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
Injector configuration
```
env:
  SN_HOST: '{{instance}}'
  SN_PASSWORD: '{{password}}'
  SN_USERNAME: '{{username}}'
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
# The website

![alt text](https://github.com/ericcames/aap.dailydemo.windows/blob/main/images/windowsweb1.png "Webtop")
![alt text](https://github.com/ericcames/aap.dailydemo.windows/blob/main/images/windowsweb2.png "Webbottom")

# A youtube video of the demo

- [AAP Daily Demo Windows](https://youtu.be/RNwel6BeCVI?si=ruIwcDFp6dyyAkjO "AAP Daily Demo Windows")


# Important Note
The user_data line in the task listed below is designed to work with a template to set the password on the machine as it is built.  It works with a machine credential in the ansible automation platform.

![alt text](https://github.com/ericcames/aap.dailydemo.windows/blob/main/images/windowsmachinecred.png "Windows Machine Credential")

[Windows Machine Instance Creation](https://github.com/ericcames/aap.dailydemo.windows/blob/main/roles/instance_create_aws/tasks/main.yml "Windows Machine Instance Creation")<br>
```
- name: "Creating AWS VMs in {{ region }}"
      register: instance
      amazon.aws.ec2_instance:
        name: "Windows Daily Demo"
        state: running
        region: "{{ region }}"
        key_name: "{{ my_ssh_key }}"
        vpc_subnet_id: "{{ vpc_subnet_id }}"
        instance_type: "{{ instance_type }}"
        security_group: "{{ ec2_security_group_name }}"
        network:
          assign_public_ip: "{{ assign_public_ip }}"
        image_id: "{{ image }}"
        tags:
          Environment: windows-dailydemo
          AlwaysUp: "{{ alwaysup }}"
          Createdby: Ansible Controller
          Contact: "{{ my_email_address }}"
          DeletebBy: "{{ ec2_ansible_group }}"
          info: "This instance was built by the Sales Team"
        user_data: "{{ lookup('template', 'scripts/aws_userdata') }}"
        wait: true
        wait_timeout: 600
```
# Day 2 Operations
**Audit**<br>
Audit registry entries and repair if needed.  Document the work in a CSV file.<br>
[Audit](https://github.com/ericcames/aap.dailydemo.windows/blob/main/playbooks/auditme.yml "auditme.yml") <br>
![alt text](https://github.com/ericcames/aap.dailydemo.windows/blob/main/images/winaudit1.png "Fixed")
![alt text](https://github.com/ericcames/aap.dailydemo.windows/blob/main/images/winaudit2.png "Good")

**Patching**<br>
We are using a survey to select what windows patches we want to apply as well as whether or not to reboot the machine.<br>
[Patching](https://github.com/ericcames/aap.dailydemo.windows/blob/main/playbooks/windows_patching_07.yml "windows_patching_07.yml") <br>
![alt text](https://github.com/ericcames/aap.dailydemo.windows/blob/main/images/winpatch1.png "surveytop")
![alt text](https://github.com/ericcames/aap.dailydemo.windows/blob/main/images/winpatch2.png "surveybottom")

# Adhoc windows commands
```
win_ping
win_shell -> Get-Service
win_shell -> Get-Process
setup
win_shell -> Add-WindowsCapability -Online -Name OpenSSH.Server
win_shell -> Start-Service sshd
win_shell -> Set-Service -Name sshd -StartupType ‘Automatic’
win_service -> name=sshd
```
Looking for other Daily Demos?
=========

- [AAP Daily Demo Windows](https://github.com/ericcames/aap.dailydemo.windows "AAP Daily Demo Windows")
- [AAP Daily Demo Linux](https://github.com/ericcames/aap.dailydemo.linux "AAP Daily Demo Linux")
- [AAP Daily Demo F5](https://github.com/ericcames/aap.dailydemo.F5 "AAP Daily Demo F5")
- [AAP Daily Demo Panos](https://github.com/ericcames/aap.dailydemo.Panos "AAP Daily Demo Panos")
- [AAP Daily Demo Satellite](https://github.com/ericcames/aap.dailydemo.satellite "AAP Daily Demo Satellite")