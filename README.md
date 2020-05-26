ansible-zookeeper
=================

[![Build Status](https://travis-ci.org/AnsibleShipyard/ansible-zookeeper.svg?branch=master)](https://travis-ci.org/AnsibleShipyard/ansible-zookeeper)

ZooKeeper playbook for Ansible

Installation
-----------

```
git clone https://github.com/law-lee/ansible-zookeeper.git
```

Requirements
------------

Ansible version at least 1.6

Role Variables
--------------
```

# use the latest minor version of major 3.4
zookeeper_version: 3.4.14
zookeeper_url: https://mirrors.tuna.tsinghua.edu.cn/apache/zookeeper/zookeeper-{{zookeeper_version}}/zookeeper-{{zookeeper_version}}.tar.gz

# Flag that selects if systemd or upstart will be used for the init service:
# Note: by default Ubuntu 15.04 and later use systemd (but support switch to upstart)
zookeeper_debian_systemd_enabled: "{{ ansible_distribution_version|version_compare(15.04, '>=') }}"
zookeeper_debian_apt_install: false
# (Optional:) add custom 'ppa' repositories depending on the distro version (only with debian_apt_install=true)
# Example: to use a community zookeeper v3.4.8 deb pkg for Ubuntu 14.04 (where latest official is v3.4.5)
zookeeper_debian_apt_repositories:
  - repository_url: "ppa:ufscar/zookeeper"
    distro_version: "14.04"

apt_cache_timeout: 3600
zookeeper_register_path_env: false

client_port: 2181
init_limit: 5
sync_limit: 2
tick_time: 2000
zookeeper_autopurge_purgeInterval: 0
zookeeper_autopurge_snapRetainCount: 10
zookeeper_cluster_ports: "2888:3888"
zookeeper_max_client_connections: 60

data_dir: /var/lib/zookeeper
log_dir: /var/log/zookeeper
zookeeper_dir: /opt/zookeeper-{{zookeeper_version}} # or /usr/share/zookeeper when zookeeper_debian_apt_install is true
zookeeper_conf_dir: {{zookeeper_dir}} # or /etc/zookeeper when zookeeper_debian_apt_install is true
zookeeper_tarball_dir: /opt/src

zookeeper_hosts_hostname: "{{inventory_hostname}}"

zookeeper_hosts: "
    {%- set ips = [] %}
    {%- for host in groups['zookeepers'] %}
    {{- ips.append(dict(id=loop.index, host=host, ip=hostvars[host]['ansible_default_ipv4'].address)) }}
    {%- endfor %}
    {{- ips -}}"
# Dict of ENV settings to be written into the (optional) conf/zookeeper-env.sh
zookeeper_env: {}

# Controls Zookeeper myid generation
zookeeper_force_myid: yes
```

Usage
-----
```
ansible-playbook -i inventory.ini zookeeper.yaml

```

License
-------

The MIT License (MIT)

Copyright (c) 2014 Kien Pham

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.


AnsibleShipyard
-------

Our related playbooks

1. [ansible-mesos](https://github.com/AnsibleShipyard/ansible-mesos)
1. [ansible-marathon](https://github.com/AnsibleShipyard/ansible-marathon)
1. [ansible-chronos](https://github.com/AnsibleShipyard/ansible-chronos)
1. [ansible-zookeeper](https://github.com/AnsibleShipyard/ansible-zookeeper)

Author Information
------------------

@AnsibleShipyard/developers and others.
