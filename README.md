Ansible MariaDB Galera cluster
=========

Install and configure a MariaDB galera cluster with Ansible.

Requirements
------------

Ubuntu-based hosts.

Role Variables
--------------

* galera_ip_[*]. IPs of the Galera nodes
* galera_name: Name of the Galera cluster

Dependencies
------------

None

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

```
- name: Install galera cluster
  hosts: "host_list"
  gather_facts: true
  become: true
  become_user: root
  vars:
    ansible_user: ubuntu
    ansible_python_interpreter: "/usr/bin/env python3"
  tasks:

    - name: Upgrade all packages to the latest version
      apt:
        name: "*"
        state: latest
        force_apt_get: True

    - name: Save private IPs as facts
      set_fact:
        galera_instances: "{{ hostvars['localhost']['galera_instances'] }}"

    - name: Save independent variables
      set_fact:
        galera_ip_1: "{{ galera_instances.instances.0.private_ip_address }}"
        galera_ip_2: "{{ galera_instances.instances.1.private_ip_address }}"
        galera_ip_3: "{{ galera_instances.instances.2.private_ip_address }}"
        galera_name: "{{ ansible_default_ipv4.address }}"

    - name: Invoke Galera MariaDB role
      include_role:
        name: ansible-galera-mariadb
```

License
-------

GPLv3

Author Information
------------------

[Agustín García Flores](https://www.linkedin.com/in/agustingarciaflores/)
