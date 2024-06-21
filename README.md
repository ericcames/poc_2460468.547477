- [Custom Exeuction Environment](https://quay.io/locust61/cisco-infoblox-ee "Custom Exeuction Environment")

- Cisco Identity Services Engine (ISE) credential
```
Input configuration

fields:
  - id: ISE_HOSTNAME
    type: string
    label: Hostname
  - id: ISE_USERNAME
    type: string
    label: Username
  - id: ISE_PASSWORD
    type: string
    label: Password
    secret: true
  - id: ISE_VERIFY
    type: string
    label: SSL Verify
required:
  - Hostname
  - Username
  - Password
  - SSL Verify
```
```
Injector configuration

env:
  ISE_HOSTNAME: '{{ ISE_HOSTNAME }}'
  ISE_PASSWORD: '{{ ISE_PASSWORD }}'
  ISE_USERNAME: '{{ ISE_USERNAME }}'
  ISE_VERIFY: '{{ ISE_VERIFY }}'
```

- Cisco DNA Center (DNAC) credential
```
Input configuration

fields:
  - id: DNAC_HOST
    type: string
    label: Hostname
  - id: DNAC_USERNAME
    type: string
    label: Username
  - id: DNAC_PASSWORD
    type: string
    label: Password
    secret: true
  - id: DNAC_VERIFY
    type: string
    label: SSL VERIFY

```
```
Injector configuration

env:
  DNAC_HOST: '{{ DNAC_HOST }}'
  DNAC_VERIFY: '{{ DNAC_VERIFY }}'
  DNAC_PASSWORD: '{{ DNAC_PASSWORD }}'
  DNAC_USERNAME: '{{ DNAC_USERNAME }}'
```

- Infoblox credential
```
Input configuration

fields:
  - id: INFOBLOX_HOST
    type: string
    label: Hostname
  - id: INFOBLOX_USERNAME
    type: string
    label: Username
  - id: INFOBLOX_PASSWORD
    type: string
    label: Password
    secret: true
required:
  - Username
  - Password
  - Hostname
```
```
Injector configuration

env:
  INFOBLOX_HOST: '{{ INFOBLOX_HOST }}'
  INFOBLOX_PASSWORD: '{{ INFOBLOX_PASSWORD }}'
  INFOBLOX_USERNAME: '{{ INFOBLOX_USERNAME }}'
```

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

[Example Error Handling in support of incident enrichment](https://github.com/ericcames/aap.dailydemo.linux/blob/main/roles/instance_create_aws/tasks/main.yml "Example Error Handling") <br>

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
