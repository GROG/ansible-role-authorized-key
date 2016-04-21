# Authorized-key

[![Ansible Galaxy][galaxy_image]][galaxy_link]
[![Build Status][travis_image]][travis_link]
[![Latest tag][tag_image]][tag_url]
[![Gitter chat][gitter_image]][gitter_url]

A role for managing authorized keys.

Following roles where designed to neatly work together with this role:

- [user][grog.user], for managing users.
- [sudo][grog.sudo], for managing sudo rights.

The [management-user][grog.management-user] role combines all these roles in
one easy to use role.

## Requirements

- Hosts should be bootstrapped for ansible usage (have python,...)
- Root privileges, eg `become: yes`

## Role Variables

| Variable | Description | Default value |
|----------|-------------|---------------|
| `authorized_key_list` | List of users and their keys **(see details!)** | `[]` |
| `authorized_key_list_host`| List of users and their keys **(see details!)**  | `[]` |
| `authorized_key_list_group` | List of users and their keys **(see details!)** | `[]` |
| `authorized_key_exclusive` | Default value for `exclusive` | `no` |
| `authorized_key_key_options` | Default value for `key_options` | / |
| `authorized_key_manage_dir` | Default value for `manage_dir` | `yes` |
| `authorized_key_state` | Default value for `state` | `present` |

#### `authorized_key_list` details

`authorized_key_list`, `authorized_key_list_host` and `authorized_key_list_group` are merged when
managing the authorized keys. You can use the host and group lists to specify
keys per host or group off hosts.

The authorized-key list allows you to define which users and there keys must be
managed. Each item in the list consists of a username and a list of keys.

| Variable | Description | Default |
|----------|-------------|---------|
| `name` | User name | / |
| `authorized_keys` | List of keys | / |

Each key in the list of authorized_keys can have following attributes:

| Variable | Description | Required | Default |
|----------|-------------|----------|---------|
| `exclusive` | Should this key be exclusive? | no | `authorized_key_exclusive` |
| `key` | SSH key | yes | / |
| `key_options` | SSH key options to prepend to key | no | / |
| `manage_dir` | Manage the authorized_keys directory? | no | `authorized_key_manage_dir` |
| `path` | Path for the SSH key | no | 'home_dir/.ssh/authorized_keys' |
| `state` | State of the key (present/absent) | no | `authorized_key_state` |

###### Example `authorized_key_list`

```yaml
authorized_key_list:
  - name: testuser1
    authorized_keys:
      - key: "{{ lookup('file', '/home/charlie/.ssh/id_rsa.pub') }}"
      - key: "{{ lookup('file', '/home/john/.ssh/id_rsa.pub') }}"
        state: absent
  - name: testuser2
    authorized_keys:
      - key: "{{ lookup('file', '/home/charlie/.ssh/id_rsa.pub') }}"
```

## Dependencies

None.

## Example Playbook

```yaml
---
- hosts: servers
  roles:
  - { role: GROG.authorized-key, become: yes }
```

Inside `group_vars/servers.yml`:

```yaml
authorized_key_list_group:
  - name: user
    authorized_keys:
      - key: "{{ lookup('file', '/home/charlie/.ssh/id_rsa.pub') }}"
      - key: "{{ lookup('file', '/home/john/.ssh/id_rsa.pub') }}"
```

## Contributing
All assistance, changes or ideas [welcome][issues]!

## Author
By [G. Roggemans][groggemans]

## License
MIT

[galaxy_image]:         https://img.shields.io/badge/galaxy-GROG.authorized--key-660198.svg?style=flat
[galaxy_link]:          https://galaxy.ansible.com/GROG/authorized-key
[travis_image]:         https://travis-ci.org/GROG/ansible-role-authorized-key.svg?branch=master
[travis_link]:          https://travis-ci.org/GROG/ansible-role-authorized-key
[tag_image]:            https://img.shields.io/github/tag/GROG/ansible-role-authorized-key.svg
[tag_url]:              https://github.com/GROG/ansible-role-authorized-key/tags
[gitter_image]:         https://badges.gitter.im/GROG/chat.svg
[gitter_url]:           https://gitter.im/GROG/chat

[grog.user]:            https://galaxy.ansible.com/GROG/user
[grog.sudo]:            https://galaxy.ansible.com/GROG/sudo
[grog.management-user]: https://galaxy.ansible.com/GROG/management-user

[issues]:               https://github.com/GROG/ansible-role-authorized-key/issues
[groggemans]:           https://github.com/groggemans
