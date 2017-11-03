---
title: "Playbooks and Roles"
teaching: 5
exercises: 0
objectives:
- "Exposure to playbooks"
keypoints:
- "Bringing together hosts and roles"
---

### What's a playbook?

Can be as simple as a sequence of tasks. 

Example - from your clone of ansible-examples-omero:

~~~
$ cd ~/forks/ansible-examples-omero/public-user
$ cat playbook.yml
~~~
{: .bash}
[playbook.yml](https://github.com/ome/ansible-examples-omero/blob/master/public-user/playbook.yml), uses roles like [openmicroscopy.postgresql](https://github.com/openmicroscopy/ansible-role-postgresql), [openmicroscopy.omero-server](https://github.com/openmicroscopy/ansible-role-omero-server), ...
~~~
# Install OMERO.server and OMERO.web with a public user on localhost

- hosts: all
  roles:

    - role: openmicroscopy.postgresql
      postgresql_databases:
      - name: omero
      postgresql_users:
      - user: omero
        password: omero
        databases: [omero]
      postgresql_version: "9.6"

    - role: openmicroscopy.omero-server

    - role: openmicroscopy.omero-web
      omero_web_config_set:
        omero.web.public.enabled: True
        omero.web.public.server_id: 1
        omero.web.public.user: public
        omero.web.public.password: "{{ omero_web_public_password }}"
        omero.web.public.url_filter: "^/(webadmin/myphoto/|webclient/(?!(action|logout|annotate_(file|tags|comment|rating|map)|script_ui|ome_tiff|figure_script))|webgateway/(?!(archived_files|download_as)))"


    # This role only works on OMERO 5.3+
    - role: omero-user
      omero_user_bin_omero: /opt/omero/server/OMERO.server/bin/omero
      omero_user_system: omero-server
      omero_user_admin_user: root
      omero_user_admin_pass: omero
      omero_group_create:
      - name: demo
        type: read-only
      omero_user_create:
      - login: public
        firstname: public
        lastname: user
        password: "{{ omero_web_public_password }}"
        groups: "--group-name demo"

  vars:
    omero_web_public_password: public
~~~
{: .output}

{% include links.md %}
