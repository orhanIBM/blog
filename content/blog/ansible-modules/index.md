---
title: Ansible Modules
date: "2021-12-29T02:26:32Z"
description: "Ansible Modules"
---

Modules are task libraries for Ansibles.

They can be used from the cli or in a playbook.

Ansible will execute the module on the remote host, collects and return values.

For example to restart all webservers listed in inventory file

```ansible webservers -m command - a "/sbin/reboot -t now"```

To find more about modules, just search for *ansible docs modules latest user guide*