The Proof of Concept
=========
Managing network device upgrades using ansible automation platform and a IT Service managmenet (ITSM) system.  Managing network health test automation in the same fashion as the first use case.  In the first use case we arbitrarily opted to use the Incident Managment process and on the second use case we opted to used the Standard Change process.  The third use case was replacing an existing piece of network gear.  The systems we were working with were Infoblox, Cisco DNA Center, Cisco Identity Service Engine, TACACS, ServiceNOW and of course the Ansible Automation Platform.

# Device Upgrades

**Incident Ticket created from a template**

![alt text](https://github.com/ericcames/poc_2460468.547477/blob/main/images/inc1.png "")
![alt text](https://github.com/ericcames/poc_2460468.547477/blob/main/images/inc2.png "")
![alt text](https://github.com/ericcames/poc_2460468.547477/blob/main/images/inc3.png "")

**Incident Ticket fires off an Ansible worflow template**

![alt text](https://github.com/ericcames/poc_2460468.547477/blob/main/images/incaapjob1.png "")

**Here is the Incident Template**

![alt text](https://github.com/ericcames/poc_2460468.547477/blob/main/images/DeviceUpgradeTemplate.png "")

**Device upgrade workflow in ServiceNOW**

![alt text](https://github.com/ericcames/poc_2460468.547477/blob/main/images/DeviceUpgradeFlow1.png "")
![alt text](https://github.com/ericcames/poc_2460468.547477/blob/main/images/DeviceUpgradeFlow2.png "")
![alt text](https://github.com/ericcames/poc_2460468.547477/blob/main/images/DeviceUpgradeFlow3.png "")

**The Playbook that does the work**

[IOS Change Firmware](https://github.com/ericcames/poc_2460468.547477/blob/main/playbooks/cisco/ios_change_firmware.yml "IOS Change Firmware") <br>

# Network Health Test Automation

**Change Ticket created from a template**

![alt text](https://github.com/ericcames/poc_2460468.547477/blob/main/images/crq1.png "")
![alt text](https://github.com/ericcames/poc_2460468.547477/blob/main/images/crq2.png "")

**Change Ticket fires off an Ansible worflow template**

![alt text](https://github.com/ericcames/poc_2460468.547477/blob/main/images/crqaapjob1.png "")
![alt text](https://github.com/ericcames/poc_2460468.547477/blob/main/images/crqaapjob2.png "")


# Configuration Management Database (CMDB) updates

**The cmdb records**

![alt text](https://github.com/ericcames/poc_2460468.547477/blob/main/images/CMDBRecords.png "")

**The Playbook that does the work**

[CMDB Updates](https://github.com/ericcames/poc_2460468.547477/blob/main/playbooks/servicenow/create_ci.yml "CMDB Updates") <br>

# Automated incident management example

![alt text](https://github.com/ericcames/poc_2460468.547477/blob/main/images/automatedinc.png "")

[Example Error Handling in support of incident enrichment](https://github.com/ericcames/poc_2460468.547477/blob/main/playbooks/infoblox/create_host_record.yml "Example Error Handling") <br>

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


# Environment

**Infoblox**

![alt text](https://github.com/ericcames/poc_2460468.547477/blob/main/images/InfoBlox1.png "")
![alt text](https://github.com/ericcames/poc_2460468.547477/blob/main/images/infoblox2.png "")

**Cisco ISE**

![alt text](https://github.com/ericcames/poc_2460468.547477/blob/main/images/ise1.png "")

**Ansible Users**

![alt text](https://github.com/ericcames/poc_2460468.547477/blob/main/images/aapusers.png "")

**Ansible Service Account in SNOW**

![alt text](https://github.com/ericcames/poc_2460468.547477/blob/main/images/AnsibleSVC.png "")
![alt text](https://github.com/ericcames/poc_2460468.547477/blob/main/images/AnsibleSVCroles.png "")
![alt text](https://github.com/ericcames/poc_2460468.547477/blob/main/images/AnsibleSVCgroups.png "")


**Open issue**
- [Open Issue - swim_workflow_manager ansible module](https://github.com/cisco-en-programmability/dnacenter-ansible/issues/174 "Open Issue - swim_workflow_manager ansible module")

- [Cisco Engineer found the same issue](https://beye.blog/cisco-dna-center-ansible-issue-dna-center-python-sdk-not-installed/ "")

**Custom Execution Environment**

```
---
version: 3
images:
  base_image:
    name: registry.redhat.io/ansible-automation-platform-24/ee-supported-rhel8:latest
dependencies:
  galaxy:
    collections:
      - community.network
      - cisco.dnac
      - cisco.ios
      - cisco.ise
      - infoblox.nios_modules
  system:
    - pkgconf-pkg-config [platform:rpm]
    - systemd-devel [platform:rpm]
    - gcc [platform:rpm]
    - python39-devel [platform:rpm]
  python:
    - packaging
    - requests[security]
    - xmltodict
    - msgraph-sdk==1.0.0
    - psycopg2-binary
    - infoblox-client
    - ciscoisesdk
    - dnacentersdk
    - urllib3==1.26.15
options:
  package_manager_path: /usr/bin/microdnf
```

- [quay.io/locust61/cisco-infoblox-ee](https://quay.io/locust61/cisco-infoblox-ee "Custom Exeuction Environment")
- [locust61 quay repo](https://quay.io/locust61 "locust61 quay repo")

# Ansible Custom Credentials

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

# ServiceNow Ansible spoke setup

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
