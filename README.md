<img src="http://www.elao.com/images/corpo/logo_red_small.png"/>

# Ansible Role: grafana

This role will assume the setup of grafana

It's part of the ELAO [Ansible stack](http://ansible.elao.com) but can be used as a stand alone component.

## Requirements

- Ansible 1.7.2+

## Dependencies

None.

## Installation

Using ansible galaxy:

```bash
ansible-galaxy install elao.grafana
```
You can add this role as a dependency for other roles by adding the role to the meta/main.yml file of your own role:

```yaml
dependencies:
  - { role: elao.grafana }
```

## Role Handlers

| Name            | Type    | Description            |
| --------------- | ------- | ---------------------- |
| grafana restart | Service | Restart grafana server |

## Role Variables

| Name                         | Default                  | Type   | Description |
| ---------------------------- | ------------------------ | ------ | ----------- |
| elao_grafana_config_file     | /etc/grafana/grafana.ini | string |             |
| elao_grafana_config_template | config/base.ini.j2       | string |             |
| elao_grafana_config          | []                       | Array  |             |
| elao_grafana_api_url         | http://127.0.0.1:3000    | string |             |
| elao_grafana_api_user        | admin                    | string |             |
| elao_grafana_api_password    | admin                    | string |             |
| elao_grafana_datasources     | []                       | Array  |             |
| elao_grafana_dashboards      | []                       | Array  |             |

### Configuration example

See : http://docs.grafana.org/installation/configuration/

```yaml
elao_grafana_config:
  - app_mode: production
  - server:
    - http_port: 3001
  - security:
    - admin_user: admin
    - admin_password: admin
```

Configure datasources :

```yaml
elao_grafana_datasources:
  - name: influxdb
    type: influxdb
    isDefault: true
    url: http://127.0.0.1:8086
    access: proxy
    basicAuth: false
    database: stats
    user: admin
    password: admin
```

Configure dashboards :

  * Template need to be a valid json respecting [grafana API schema](http://docs.grafana.org/reference/http_api/#create-update-dashboard)
  * Set id = null to create a new dashboard
  * Set overwrite = true to overwrite existing dashboard.

```yaml
elao_grafana_dashboards:
  - name: default
    template: "{{ playbook_dir }}/templates/grafana/dashboards/default.json"
```

## Example playbook

    - hosts: servers
      roles:
         - { role: elao.grafana }

# Licence

MIT

# Author information

ELAO [**(http://www.elao.com/)**](http://www.elao.com)
