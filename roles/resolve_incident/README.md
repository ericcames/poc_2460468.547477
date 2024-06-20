resolve_incident
=========
```
Resolve incident ticket in ServiceNow
```
Requirements
------------

Role Variables
--------------
```
incident_close_code: Solution provided
incident_close_notes: IOS Upgrade Process
incident_state: 6
```
Dependencies
------------

Example Playbook
----------------
```
---
- name: Update requested item ticket in servicenow
  hosts: localhost

  roles:

    - role: resolve_incident
```
License
-------

https://opensource.org/license/mit
