---
title: Ansible Inventory
date: "2021-12-29T02:03:32Z"
description: "Ansible Inventory Basics"
---

Ansible can work with multiple servers.

But Ansible needs to access the target machines.

For linux these machines are through ssh and for windows through Powershell Remoting

So no need to install any application to the target machines.

Those target machines are in an inventory file.

By default ansible looks at /etc/ansible/hosts file if you do not set up one.

Ansible inventory file can contain more parameters

* ansible_host
* ansible_connection - ssh/winrm/localhost
* ansible_port - 22/5986
* ansible_user - root/admin/...
* ansible_ssh_pass - password, but use keyless in production 

an example could be 
```inventory.ini
//inventory.ini file
target1 ansible_host=192.168.1.10 ansible_connection=ssh ansible_port=22 ansible_user=root ansible_ssh_pass=password
```

then run the following ansible command to ping target1 server

```$ ansible target1 -m ping -i inventory.ini```

here we are running ping module on target servers located in inventory file.

If you have more than one target, you can group them


```inventory.ini
server1.example.com
server2.example.com

[web]
web1.example.com
web2.example.com

[db]
db.example.com
```

or YAML

```inventory.yaml
#inventory yaml file:
all:
    hosts:
        example.com
    childern:
        webservers:
            hosts:
                192.168.0.1
                192.168.0.5
        
        dbservers:
            hosts:
                192.168.0.10
                

```

so to *execute ping module on webservers*, all you have to do is:

 ``` $ ansible webservers -m ping -i inventory.yaml```