# Private server to server network with ansible and wireguard 
 
[![Ansible Role](https://img.shields.io/ansible/role/d/33136)](https://galaxy.ansible.com/maxisme/wireguard_private_networking)
 
This role allowes you to deploy a fast, secure and provider agnostic private network between multiple servers. This is usefull for providers that do not provide you with a private network or if you want to connect servers that are spread over multiple regions and providers.

## How

The role installs [wireguard](https://wireguard.com) on Debian or Ubuntu, creates a mesh between all servers by adding them all as peers and configures the wg-quick systemd service.

## Installation

Installation can be done using [ansible galaxy](https://galaxy.ansible.com/maxisme/wireguard_private_networking):

```
$ ansible-galaxy install maxisme.wireguard_private_networking
```

## Setup

Install this role, assign a `wg_num` variable to every host that should be part of the network and run the role. Plese make sure to allow the VPN port (default is 5888) in your firewall. Here is a small example configuration:

```yaml
# inventory host file

wireguard:
  hosts:
    1.1.1.1:
      wg_num: 10.1.0.1
    2.2.2.2:
      wg_num: 10.1.0.2

```

```yaml
# playbook

- name: Configure wireguard mesh
  hosts: wireguard
  remote_user: root
  roles:
    - maxisme.wireguard_private_networking
  vars:
    wireguard_network_name: "maxnet"
```

add additional peers:
```
vars:
    wireguard_additional_peers:
      - wg_num: 10.2.3.4
        key: your_wireguard_public_key
```
