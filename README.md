# Sudo Role for Ansible

[![Build Status](https://travis-ci.org/petemcw/ansible-role-sudo.svg?branch=master)](https://travis-ci.org/petemcw/ansible-role-sudo)

This role manages the `/etc/sudoers` file and `/etc/sudoers.d` directory.

## Role Variables

The variables that can be passed to this role and a brief description about
them are as follows:

```yaml
# Default administrative group
sudo_admin_group: "sudo"

# System locations
sudo_config_file: "/etc/sudoers"
sudo_config_path: "/etc/sudoers.d/"

# Items that get generated into /etc/sudoers.d/{item} files
sudo_config_items: []

# User definitions
sudo_users: []

# Default definitions
sudo_defaults:
  - "!requiretty"
  - "!visiblepw"
  - always_set_home
  - env_reset

# Alias definitions
sudo_command_aliases: []
sudo_host_aliases: []
sudo_user_aliases: []
sudo_run_aliases: []
```

## Examples

1. Configure sudo with the defaults and a deploy user:

    ```yaml
    ---
    # This playbook configures sudo

    - name: Configure sudo on all nodes
      hosts: all
      roles:
        - sudo
      vars:
        sudo_users:
          - name: "deploy"
            nopasswd: true
            commands:
              - deployment

        sudo_command_aliases:
          - name: deployment
            alias: "/usr/bin/rsync, /usr/sbin/service"
    ```

2. Configure sudo with more complex options:

    ```yaml
    ---
    # This playbook configures sudo

    - name: Configure sudo on all nodes
      hosts: all
      roles:
        - sudo
      vars:
        sudo_config_items:
          - name: test_user
            hosts: 
              - intnet
            as_users: 
              - op
            nopasswd: true
            commands:
              - printing
        sudo_users:
          - name: linus
            as_users:
              - op
          - name: user2
            hosts:
              - intnet
          - name: user3
            commands:
              - printing
            nopasswd: true
        sudo_command_aliases:
          - name: printing
            alias: "/usr/sbin/lpc, /usr/bin/lprm"
        sudo_host_aliases:
          - name: intnet
            alias: "10.1.2.0/255.255.255.0"
        sudo_user_aliases:
          - name: operators
            alias: joe, mike, jude
        sudo_run_aliases:
          - name: op
            alias: root, operator
    ```

## Dependencies

* Ansible >= 1.9

## License

MIT
