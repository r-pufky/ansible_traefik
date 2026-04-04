# [Traefik][h]
Traefik Application Proxy Server.

## [Requirements][i]
Requires [r_pufky.srv][g] galaxy-ng collection. See
[additional documentation][m] and [reference documentation][h] for
troubleshooting and config variables.

Install size: ~170MB

> Any version of traefik may be run.

## Role Variables
Detailed variable use documented in defaults. See usage for role operation.

* [defaults][j] - User configurable options.

* [ports][k] - Ports are **not** managed (defined for external use).

## Usage
**htpasswd** is installed with Traefik for manual generation of basic auth
password hashes. See [documentation][j].

 Path             | Usage
 -----------------|-------
 /etc/opt/treafik | Traefik static, dynamic configuration.
 /var/opt/traefik | Traefik working directory, read/write area for service.

### Feature Flags
Tasks are gated by feature flags and executed in the following order.

  Step | Flag                    | Notes
 ------|-------------------------|-------
  1    | traefik_flg_maintenance | Preform role maintenance tasks.
  2    | traefik_flg_install     | Install required packages, users, etc.
  3    | traefik_flg_config      | Install user-defined config.

### Example Playbooks

#### New deployment
Traefik will be serving the default insecure dashboard showing the service is
running. This is good to get an initial working deployment to further
configure but should not be used in production. See
[Pre-configured Deployments](#pre-configured-deployment).

``` yaml
- name: 'Create new Traefik instance, insecure dashboard, running as traefik user.'
  ansible.builtin.include_role:
    name: 'r_pufky.srv.traefik'
```

#### Pre-configured deployment
Traefik deployments will typically use this. It will deploy a pre-configured
instance and optionally ansible-sourced templated files to be used by the
Traefik service.

Traefik configuration is complex and nuanced. See [reference documentation][h]
keeping in mind where the [role places files](#usage).

Example traefik.yml
``` ini
---
# Always store ACME cert state (or any state) in /var/opt/traefik.
log:
  level: 'DEBUG'
  format: 'json'

accessLog:
  format: 'json'

api:
  dashboard: true
  disableDashboardAd: true
  insecure: false
  debug: true

# Dynamically load all other configuration.
providers:
  file:
    directory: '/etc/opt/traefik/dynamic'
    watch: true
```

Example dynamic/middleware.yml
``` yaml
http:
  middlewares:
    basic_auth_users:
      basicAuth:
        users:
          - '{{ vault_user }}'
```

``` yaml
- name: 'Deploy Traefik, setting up GCP environment for lego lets encrypt certs.'
  ansible.builtin.include_role:
    name: 'r_pufky.srv.traefik'
  vars:
    traefik_cfg_env:
      - var: 'GCE_PROJECT'
        value: 'my-project-id-123456789'
      - var: 'GCE_SERVICE_ACCOUNT_FILE'
        value: '/etc/opt/traefik/gcloud_dns.json'
    traefik_cfg_d: 'host_vars/traefik.example.com/conf.d'
```

## Development
Configure [environment][a].

``` bash
# Run all tests.
molecule test --all
```

Testing variables:

  Variable            | type | Description
 ---------------------|------|-------------
  molecule_flg_inject | bool | Flag to inject files locally.

### [Releases][b]

  Release | Debian | Ansible | Traefik | Notes
 ---------|--------|---------|---------|-------
  3.x.x   | 13     | 2.20    | v3.6.12 | Ansible 2.20, feature flags, and semantic versioning.
  2.x.x   | 13     | 2.18    | v3.5.6  | Add configurable service environment.
  1.x.x   | 13     | 2.18    | v3.5.3  | Initial release.

## Issues
Create a bug and provide as much information as possible.

Associate pull requests with a submitted bug.

## License
[AGPL-3.0 License][c] | [direct link][f]

## Author Information
PGP: [466EEC2B67516C7117C85CE3A0BC35D16698BAB9][d] | [github gist][e]


[a]: https://r-pufky.github.io/ansible_docs
[b]: https://semver.org/spec/v2.0.0
[c]: https://www.tldrlegal.com/license/gnu-affero-general-public-license-v3-agpl-3-0
[d]: https://keys.openpgp.org/vks/v1/by-fingerprint/466EEC2B67516C7117C85CE3A0BC35D16698BAB9
[e]: https://gist.github.com/r-pufky/a8df36977c55b5bb20829267c4c49d22

[f]: https://github.com/r-pufky/ansible_traefik/blob/main/LICENSE
[g]: https://github.com/r-pufky/ansible_collection_srv
[h]: https://doc.traefik.io/traefik/getting-started/configuration-overview/
[i]: https://github.com/r-pufky/ansible_traefik/blob/main/meta/main.yml
[j]: https://github.com/r-pufky/ansible_traefik/tree/main/defaults/main/main.yml
[k]: https://github.com/r-pufky/ansible_traefik/blob/main/defaults/main/ports.yml
[m]: http://r-pufky.github.io/docs/service/traefik