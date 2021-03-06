# rsync-server

[![CI](https://github.com/infOpen/ansible-role-rsync-server/workflows/CI/badge.svg)](https://github.com/infOpen/ansible-role-rsync-server/actions)
[![Mergify Status][mergify-status]][mergify]
[![Updates](https://pyup.io/repos/github/infOpen/ansible-role-rsync-server/shield.svg)](https://pyup.io/repos/github/infOpen/ansible-role-rsync-server/)
[![Python 3](https://pyup.io/repos/github/infOpen/ansible-role-rsync-server/python-3-shield.svg)](https://pyup.io/repos/github/infOpen/ansible-role-rsync-server/)
[![Ansible Role](https://img.shields.io/ansible/role/21209.svg)](https://galaxy.ansible.com/infOpen/rsync-server/)

Install rsync-server package.

## Requirements

This role requires Ansible 2.8 or higher,
and platform requirements are listed in the metadata file.

## Testing

This role use [Molecule](https://github.com/ansible-community/molecule) to run tests.

Local and Github Actions tests run tests on Docker by default.
See molecule documentation to use other backend.

Currently, tests are done on:
- CentOS 7
- CentOS 8
- Debian Buster
- Debian Stretch
- Ubuntu Bionic
- Ubuntu Focal

and use:
- Ansible 2.8.x
- Ansible 2.9.x

### Running tests

#### Using Docker driver

```
$ tox
```

You can also configure molecule options and molecule command using environment variables:
* `MOLECULE_OPTIONS` Default: "--debug"
* `MOLECULE_COMMAND` Default: "test"

```
$ MOLECULE_OPTIONS='' MOLECULE_COMMAND=converge tox
```

## Role Variables

### Default role variables

``` yaml
# Packages and repositories
rsync_server_repository_cache_valid_time: 3600
rsync_server_packages: "{{ _rsync_server_packages }}"

# User and group management
rsync_server_group:
  name: 'rsync'
rsync_server_user:
  name: 'rsync'
  group: "{{ rsync_server_group.name }}"
  home: '/dev/null'
  shell: '/bin/false'

# Main configuration
rsync_server_config_main:
  global: |
    uid = rsync
    gid = rsync
  modules: []

# Default daemon configuration
rsync_server_config_default: |
  RSYNC_ENABLE=false
  RSYNC_OPTS=''
  RSYNC_NICE=''

# Service management
rsync_server_service: "{{ _rsync_server_service }}"
```

### Debian OS family role variables

``` yaml
# Packages and repositories
_rsync_server_packages:
  - name: 'rsync'

# Paths
_rsync_server_paths:
  files:
    default:
      path: '/etc/default/rsync'
    main_config:
      path: '/etc/rsyncd.conf'

# Service management
_rsync_server_service:
  name: 'rsync'
```

## How ...

### Manage rsync configuration

Follow this example:
``` yaml
rsync_server_config:
  global: |
    uid: 'rsync'
    gid: 'rsync'
  modules:
    - name: 'my_sync'
      config: |
        path = /data
        comment = Example sync
        read only = false
```

### Manage users used to authorization

``` yaml
rsync_users:
  - name: 'foo'
    password: 'bar'
```


## Dependencies

None

## Example Playbook

``` yaml
- hosts: servers
  roles:
    - { role: infOpen.rsync-server }
```

## License

MIT

## Author Information

Alexandre Chaussier (for Infopen company)
- https://www.infopen.pro
- a.chaussier [at] infopen.pro

[mergify]: https://mergify.io
[mergify-status]: https://img.shields.io/endpoint.svg?url=https://gh.mergify.io/badges/infOpen/ansible-role-rsync-server&style=flat
