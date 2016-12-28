# Introduction

The primary purpose of this playbook is to secure the private network of DigitalOcean Droplets ,IndonesianCloud ,and Azure with tinc VPN. You can use it with any Ubuntu servers 16.04 that can reach each other over a network.

This sets up a tinc VPN between several servers. It also adds /etc/hosts entries for the inventory hostnames to resolve to the VPN IP addresses.

## Prerequisites

This playbook been tested on Ubuntu 16.04

Your local machine (where Ansible is installed) must be able to log in to the remote servers as "root", preferably with passwordless public SSH key, which is specified as the `remote_user` in `/ansible.cfg`. Due to a [bug with the Ansible Synchronize module](https://github.com/ansible/ansible/issues/13825), it is not possible to use a different `remote_user` at this time.

By default, this playbook will bind tinc to the IP address on the `ansible_host`/ public ip. See the "Review Group Variables" section to change this.

## Preparation

### Create Inventory

Create a `/hosts` file with the nodes that you want to include in the VPN:

```
[vpn]
node01 vpn_ip=10.0.0.1 ansible_host=162.243.125.98 ansible_port=ssh_port
node02 vpn_ip=10.0.0.2 ansible_host=162.243.243.235 ansible_port=ssh_port
node03 vpn_ip=10.0.0.3 ansible_host=162.243.249.86 ansible_port=ssh_port
node04 vpn_ip=10.0.0.4 ansible_host=162.243.252.151 ansible_port=ssh_port

[removevpn]


The first line, `[vpn]`, specifies that the host entries directly below it are part of the "vpn" group. Members of this group will have the Tinc mesh VPN configured on them.

- The first column is where you set the inventory name of a host, "node01" in the first line of the example, how Ansible will refer to the host. This value is used to configure Tinc connections, and to generate `/etc/hosts` entries. Do not use hyphens here, as Tinc does not support them in host names
- `vpn_ip` is the IP address that the node will use for the VPN
- `ansible_host` must be set to a value that your ansible machine can reach the node at

This playbook does not support multiple VPNs but it could be easily extended.

### Add New Servers
All servers listed in the the [vpn] group in the hosts file will be part of the VPN. To add new VPN members, simply add the new servers to the [vpn] group then re-run the Playbook:

- ansible-playbook site.yml

####Remove Servers
[vpn]
node01 vpn_ip=10.0.0.1 ansible_host=162.243.125.98 ansible_port=ssh_port
node02 vpn_ip=10.0.0.2 ansible_host=162.243.243.235 ansible_port=ssh_port
node03 vpn_ip=10.0.0.3 ansible_host=162.243.249.86 ansible_port=ssh_port

[removevpn]
node04 vpn_ip=10.0.0.4 ansible_host=162.243.252.151
Then re-run the Playbook:

- ansible-playbook site.yml

####Allow ports
Tinc vpn use ports 655 ( udp & tcp )


Thanks for thisismitch https://github.com/thisismitch/ansible-tinc


