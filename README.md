> [!WARNING]
> This role is still a **WIP**. There is no guarantee that this role will achieve the desired result.
> As soon as a stable version is available, there will be the first release.

# Ansible Role - `sombeyyy.docker`

This is an [Ansible](https://www.ansible.com) role that installs and configures [Docker](https://www.docker.com) on
Linux systems.

## Requirements

None.

## Dependencies

None.

## Role Variables

| Variable                        | Default                                                                                                                                                                                                            | Description                                                                                                             |
|---------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------|
| docker_clean_install            | `false`                                                                                                                                                                                                            | Removes the Docker Engine and all Docker objects on target                                                              |
| docker_packages                 | `[ docker-ce, docker-ce-cli, containerd.io, docker-buildx-plugin, docker-compose-plugin ]`                                                                                                                         | List of packages to be installed                                                                                        |
| docker_add_repo                 | `true`                                                                                                                                                                                                             | Whether a Docker repository should be added or not. If `false`, the repository mus be added independently               |
| docker_repo_url                 | `https://download.docker.com/linux`                                                                                                                                                                                | The base URL to the Docker repository                                                                                   |
| docker_apt_release_channel      | `stable`                                                                                                                                                                                                           | The release channel for the repository (only for Debian-based operating systems)                                        |
| docker_apt_ansible_distribution | `{{ 'ubuntu' if ansible_distribution in ['Pop!_OS', 'Linux Mint'] else ansible_distribution }}`                                                                                                                    | The distribution of the target (only for Debian-based operating systems)                                                |
| docker_apt_arch                 | `{{ 'arm64' if ansible_architecture == 'aarch64' else 'armhf' if ansible_architecture == 'armv7l' else 'amd64' }}`                                                                                                 | The architecture of the target (only for Debian-based operating systems)                                                |
| docker_apt_repository           | `deb [arch={{ docker_apt_arch }} signed-by=/etc/apt/keyrings/docker.asc] {{ docker_repo_url }}/{{ docker_apt_ansible_distribution \| lower }} {{ ansible_distribution_release }} {{ docker_apt_release channel }}` | The Docker repository URL (only for Debian-based operating systems)                                                     |
| docker_apt_gpg_key              | `{{ docker_repo_url }}/{{ docker_apt_ansible_distribution \| lower }}/gpg`                                                                                                                                         | The GPG key for the Docker Repository (only for Debian-based operating systems)                                         |
| docker_apt_filename             | `docker`                                                                                                                                                                                                           | The name of the Docker Repository filename (only for Debian-based operating systems)                                    |
| docker_yum_repo_url             | `{{ docker_repo_url }}/{{ ansible_distirbution \| lower }}/docker-ce.repo`                                                                                                                                         | The Docker repository URL (only for RedHat-based operating systems)                                                     |
| docker_yum_gpg_key              | `{{ docker_repo_url }}/{{ ansible_distribution \| lower }}/gpg`                                                                                                                                                    | The GPG key for the Docker Repository (only for RedHat-based operating systems)                                         |
| docker_yum_gpg_fingerprint      | `060A 61C5 1B55 8A7F 742B 77AA C52F EB6B 621E 9F35`                                                                                                                                                                | The fingerprint of the GPG key (only for RedHat-based operating systems)                                                |
| docker_service_manage           | `true`                                                                                                                                                                                                             | Whether the Docker service should be managed ot nor                                                                     |
| docker_service_state            | `started`                                                                                                                                                                                                          | The status that the Docker service should have                                                                          |
| docker_service_enabled          | `true`                                                                                                                                                                                                             | Whether the Docker service should start on boot                                                                         |
| docker_users                    | `[ ]`                                                                                                                                                                                                              | A list of users to be added to the `docker` group (so they can use Docker on the machine)                               |
| docker_daemon_options           | `{ }`                                                                                                                                                                                                              | Custom `dockerd` options can be configured through this dictionary representing the json file `/etc/docker/daemon.json` |

## Example Playbook

```yaml
---
- hosts: all

  vars:
    docker_clean_install: true
    docker_users:
      - user1
      - user2
    docker_daemon_options:
      storage_driver: "devicemapper"
      log-opts:
        max.size: "100m"

  roles:
    - sombeyyy.docker
```

## License

This project is licensed under the [MIT License](LICENSE)

## Author Information

This role was created in 2024 by Sombeyyy
