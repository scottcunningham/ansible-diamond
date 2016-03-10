# diamond

[![License](https://img.shields.io/badge/license-New%20BSD-blue.svg?style=flat)](https://raw.githubusercontent.com/saucelabs-ansible/diamond/master/LICENSE)
[![Build Status](https://travis-ci.org/saucelabs-ansible/diamond.svg?branch=master)](https://travis-ci.org/saucelabs-ansible/diamond)

[![Platform](http://img.shields.io/badge/platform-ubuntu-dd4814.svg?style=flat)](#)

[![Project Stats](https://www.openhub.net/p/saucelabs-ansible-diamond/widgets/project_thin_badge.gif)](https://www.openhub.net/p/saucelabs-ansible-diamond/)

Ansible role to setup [diamond](https://github.com/python-diamond/Diamond).


## Tests

| Family | Distribution | Version | Test Status |
|:-:|:-:|:-:|:-:|
| Debian | Ubuntu  | Trusty  | [![x86_64](http://img.shields.io/badge/x86_64-passed-006400.svg?style=flat)](#) |


## Requirements

- ansible >= 1.9.4


## Role Variables

- **diamond_conf_server**: diamond `[server]` section.
- **diamond_conf_handlers_main**: diamond `[handlers]` main section.
- **diamond_conf_handlers_default**: diamond `[handlers][[defaults]]` section.
- **diamond_conf_handlers**: diamond `[handlers][[SomeHandler]]` sections.
- **diamond_conf_collectors_default**: diamond `[collectors][[defaults]]` section.
- **diamond_conf_collectors**: diamond `[collectors][[SomeCollector]]` sections.
- **diamond_conf_logging_main**: diamond `[loggers]`/`[formatters]` configuration sections.
- **diamond_conf_logging_loggers**: diamond `[logger_*]` configuration sections.
- **diamond_conf_logging_handlers**: diamond `[handler_*]` configuration sections.
- **diamond_conf_logging_formatters**: diamond `[formatter_*]` configuration sections.
- **diamond_default_enabled**: flag to indicate if diamond should be enabled.
- **diamond_default_pid**: path to the file that will store the daemon pid.
- **diamond_default_user**: run the daemon under this user.
- **diamond_version**: the version to be installed.
- **debug**: flag to run debug tasks (default: false).

Unless stated otherwise
a default value is provided for each of the variables mentioned above
in `defaults/main.yml`.


## Dependencies

None.


## Playbooks

Including an example of how to use your role
(for instance, with variables passed in as parameters)
is always nice for users too:

    - hosts: servers
      vars:
        debug: yes
        diamond_default_enabled: yes
        diamond_default_pid: /var/run/diamond.pid
        diamond_default_user: diamond
        diamond_version: 4.0.195
        diamond_conf_server:
          handlers: diamond.handler.archive.ArchiveHandler
          user: ''
          group: ''
          pid_file: /var/run/diamond.pid
          collectors_path: /usr/share/diamond/collectors/
          collectors_config_path: /etc/diamond/collectors/
          handlers_config_path: /etc/diamond/handlers/
          handlers_path: /usr/share/diamond/handlers/
        diamond_conf_handlers_main:
          keys: rotated_file
        diamond_conf_handlers_default: {}
        diamond_conf_handlers:
          ArchiveHandler:
            log_file: /var/log/diamond/archive.log
            days: 7
        diamond_conf_collectors_default: {}
        diamond_conf_collectors:
          CPUCollector:
            enabled: 'True'
          DiskSpaceCollector:
            enabled: 'True'
          DiskUsageCollector:
            enabled: 'True'
          LoadAverageCollector:
            enabled: 'True'
          MemoryCollector:
            enabled: 'True'
          VMStatCollector:
            enabled: 'True'
        diamond_conf_logging_main:
          loggers:
            keys: root
          formatters:
            keys: default
        diamond_conf_logging_loggers:
          root:
            level: INFO
            handlers: rotated_file
            propagate: 1
        diamond_conf_logging_handlers:
          rotated_file:
            class: handlers.TimedRotatingFileHandler
            level: DEBUG
            formatter: default
            args: "('/var/log/diamond/diamond.log', 'midnight', 1, 7)"
        diamond_conf_logging_formatters:
          default:
            format: "[%(asctime)s] [%(threadName)s] %(message)s"
            datefmt: ''

      roles:
         - role: saucelabs.diamond
           tags: diamond


## Tags

- **configuration**: configuration tasks.
- **debug**: role variables debug task.
- **upstart**: tasks related to upstart.
- **installation**: installation tasks.
- **validation**: role variables validation task.

## Test

To run the tests you will need to install:

- [tox](https://tox.readthedocs.org/)
- [vagrant](https://www.vagrantup.com/)

To run all tests against all pre-defined OS/distributions * ansible versions:

```
$ tox
```

To run tests for `trusty64`:

```
$ cd tests
$ bash test_idempotence.sh --box trusty64.vagrant.dev
# log file will be stores under tests/log
```

To perform debugging on a specific environment:

```
$ cd tests
$ vagrant up trusty64.vagrant.dev

# to provision using the test.yml playbook (as many time as you need)
$ vagrant provision trusty64.vagrant.dev

# to access the Vagrant box
$ vagrant ssh trusty64.vagrant.dev
```


## Links

- [diamond.readthedocs.org](http://diamond.readthedocs.org/)
- [diamond @ github](https://github.com/python-diamond/Diamond/)
- [why ubuntu/precise is not supported](https://bugs.launchpad.net/ubuntu/+source/stdeb/+bug/1316521)


## License

BSD


## Author Information

- [steenzout](https://github.com/steenzout/)
