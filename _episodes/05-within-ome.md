---
title: "Ansible within OME"
teaching: 10
exercises: 0
objectives:
- "Learn about Ansible use within OME"
keypoints:
- "Inventory"
- "prod-playbooks"
- "testing: travis (in Docker), molecule"
---


### OME Ansible Roles

The [public user example playbook](https://github.com/openmicroscopy/prod-playbooks/) which we just ran is really just a definition of what roles we wanted to be run against our vagrant machine. 

OME have an ever growing set of roles which were used to build the IDR and more recently production systems like the OMERO demo server and Nightshade, the College of Life Science's OMERO server.

Check them out at: [github](https://github.com/search?utf8=%E2%9C%93&q=org%3Aopenmicroscopy+ansible-role&type=)

<img src="../fig/gh-ansible-roles.png" title="Lots of roles" alt="Lots of roles" style="display: block; margin: auto; width:600px; align=right" />

#### management_tools

* our systems inventory
* ssh-keyscan to overcome the initial lack of ssh host keys

#### Testing

* Some roles can be tested with travis with molecule - separate training?


{% include links.md %}
