# Ansible role docker  [![Build Status](https://travis-ci.org/datadrivers/ansible-role-docker.png?branch=master)](https://travis-ci.org/datadrivers/ansible-role-docker)

This role installs and configures Docker, ctop, docker-compose, and Nvidia supported GPU-Docker (with Nvidia drivers and Nvidia CUDA). To manage the installation of all components, check the default values of the role variables.

## Requirements

* Ansible 2.1+
* SSH access and sudo permissions
* target host with installed linux OS with systemd
* for nvidia-docker usage: a host with a compatible Nvidia graphic card
* Internet access to get the packages and gpg keys

## Supported platforms

* CentOS 7
* RedHat 7

### Limitations
This role isn't support Mac and/or Windows systems. Nvidia-docker (Nvidia GPU support for docker) only supports linux based environment. The role overwrites the configuration file `/etc/docker/daemon.json`, if you install the nvidia-docker components.

## Installation

To add this role to your ansible playbook, define or add it to the `requirements.yml`:

```yaml
# datadrivers_docker
- src: https://github.com/datadrivers/ansible-role-docker.git
  version: master
  name: datadrivers_docker
```

After that, runs the ansible-galaxy install task:
```bash
$ ansible-galaxy install -v -r requirements.yml
```

## Role Variables

Please check `defaults/main.yml` for additional & exact variables and values, that can be overridden.

| Variable                    | Description                         | Example or Defaults                 |
|-----------------------------|-------------------------------------|---------------------------------|
| `aws_docker_ansible_ssh_user`       | Ansible ssh user for the example playbook | `centos` |
| `docker_repo_name` | Naming of the Docker repository | `docker-ce` |
| `docker_repo_file_name` | Define the file name of the docker repository file  | `docker-ce` |
| `docker_software_version` | Which software channel/branch of Docker wants to use (stable, edge, etc.)? | `stable` |
| `docker_repo_description` | Define the repository description | `Docker CE` |
| `docker_repo_base_url` | The Docker repository baseurl | `https://download.docker.com/linux/centos/7/$basearch/{{ docker_software_version }}` |
| `docker_repo_gpg_key` | The Docker repository gpg key | `https://download.docker.com/linux/centos/gpg` |
| `docker_repo_gpg_check` | Activate/Deactivate gpg check for this repository | `1` |
| `docker_repo_enabled` | Activate the repository in YUM | `1` |
| `docker_repo_ssl_verify` | Verify SSL/TLS certs | `yes` |
| `docker_package_name` | The name of the install package | `docker-ce` |
| `docker_install_python_package` | Install docker python package | `yes` |
| `docker_http_proxy_active` | Configure Docker Proxy settings (systemd) | `false` |
| `docker_http_proxy` | Docker http proxy settings for an existing company proxy | `http://1.2.3.4:8032` |
| `docker_https_proxy` | Docker https proxy settings for an existing company proxy | `https://1.2.3.4:8032` |
| `docker_no_proxy` | Docker no_proxy settings | `localhost,127.0.0.1,localaddress,.localdomain.com,169.254.169.254,169.254.170.2,/var/run/docker.sock` |
| `docker_group_membership_active` | Add users to system group docker (unsafe!) | `false` |
| `docker_group_membership_users` | Dict formatted list of users, who will get the docker group membership applied | `- centos` |
| `docker_install_ctop` | Install ctop - a top-like tool for docker | `true` |
| `docker_ctop_github_release_url` | ctop release site on Github (API) | `https://api.github.com/repos/bcicen/ctop/releases` |
| `docker_ctop_destination_path` | Where to put in the downloaded tool ctop | `/usr/local/bin` |
| `docker_ctop_ssl_verify` | Verify SSL/TLS for the installation process | `yes` |
| `docker_install_docker_compose` | Install docker-compose to extend the normal Docker | `true` |
| `docker_compose_github_release_url` | docker-compose release site on Github (API)  | `https://api.github.com/repos/docker/compose/releases` |
| `docker_compose_destination_path` | Where to put in the downloaded tool docker-compose | `/usr/local/bin` |
| `docker_compose_ssl_verify` | Verify SSL/TLS for the installation process | `yes` |
| `docker_install_aws_iptables` | Apply AWS specific iptables policies to reach the internal metadata API interfaces on virtualisation host | `true` |
| `docker_install_nvidia_docker`| Install Nvidia (CUDA) driver and nvidia-docker2 for GPU-Support | `false` |

## Additional Information

In this role is a custom python library script called `iptables_raw.py` as Ansible module to apply the specific iptables policies. Check the file `library/iptables_raw.py` for more details.
The supported nvidia graphic cards are listed by the drivers tool `nvidia-detect`.
For more current information to nvidia-docker, see the [Github repository](https://github.com/NVIDIA/nvidia-docker).

## Example Playbook

```yaml
# CentOS example
- hosts: docker-host
  user: centos
  become: yes #optional
  become_user: root #optional
  gather_facts: true
  roles:
     - datadrivers_docker
```

## ToDo

Please check the file `TODO.md` for more details.

## License

* MIT license (check file `LICENSE` for more details)

## Issue Tracker

Please use the [Github Repository Issue tracker](https://github.com/datadrivers/ansible-role-docker/issues) to report us bugs.

## Contributions

Feel free to clone the code, make a branch and to hack your stuff. After that make a pull request and we can check it. To get an overview of open tasks/todos - check the file `TODO.md`. We are open for improvements.


## Author Information

* Markus Rekkenbeil - [(Datadrivers GmbH)](http://www.datadrivers.de "Datadrivers GmbH")
