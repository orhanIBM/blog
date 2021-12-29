---
title: Ansible Playbooks
date: "2021-12-29T12:26:31Z"
description: "Oh YAML! Ansible playbooks, what are they and how they work?"
---

Ansible Playbook is a YAML file that contains set of activities to run on host machine(s).

An analogy to a workshop could be, **hosts** are your raw materials, **modules** are the tools in your workshop and **playbooks** are the instruction manual of how to use each *module* to craft the *host*.

A playbook can be as simple as run command on the server and done with it or as complex as provisioning the full web/app servers and db, seting up network connectivity between, adding reverse proxy and load balancers, monitoring connections and updating all with required patches.


Given an inventory file
```inventory.ini
# inventory.ini, after ansible_hosts, variables are just for reference
[dev]
dev1 ansible_host=123.x.x.x ansible_user=root
dev2 ansible_host=123.x.x.x ansible_connection=ssh ansible_port=22

[test]
...
[staging]
...
[production]
...

```

The playbook will execute the plays. The example below will install httpd to dev servers stated in the inventory.ini file.


```playbook.yml
# playbook.yml

---
# Play 1
- name: Update dev
  hosts: dev
  remote_user: root 

  tasks:

  - name: ensure apache is at the latest version
    # below is an ansible modules
    ansible.builtin.yum:
        name: httpd
        state: latest
  
  - name: Write the apache config file
    # below is an ansible module
    ansible.builtin.template:
        src: /srv/httpd.js
        dest: /etc/httpd.conf

# Play 2
- name: Update database servers
  ...
    

```