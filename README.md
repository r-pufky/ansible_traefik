# Traefik
Traefik Application Proxy Server.

## Requirements
[supported platforms](https://github.com/r-pufky/ansible_traefik/blob/main/meta/main.yml)

## Role Variables
[defaults](https://github.com/r-pufky/ansible_traefik/tree/main/defaults/main)

### Ports
All ports and protocols have been defined for the role.

[defaults/ports.yml](https://github.com/r-pufky/ansible_traefik/blob/main/defaults/main/ports.yml)

## Dependencies
**galaxy-ng** roles cannot be used independently. Part of
[r_pufky.srv](https://github.com/r-pufky/ansible_collection_srv) collection.

## Example Playbook
WARNING
> Traefik runs as root to access privileged ports. Run in isolated container
> with no additional privileges or mounts. Service hardened with systemd.

Read defaults documentation.
[Additional documentation](http://r-pufky.github.io/r-pufky/docs/service/traefik).

Deploy Traefik with a minimal startup configuration.

host_vars/traefik.example.com/data/traefik.yml
``` yaml
log:
  level: 'DEBUG'

api:
  dashboard: true
  insecure: true
  debug: true
```

``` yaml
- name: 'Traefik server'
  hosts: 'traefik.example.com'
  become: true
  roles:
     - 'r_pufky.srv.traefik'
  vars:
    traefik_srv_cfg_dir: 'host_vars/traefik.example.com/data'
```

## Development
Configure [environment](https://r-pufky.github.io/ansible_collection_docs/ansible/environment)

Run all unit tests:
``` bash
molecule test --all
```

### Releases
Release format: **{OS}-{SERVICE}-{ROLE}**

Each type inherits the versioning system used; defaulting to schematic
versioning.

`12.0.0-2.0.3-1.0.0`

* 12.0.0 - Debian 12 (bookworm).
* 2.0.3 - Service/app version.
* 1.0.0 - Role version.

Releases are branched on Debian releases:

* **[13.x.x](https://github.com/r-pufky/ansible_traefik)**: 13 Trixie.
* **[12.x.x](https://github.com/r-pufky/ansible_traefik/tree/12.x)**: 12 Bookworm.

## Issues
Create a bug and provide as much information as possible.

Associate pull requests with a submitted bug.

## License
[AGPL-3.0 License](https://www.tldrlegal.com/license/gnu-affero-general-public-license-v3-agpl-3-0)
 [(direct link)](https://github.com/r-pufky/ansible_traefik/blob/main/LICENSE)

## Author Information
PGP Fingerprint: [466EEC2B67516C7117C85CE3A0BC35D16698BAB9](https://keys.openpgp.org/vks/v1/by-fingerprint/466EEC2B67516C7117C85CE3A0BC35D16698BAB9)
| [github gist](https://gist.github.com/r-pufky/a8df36977c55b5bb20829267c4c49d22)
