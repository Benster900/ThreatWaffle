# Ansible code is provided for production environments
## Setup Kolide server
0. cd ansible
0. openssl rand -base64 32
0. Copy the output from above
0. vim group_vars/kolide and set:
    1. kolide_jwt_key to output from above
    1. Set username and password for Kolide, MySQL
    1. Set information for certificate
0. vim group_vars/all and set
    1. timezone
    1. fleet_hostname
    1. base_domain
0. vim hosts
    1. Set management IP address under [kolide]
0. ansible-playbook -i hosts deploy_kolide.yml
0. https://<Hostname/IP addr of Kolide>
    1. Enter username
    1. Enter password

## Setup Windows agents

## Add OSQuery agents


## OS support
* Ubuntu Server 16.04 64-bit
* CentOS 7 64-bit (coming soon)
