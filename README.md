Ansible role: pihole
=========

[![MIT licensed][mit-badge]][mit-link]
[![Galaxy Role][role-badge]][galaxy-link]

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

**ATTENTION!**

**vault_pihole_setupVars_conf_WEBPASSWD** and **vault_pihole_user_passwd** vars are set in *vars/main.yml*,
which is encrypted with [ansible-vault][ansible-vault-link].

Before running the role decrypt the file *vars/main.yml* with:

    ansible-vault decrypt vars/main.yml --vault-password-file=.vault.key`

OR set environment variable:

    export ANSIBLE_VAULT_PASSWORD_FILE=.vault.key

OR (PREFERRED):
add the following to **ansible.cfg**:

    [defaults]
    vault_password_file = .vault.key

Role Variables
--------------

| Variable | Description | Default |
|----------|-------------|---------|
| **pihole_force_upgrade** | Force reinstall `pihole` even if it's already installed |`no` |
| **pihole_setupVars_conf_WEBPASSWD** | Password for pihole web interface for quiet `pihole` installation | see [`defaults/main.yml`](defaults/main.yml) |
| **pihole_setupVars_conf** | Configuration for quiet `pihole` installation | see [`defaults/main.yml`](defaults/main.yml) |
| **pihole_dnsmasq_custom_conf** | Custom DNSMasq configuration | see [`defaults/main.yml`](defaults/main.yml) |
| **pihole_user_passwd** | SHA512 salted password for user `pihole` which the installer fails to create (hopefully will be
fixed in the future) | see [`defaults/main.yml`](defaults/main.yml) |
| **pihole_pitft** | Whether install drivers for pitft display from Adafruit (role pitft is no finished) | `no` |
| **pihole_dnscrypt** | Whether install DNSCrypt-proxy2 or not | `yes` |
| **pihole_dnscrypt_force_upgrade** | Force reinstall `dnscrypt-proxy2` even if it's already installed | `no` |
| **pihole_dnscrypt_proxy2_link** | url of the latest `dnscrypt-proxy2` version. Must be updated manually | `https://github.com/jedisct1/dnscrypt-proxy/releases/download/2.0.8/dnscrypt-proxy-linux_arm-2.0.8.tar.gz` |
| **pihole_dnscrypt_bin** | Where to put dnscrypt-proxy binary | `/usr/local/bin/dnscrypt-proxy` |
| **pihole_dnscrypt_config** | Where to store dnscrypt config | `/etc/dnscrypt-proxy/dnscrypt-proxy.toml` |
| **pihole_dnscrypt_socket_only** | Use ONLY socket activation of dnscrypt-proxy.service | `yes` |

### DNSCrypt-proxy config-related vars

| Variable | Description | Default |
|----------|-------------|---------|
| **pihole_dnscrypt_ipv4_address** | IPv4 address for `dnscrypt-proxy` to listen on | `127.0.0.1` |
| **pihole_dnscrypt_port** | Port for `dnscrypt-proxy` to listen on | `41` |
| **pihole_dnscrypt_ipv4_servers** | Use servers reachable over IPv4 | `'true'` |
| **pihole_dnscrypt_ipv6_servers** | Use servers reachable over IPv6. Do not enable if you don't have IPv6 set | `'false'` |
| **pihole_dnscrypt_dc_servers** | Use servers implementing the DNSCrypt protocol | `'true'` |
| **pihole_dnscrypt_doh_servers** | Use servers implementing the DNS-over-HTTPS protocol | `'true'` |
| **pihole_dnscrypt_require_dnssec** | Server must use DNSSEC validation | `'true'` |
| **pihole_dnscrypt_require_nolog** | Server must not log user queries (declarative) | `'true'` |
| **pihole_dnscrypt_require_nofilter** | Server must NOT enforce its own blacklist (for parental control, ads blocking) | `'true'` |
| **pihole_dnscrypt_force_tcp** | Always use TCP to connect to upstream servers. Useful for TOR. | `'false'` |
| **pihole_dnscrypt_fallback_resolver** | non-encrypted fallback resolver. Only used if DNS config doesn't work. `114.114.114.114:53` for people in China | `9.9.9.9:5` |

Dependencies
------------

None.

NOTE: If installing [dnscrypt-proxy2][dnscrypt-proxy2-link] then install the [drew-kun.dnscrypt][dnscrypt-galaxy-link] role first:

    ansible-galaxy install drew-kun.dnscrypt

Example Playbook
----------------

    - hosts: raspberrypi
      gather_facts: yes
      roles: drew-kun.pihole

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
