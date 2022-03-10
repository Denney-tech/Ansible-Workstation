### Ansible-Workstation
This is an Ansible collection designed with the intent to use during and after a linux kickstart image deployment. My goal here is to build a personal Linux Workstation for daily use, for work, and development, from scratch and all the way up to joining domain. Every effort will be made to make this code environment agnostic.

## How it works
Kickstart files can use any interpreter to run arbitrary code. This code can be run in the %pre and %post sections of a kickstart file.

E.g.
``` yaml
%pre --interpreter=/usr/bin/local/ansible-platform
---
- hosts: localhost
  gather_facts: no
  tasks:
    - name: Test
      debug:
        msg: "Hello World!"
%end
%post --interpreter=/usr/bin/local/ansible-platform --log=/root/ansible-test-ks.log
---
- hosts: localhost
  gather_facts: no
  tasks:
    - name: Test
      debug:
        msg: "Hello World!"
%end
```
Kickstart can also %include other kickstart files created by %pre sections. This is a great place for Ansible to template custom dynamic kickstart files.


# Requirements
In order for Ansible to work like this, it must be installed.  While you could take the time to build your own squashfs with it installed, I like to live dangerously on the edge and always have the most recent build installed.  So the first %pre section needs to make use of pip to install Ansible, and then subsequent %pre sections will work with Ansible.

The %pre and %post sections run in different environments however, and thus Ansible must similarly be made available again in the %post section.
