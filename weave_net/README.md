Ansible role: Weave Net
=========

This role is child role of net_addon and used to install and configure weave-net.

For now, it does the following:
- open ports
- create project working directory for role on localhost
- create secret if needed and install weave-net


Requirements
------------

This is not strict requirements and it may not work with other versions than tested ones.
Anyway. feel yourself free to test by yourself, suggest addition of new functionality and contribute.

Role is tested with:
- Ansible version >= 2.8.6
- CentOS version >= 7.6 (1803)

Currently supports only 'with_k8s' deployment type, it means that weave-net will be installed into kubernetes cluster, which must be present as well as credentials to it.


Role Variables
--------------

Variables and their descriptions copied from defaults/main.yml

```bash

# Deployment type of weave-net:
weave_net_deployment_type: with_k8s

# Variable which is common for most projects, used in 
# configuration files or file/directory names.
# By default used as reference for weave_net_project_dir variable:
weave_net_project_name: test

# Variable which is common for most projects, used as
# project working directory on the localhost for the role.
# Currently is used for creating weave-net workdir, creating secret files,
# downloading kubernetes manifests, copying or creating symlink to kube config etc.:
weave_net_project_dir: files/{{ weave_net_project_name }}

# If encryption via secret must enabled between peers:
weave_net_encryption: no

```

Variables and their descriptions copied from vars/with_k8s.yml

```bash

# Kubernetes version to use in weave-net installation:
weave_net_k8s_version: 1.14.10

# Environment variables of weave-net:
weave_net_env: ""         # "&env.KEY1=value1&env.KEY2=value2"

```


Dependencies
------------

Access to Kubernetes cluster if deployment type is 'with_k8s'


Example Playbook
----------------

```bash
---
- hosts: localhost
  gather_facts: false
  become: no
  tasks:
  - name: Check ansible version >=2.8.6
    assert:
      msg: Ansible must be v2.8.6 or higher
      that:
      - ansible_version.string is version("2.8.6", ">=")
    tags:
    - check
  vars:
    ansible_connection: local

- hosts: all
  become: yes
  tasks:
  - import_role:
      name: net_addon/weave_net

```

More detailed examples ( inventories, playbooks etc. ) of this and other roles can be found [here](https://github.com/caermeglaeddyv/examples/tree/dev/ansible).

It's highly recommended to start you test deploys from there, especially if you use Google Cloud Platform or VMware vCenter as your infrastructure, for now that [repository](https://github.com/caermeglaeddyv/examples) contains [Packer](https://github.com/caermeglaeddyv/examples/tree/dev/packer) and [Terraform](https://github.com/caermeglaeddyv/examples/tree/dev/terraform) examples to build templates and deploy machines on this platforms.


License
-------

[Apache 2.0](https://github.com/caermeglaeddyv/ansible-role-rear/blob/dev/LICENSE)


Author Information
------------------

Copyright 2020 [caermeglaeddyv](https://github.com/caermeglaeddyv)
