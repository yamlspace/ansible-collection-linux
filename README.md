# yamlspace.linux

[![License](https://img.shields.io/badge/license-GPLv3-lightgreen)](https://www.gnu.org/licenses/gpl-3.0.en.html#license-text)
[![Ansible Galaxy](https://img.shields.io/badge/collection-yamlspace.linux-blue)](https://galaxy.ansible.com/yamlspace/linux)
[![Lint](https://github.com/yamlspace/ansible-collection-linux/actions/workflows/pre-commit.yml/badge.svg)](https://github.com/yamlspace/ansible-collection-linux/actions/workflows/pre-commit.yml)
[![Release](https://github.com/yamlspace/ansible-collection-linux/actions/workflows/release.yml/badge.svg)](https://github.com/yamlspace/ansible-collection-linux/actions/workflows/release.yml)

A collection of linux roles

# Collection

## Build

    ansible-galaxy collection build

## Install

    ansible-galaxy collection install yamlspace.linux

## Molecule

    /usr/bin/python3 -m venv venv
    source venv/bin/activate
    pip3 install -r requirements.txt

# Available Roles

## [authorized_key](roles/authorized_key/README.md)
Manages SSH authorized keys for user accounts.

    - hosts: server
      roles:
        - role: yamlspace.linux.authorized_key
          authorized_key_list:
            - user: root
              exclusive: true
              key: >-
                ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIBwCfEABKtScVJMqT8kH0rdaisRopPeLqAUnRz4BKM7S

## [chrony](roles/chrony/README.md)
Configures time synchronization using chrony.

    - hosts: server
      roles:
        - role: yamlspace.linux.chrony
          chrony_conf:
            - authselectmode require
            - cmdport 0
            - driftfile /var/lib/chrony/drift
            - hwtimestamp *
            - log measurements statistics tracking
            - logdir /var/log/chrony
            - makestep 1.0 3
            - minsources 2
            - rtcsync
            - server ntppool1.time.nl iburst nts
            - server ntppool2.time.nl iburst nts

## [cloudflared](roles/cloudflared/README.md)
Manages Cloudflare Tunnel configurations.

    - hosts: server
      roles:
        - role: yamlspace.linux.cloudflared
          cloudflared_command: tunnel run -p http2
          cloudflared_tunnels:
            - name: example.com
              account_tag: 68b329da9893e34099c7d8ad5cb9c940
              id: ac5a6018-3af1-11ee-be56-0242ac120002
              ingress:
                - hostname: example.com
                  service: ssh://localhost:22
                - service: http_status:404

## [group](roles/group/README.md)
Manages Linux groups.

    - hosts: server
      roles:
        - role: yamlspace.linux.group
          group_list:
            - name: developers
              gid: 5000
            - name: operators
              gid: 5001

## [user](roles/user/README.md)
Manages Linux user accounts.

    - hosts: server
      roles:
        - role: yamlspace.linux.user
          user_list:
            - name: devuser
              uid: 5000
              group: developers
              groups: [docker, operators]
            - name: opsuser
              uid: 5001
              group: operators

## [docker](roles/docker/README.md)
Installs and configures Docker Engine.

    - hosts: server
      roles:
        - role: yamlspace.linux.docker
          docker_daemon_config:
            log-driver: "json-file"
            log-opts:
              max-size: "100m"
              max-file: "3"
          docker_users:
            - devuser
            - opsuser

## [cronie](roles/cronie/README.md)
Manages cron daemon and cron access configuration.

    - hosts: server
      roles:
        - role: yamlspace.linux.cronie
          cronie_allow:
            - root
            - admin
          cronie_deny:
            - guest

## [device_info](roles/device_info/README.md)
Gathers and manages device information.

    - hosts: server
      roles:
        - role: yamlspace.linux.device_info
          device_info_list:
            - /dev/sda
            - /dev/sdb

## [directory](roles/directory/README.md)
Manages directory creation and permissions.

    - hosts: server
      roles:
        - role: yamlspace.linux.directory
          directory_list:
            - path: /opt/data
              owner: app
              group: app
              mode: '0755'

## [dnf](roles/dnf/README.md)
Configures DNF package manager settings.

    - hosts: server
      roles:
        - role: yamlspace.linux.dnf
          dnf_conf:
            main:
              gpgcheck: 1
              installonly_limit: 2
              clean_requirements_on_remove: true

## [dnf_automatic](roles/dnf_automatic/README.md)
Configures automatic DNF updates.

    - hosts: server
      roles:
        - role: yamlspace.linux.dnf_automatic
          dnf_automatic_conf:
            commands:
              download_updates: 'yes'
              apply_updates: 'yes'
            emitters:
              system_name: 'example.com'
              emit_via: 'email'

## [dnf_versionlock](roles/dnf_versionlock/README.md)
Manages package version locking with DNF.

    - hosts: server
      roles:
        - role: yamlspace.linux.dnf_versionlock
          dnf_versionlock_list:
            - kernel-3.10.0-957.el7

## [docker_compose](roles/docker_compose/README.md)
Manages Docker Compose deployments.

    - hosts: server
      roles:
        - role: yamlspace.linux.docker_compose
          docker_compose_files:
            - name: webapp
              content:
                version: '3'
                services:
                  web:
                    image: nginx:latest
                    ports:
                      - "80:80"

## [docker_container](roles/docker_container/README.md)
Manages Docker containers.

    - hosts: server
      roles:
        - role: yamlspace.linux.docker_container
          docker_container_list:
            - name: webserver
              image: nginx:latest
              ports:
                - "80:80"

## [docker_network](roles/docker_network/README.md)
Manages Docker networks.

    - hosts: server
      roles:
        - role: yamlspace.linux.docker_network
          docker_network_list:
            - name: app_network
              driver: bridge
              ipam_config:
                - subnet: 172.20.0.0/16

## [elrepo](roles/elrepo/README.md)
Configures ELRepo repository.

    - hosts: server
      roles:
        - role: yamlspace.linux.elrepo
          elrepo_enabled: true
          elrepo_kernel: ml

## [epel](roles/epel/README.md)
Manages EPEL repository configuration.

    - hosts: server
      roles:
        - role: yamlspace.linux.epel
          epel_enabled: true

## [file](roles/file/README.md)
Manages file content and attributes.

    - hosts: server
      roles:
        - role: yamlspace.linux.file
          file_list:
            - path: /etc/myapp.conf
              content: |
                # My App Config
                port = 8080
              mode: '0644'

## [filesystem](roles/filesystem/README.md)
Manages filesystem creation and mounting.

    - hosts: server
      roles:
        - role: yamlspace.linux.filesystem
          filesystem_list:
            - dev: /dev/sdb1
              fstype: ext4
              opts: defaults

## [git](roles/git/README.md)
Manages Git repositories and configuration.

    - hosts: server
      roles:
        - role: yamlspace.linux.git
          git_config:
            user.name: "John Doe"
            user.email: "john@example.com"

## [hostnamectl](roles/hostnamectl/README.md)
Manages system hostname configuration.

    - hosts: server
      roles:
        - role: yamlspace.linux.hostnamectl
          hostnamectl_hostname: "webserver.example.com"

## [kopia](roles/kopia/README.md)
Manages Kopia backup configuration.

    - hosts: server
      roles:
        - role: yamlspace.linux.kopia
          kopia_server_enabled: true
          kopia_server_address: "0.0.0.0:51515"

## [localectl](roles/localectl/README.md)
Manages system locale settings.

    - hosts: server
      roles:
        - role: yamlspace.linux.localectl
          localectl_keymap: us
          localectl_locale: en_US.UTF-8

## [logind](roles/logind/README.md)
Configures systemd-logind behavior.

    - hosts: server
      roles:
        - role: yamlspace.linux.logind
          logind_conf:
            Login:
              HandleLidSwitch: ignore
              IdleAction: suspend

## [logrotate](roles/logrotate/README.md)
Manages log rotation configuration.

    - hosts: server
      roles:
        - role: yamlspace.linux.logrotate
          logrotate_conf:
            - path: /var/log/*.log
              options:
                - weekly
                - rotate 4
                - compress

# Example Playbook

An example playbook utilizing multiple roles from this collection:

    - hosts: server
      vars:
        global_users:
          - name: johnd
            id: 2000
            key: ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIIRtpHq0ih6ZsXzskVMqHLc3bvCp82l1lS/V9i3wXwQQ
          - name: janed
            id: 2001
            key: ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIEQYZwthruEeeRtn4QE2x5xeVosMNha99UOVptoNjVbs

      roles:
        - role: yamlspace.linux.group
          group_list:
            "{{ (global_users |
                json_query('[].{
                  name: name,
                  gid: id
                }')) |
                d([]) }}"

        - role: yamlspace.linux.user
          user_list:
            "{{ (global_users |
                json_query('[].{
                  name: name,
                  uid: id
                }')) |
                d([]) }}"

        - role: yamlspace.linux.authorized_key
          authorized_key_list:
            "{{ (global_users |
                json_query('[].{
                  user: name,
                  key: key,
                  exclusive: `true`
                }')) |
                d([]) }}"
