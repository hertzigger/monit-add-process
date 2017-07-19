# Monit Add Process

## Description

This is an ansible project built to allow you to add new processes to an already installed monit instance. Please see [Monit-Install](https://github.com/hertzigger/monit-install) for monit installation.

## Dependency's

None

## Supported OS

centos


## Example Usage

### Playbook

```yaml
- hosts: all
  become: true
  vars:
    monit_add_process_name: 'ssh'
    monit_add_process_type: 'process'
    monit_add_process_with: "pidfile /var/run/sshd.pid"
    monit_add_process_start_program:
      command: '/usr/bin/systemctl start sshd.service'
    monit_add_process_stop_program:
      command: '/usr/bin/systemctl stop sshd.service'
    monit_add_process_check_options:
      - test: 'FAILED'
        action: 'RESTART'
        checks:
          - 'port 22 protocol ssh'
      - test: 'DOES NOT EXIST'
        action: 'RESTART'
      - test: '5 restarts'
        action: 'alert'
        checks:
          - 'within 5 cycles'
  roles:
  - role: monit-add-process
```

### Include Role

```yaml
- include_role:
    name: monit-add-process
  vars:
    monit_add_process_name: 'ssh'
    monit_add_process_type: 'process'
    monit_add_process_with: "pidfile /var/run/sshd.pid"
    monit_add_process_start_program:
      command: '/usr/bin/systemctl start sshd.service'
    monit_add_process_stop_program:
      command: '/usr/bin/systemctl stop sshd.service'
    monit_add_process_check_options:
      - test: 'FAILED'
        action: 'RESTART'
        checks:
          - 'port 22 protocol ssh'
      - test: 'DOES NOT EXIST'
        action: 'RESTART'
      - test: '5 restarts'
        action: 'alert'
        checks:
          - 'within 5 cycles'
  when: monit_install_run
```