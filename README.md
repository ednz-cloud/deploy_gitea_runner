deploy_gitea_runner
=========

This role installs and configure a gitea act runner on **debian-based** distributions.

Requirements
------------

None.

Role Variables
--------------
Available variables are listed below, along with default values. A sample file for the default values is available in `default/deploy_gitea_runner.yml.sample` in case you need it for any `group_vars` or `host_vars` configuration.

```yaml
your_defaults_here: default_value # by default, set to default_value
```
A quick description of the variable, what it does, and how to use it.

Dependencies
------------



Example Playbook
----------------

```yaml
# calling the role inside a playbook with either the default or group_vars/host_vars
- hosts: servers
  roles:
    - ednxzu.deploy_gitea_runner
```

License
-------

MIT / BSD

Author Information
------------------

This role was created by Bertrand Lanson in 2023.