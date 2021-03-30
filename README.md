# Ansible Role: Alertmanager for MSTeams

[![ubuntu-18](https://img.shields.io/badge/ubuntu-18.x-orange?style=flat&logo=ubuntu)](https://ubuntu.com/)
[![ubuntu-20](https://img.shields.io/badge/ubuntu-20.x-orange?style=flat&logo=ubuntu)](https://ubuntu.com/)
[![debian-9](https://img.shields.io/badge/debian-9.x-orange?style=flat&logo=debian)](https://www.debian.org/)
[![debian-10](https://img.shields.io/badge/debian-10.x-orange?style=flat&logo=debian)](https://www.debian.org/)
[![centos-7](https://img.shields.io/badge/centos-7.x-orange?style=flat&logo=centos)](https://www.centos.org/)
[![centos-8](https://img.shields.io/badge/centos-8.x-orange?style=flat&logo=centos)](https://www.centos.org/)
[![License](https://img.shields.io/badge/license-MIT%20License-brightgreen.svg?style=flat)](https://opensource.org/licenses/MIT)
[![GitHub issues](https://img.shields.io/github/issues/OnkelDom/ansible-role-alertmanager-msteams?style=flat)](https://github.com/OnkelDom/ansible-role-alertmanager-msteams/issues)
[![GitHub tag](https://img.shields.io/github/tag/OnkelDom/ansible-role-alertmanager-msteams.svg?style=flat)](https://github.com/OnkelDom/ansible-role-alertmanager-msteams/tags)

## Description

Deploy [MSTeams Alert](https://github.com/prometheus-msteams/prometheus-msteams) using ansible.

## Requirements

- Ansible >= 2.9 (It might work on previous versions, but we cannot guarantee it)
- Community Packages: `ansible-galaxy collection install community.general`

## Role Variables

All variables which can be overridden are stored in [defaults/main.yml](defaults/main.yml) file as well as in table below.

| Name           | Default Value | Description                        |
| -------------- | ------------- | -----------------------------------|
| `proxy_env` |  {} | Set proxy environment variables |
| `msteams_version` | 1.4.1 | MSTeams download Version |
| `msteams_binary_install_dir` | /usr/local/bin | default bin dir |
| `msteams_config_dir` | /etc/msteams | Config Path |
| `msteams_template_dir` | "{{ msteams_config_dir }}/templates" | Template store path |
| `msteams_config_file` | config.yml | Config file name |
| `msteams_allow_firewall` | false | Allow access on firewalld port |
| `msteams_binary_install_dir` | "/usr/local/bin" | Base binary path |
| `msteams_system_user` | prometheus | User for Consul Template |
| `msteams_system_group` | prometheus | Group for Consul Template |
| `msteams_web_listen_port` | 2000 | MSTeams Alert listen Port |
| `msteams_web_listen_address` | 0.0.0.0 | MSTeams Alert listen Address |
| `msteams_config_channels` | {} | Configure MSTeams channels |
| `msteams_config_custom_channels` | {} | Configure MSTeams custom channels |
| `msteams_default_template` | default-message-card | default message template |
| `msteams_http_proxy` | null | set proxy to use for alert sending |
| `msteams_https_proxy` | null | set proxy to use for alert sending |
| `msteams_disable_autoescape_underscores` | true | disable autoescape underscore |

## Example

### Playbook

```yaml
---
- hosts: all
  roles:
  - onkeldom.alertmanager_msteams
  vars:
    alertmanager_receivers:
      - name: 'msteams_channel1'
        webhook_configs:
        - send_resolved: true
          url: http://localhost:2000/channel1
      - name: 'msteams_channel2'
        webhook_configs:
        - send_resolved: true
          url: http://localhost:2000/channel12
  alertmanager_child_routes:
    - receiver: "msteams_channel1"
      continue: false
      group_wait: 4h
      match_re:
        hostname: "^(.+prod\\.exmaple\\.com)|(server.+)"
    - receiver: "msteams_channel2"
      continue: false
      group_wait: 4h
      match_re:
        hostname: "^(.+test\\.exmaple\\.com)|(server-test.+)"
```

## Contributing

See [contributor guideline](CONTRIBUTING.md).

## License

This project is licensed under MIT License. See [LICENSE](/LICENSE) for more details.
