DOCKER
======

Ansible role to install and configure Docker environment based on [wunzeco.docker](https://github.com/wunzeco/ansible-docker) role. (BSD license according to meta/main.)

## Testing

This role can be tested (in part) using molecule.

Please note there are two tests, one Docker and one Vagrant. Both use:

```
$ molecule test
```

`molecule test` should be used, as it starts from a fresh setup.

TODO: Roll these into one file, with checks denoted on the commandline. 

### Docker

The reason this is a 'part' test, is because it doesn't let docker try to start, docker in docker brings its own problems.

It relies on a specific environment variable, listed in `molecule.yml`

### Vagrant

There is also a Vagrant/Libvirt test. This relies on Libvirt being installed on your local machine, along with Vagrant and associated files.

To run it something like the following can be invoked:

```
$ cp molecule.yml-vagrant molecule.yml
$ molecule destroy
$ molecule test
```

Once completed, and you're happy docker installs and runs, move the `-docker` version back to `molecule.yml` for pushing.

## Example

- To configure install and configure docker engine with remote api enabled:

```
  vars:
    docker_port: 2375
    docker_opts:
      - "-H tcp://0.0.0.0:{{ docker_port }}"
      - "-H unix:///var/run/docker.sock"

  roles:
    - devops.docker
```
