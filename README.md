deploy_gitea_runner
=========

This role installs and configure a gitea act runner on **debian-based** distributions.

Requirements
------------

If `deploy_gitea_runner_deploy_method` is set to `docker`, this role assumes you have `docker`, `docker-compose` installed on the target hosts. The role will not install these components, but you can install them using the [install_docker](https://github.com/ednz_cloud/install_docker) role.

Role Variables
--------------
Available variables are listed below, along with default values. A sample file for the default values is available in `default/deploy_gitea_runner.yml.sample` in case you need it for any `group_vars` or `host_vars` configuration.

```yaml
deploy_gitea_runner_version: latest # by default, set to latest
```
This variable defines the version that will be deployed to your host. In case you use `deploy_gitea_runner_deploy_method: host`, this has to match a release version on [gitea act runner repository](https://gitea.com/gitea/act_runner/releases). If using `deploy_gitea_runner_deploy_method: docker`, this has to match a tag on the [gitea act runner docker registry](https://hub.docker.com/r/gitea/act_runner/tags)

```yaml
deploy_gitea_runner_deploy_method: host # by default, set to host
```
This variable defines whether the gitea-runner should be deployed as a binary on the host, or as a docker container. This defaults to `host` but can also be `docker`.

```yaml
deploy_gitea_runner_directory: /opt/gitea-actions # by default, set to /opt/gitea-actions
```
This variable defines where to store the files for the gitea-runner (config, potential docker-compose, etc...)

```yaml
deploy_gitea_runner_timezone: "Europe/Paris" # by default, set to Europe/Paris
```
This variable is only used for if `deploy_gitea_runner_deploy_method` is `docker`, to set the timezone inside the container.

```yaml
deploy_gitea_runner_register: false # by default, set to false
```
This variable sets whether or not the role will register the runner against your gitea instance. It will only register if it cannot find the `.runner` file that is generated when registering, and if `deploy_gitea_runner_server_token` is not empty. If `deploy_gitea_runner_deploy_method` is `docker`, this has no impact, since the registration will be handle automatically when to container starts up, given that you have provided a valid URL and token (either via the role's variable, or manually after deploying).

```yaml
deploy_gitea_runner_start_service: false # by default, set to false
```
This variable sets whether to start the service immediately or not. In case you manually register the runner after deployment, this should be set to `false`.

```yaml
deploy_gitea_runner_server_url: https://git.example.com # by default, set to https://git.example.com
```
This is the url of your gitea instance, and should be resolvable by the runner.

```yaml
deploy_gitea_runner_server_token: "" # by default, set to an empty string
```
This is your gitea token. if it isn't set, you cannot run auto-registration. THIS IS A SENSITIVE VALUE, AND SHOULD NOT APPEAR IN CLEAR TEXT IN YOUR REPOSITORY.

```yaml
deploy_gitea_runner_name: gitea-runner # by default, set to gitea-runner
```
This is the name under which the runner will register itself against your gitea server.

```yaml
deploy_gitea_runner_config: # by default, set to the following
  log:
    level: info
  runner:
    file: "{{ deploy_gitea_runner_directory }}/.runner" # this HAS TO BE .runner if deploy_gitea_runner_deploy_method is docker
    capacity: 1
    timeout: 3h
    insecure: false
    fetch_timeout: 5s
    fetch_interval: 2s
    labels: []
  cache:
    enabled: true
    dir: "{{ deploy_gitea_runner_directory }}/cache" # this HAS TO BE /cache if deploy_gitea_runner_deploy_method is docker
    host:
    port: 0
    external_server:
  container:
    network: ""
    privileged: false
    options:
    workdir_parent:
    valid_volumes: []
    docker_host: ""
  host:
    workdir_parent:
```
This is the config file for gitea, put into a variable. The default values are from the default config.yaml generated when running `act_runner generate-config`. Some of the values, like `cache.dir` and `runner.file` have to be set to specific values in case you're running this role with `deploy_gitea_runner_deploy_method` set to `docker`. The rest is configurable according to the standard documentation.

Dependencies
------------

None.

Example Playbook
----------------

```yaml
# calling the role inside a playbook with either the default or group_vars/host_vars
- hosts: servers
  roles:
    - ednz_cloud.deploy_gitea_runner
```

License
-------

MIT / BSD

Author Information
------------------

This role was created by Bertrand Lanson in 2023.