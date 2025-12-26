# Ansible Role for Grafana
This ansible role is meant to automate the process of deploying and configuring Grafana from binary, and hopefully, in the future, the process of [provisioning Grafana](https://github.com/grafana/grafana-ansible-collection/tree/main).

## Using the role

### Variables

All variables are defined in `defaults/main.yml` and can be overriden.

Here are variables which you are most likely to use:
```yaml
# Grafana binary URI which will be used for installation
grafana_bin_uri: "https://dl.grafana.com/grafana/release/12.3.1/grafana_12.3.1_20271043721_linux_amd64.tar.gz"

# Directory to which tarball will be unarchived
grafana_dir: "/usr/local/grafana"

# This sets grafana unit's restart mode
grafana_restart_on: "always"

# This is a key:value dictionary used for defining additional options of grafana systemd unit generated; see examples in defaults/main.yml
grafana_unit_options: {}

# This is a dictionary for configuration options which will be used to generate grafana.ini; see grafana documentation
grafana_ini: {}
```

### Example playbook

You have to provide ansible with become password if ansible user does not have NOPASSWD flag, nor is it root user, this is necessary for the role
```yaml
- hosts: grafana_hosts
  remote_user: ansible_user
  roles:
    - role: grafana
      vars:
        grafana_user: mycooluser
        grafana_plugins_dir: "{{ grafana_dir }}/plugins"
        grafana_unit_options:
          LimitNOFILE: "96000"
          AmbientCapabilities: "CAP_NET_BIND_SERVICE"
          CapabilityBoundingSet: "CAP_NET_BIND_SERVICE"
        grafana_ini:
          server:
            protocol: https
            http_port: 443
            cert_file: "/etc/grafana/fullchain.pem"
            cert_key: "/etc/grafana/cert.key"
```

## Contributing

Contributions are highly welcome. Ways to help:
* Bug reports and feature requests
* Pull requests with enhancements
* Tests and CI
