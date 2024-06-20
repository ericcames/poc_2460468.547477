update_incident
=========
```
Update incident ticket in ServiceNow
```
Requirements
------------

Role Variables
--------------
```
incident_work_notes: This came from AAP!! Hoo-yah!
incident_state: 2
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

    - role: update_incident
```
License
-------

https://opensource.org/license/mit
