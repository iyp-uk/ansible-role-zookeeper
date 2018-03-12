Role Name
=========

This role provides zookeeper as a cluster.
It can also install it as standalone.

Requirements
------------

None.

Role Variables
--------------

See [defaults/main.yml](defaults/main.yml).

Dependencies
------------


There's a dependency on Java being installed, but it's not enforced by the role.

> Refer to [geerlingguy/ansible-role-java](https://github.com/geerlingguy/ansible-role-java) for an Ansible Java role.

Example Playbook
----------------


The example below shows how to include the role in your playbook, overriding many of the variables exposed.

Notice `zookeeper_config` is a dictionary, and we only override a few keys.
For this to work you'll need to have the following in your `ansible.cfg`: 

```ini
[defaults]
hash_behaviour=merge
```

> If you prefer not to do it, you can still copy the whole `zookeeper_config` dict and override just where you want.
You'd then have to keep track of changes when this role updates.

```yaml

- name: Installing ZooKeeper
  hosts: zookeeper
  become: yes
  vars:
    zookeeper_log4j_default_conversion_pattern: "[%d] [myid:%X{myid}] %p %m (%c)%n"
  roles:
    - role: zookeeper
      zookeeper_hosts: "{{groups['zookeeper']}}"
      zookeeper_version: 3.4.10
      zookeeper_log4j_prop: "TRACE,CONSOLE,ROLLINGFILE" # Make sure not to include any space in there
      zookeeper_config:
        log4j:
          zookeeper_log_threshold: INFO
          log4j_appender_CONSOLE:
            layout:
              conversion_pattern: "{{ zookeeper_log4j_default_conversion_pattern }}"
          log4j_appender_ROLLINGFILE:
            layout:
              conversion_pattern: "{{ zookeeper_log4j_default_conversion_pattern }}"
```

License
-------

MIT

Author Information
------------------

[@miaoulafrite](https://github.com/miaoulafrite)
