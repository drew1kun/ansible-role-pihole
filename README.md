Ansible role: pihole
=========

[![MIT licensed][mit-badge]][mit-link]
[![Galaxy Role][role-badge]][galaxy-link]

**[DEPRECATED]**

*(Left here for reference only)*

Ansible role which installs and configures [Pihole][pihole-link] and [DNSCrypt-proxy2][dnscrypt-proxy2-link] on RaspberryPi.

The role achieves the following:
 - Installs and configures pihole
 - Installs dnscrypt-proxy 2
 - Configures dnscrypt-proxy to be run as unprivileged user [ for security purposes ]
 - If distro uses systemd init: configures systemd socket activated dnscrypt-proxy.service
 - If distro does not use systemd: configures built in dnscrypt-proxy service

Requirements
------------

NOTE: Role requires Fact Gathering by ansible!

One of the following distros (or derivatives) required:
 - Debian | Raspbian | [Minibian][minibian-link]
    - jessie
    - stretch

Role Variables
--------------

| Variable | Description | Default |
|----------|-------------|---------|
| **pihole_force_upgrade** | Force reinstall `pihole` even if it's already installed |`no` |
| **pihole_setupVars_conf_WEBPASSWD** | Password for pihole web interface for quiet `pihole` installation | see [`defaults/main.yml`](defaults/main.yml#L17) |
| **pihole_setupVars_conf** | Configuration for quiet `pihole` installation | see [`defaults/main.yml`](defaults/main.yml#L19) |
| **pihole_dnsmasq_custom_conf** | Custom DNSMasq/pihole-FTL configuration | see [`defaults/main.yml`](defaults/main.yml#L40) |

### Docker-related vars

| Variable | Description | Default |
|----------|-------------|---------|
| **pihole_docker** | Whether to use docker or not | `no` |
| **pihole_docker_ports** | List of ports to expose in docker container | `[127.0.0.1:53:53/udp, 0.0.0.0:67:67/udp, 0.0.0.0:80:80/tcp, 0.0.0.0:443:443/tcp]` |
| **pihole_docker_env** | Dictionary, containing the host ipv4/ipv6 addresses | `{ ServerIP: 192.168.1.247 }` |
| **pihole_docker_dns_servers** | List of DNS servers for pihole docker container to use | `[127.0.0.1]` |


Dependencies
------------

None.

Example Playbook
----------------

```yaml
    - hosts: raspberrypi
      gather_facts: yes
      roles:
      - role: drew-kun.pihole
        pihole_setupVars_conf_WEBPASSWD: "{{ vault_pihole_setupVars_conf_WEBPASSWD }}"
```

License
-------

[MIT][mit-link]

Author Information
------------------

Andrew Shagayev | [e-mail](mailto:drewshg@gmail.com)

[role-badge]: https://img.shields.io/badge/role-drew--kun.pihole-green.svg
[galaxy-link]: https://galaxy.ansible.com/drew-kun/pihole/
[mit-badge]: https://img.shields.io/badge/license-MIT-blue.svg
[mit-link]: https://raw.githubusercontent.com/drew-kun/ansible-pihole/master/LICENSE
[minibian-link]: https://minibianpi.wordpress.com/
[pihole-link]: https://pi-hole.net/
[dnscrypt-proxy2-link]: https://github.com/jedisct1/dnscrypt-proxy
[dnscrypt-galaxy-link]: https://galaxy.ansible.com/drew-kun/dnscrypt/
