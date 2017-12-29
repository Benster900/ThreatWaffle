# ThreatWaffle
Threat hunting repo for my independent study on threat hunting with OSQuery.

# Recommended security options not implemented
* Enable MySQL TLS
* Enable Redis password auth
* Restrict MySQL operations
* Restrict Redis operations

# Ansible code is provided for production environments
## Setup Doorman server
0. openssl rand -base64 32
0. Copy the output from above
0. vim group_vars/doorman and set:
    1. doorman_secret_key to output from above
    1. Set username and password for postgresql, doorman_webgui
0. vim group_vars/all and set
    1. timezone
    1. doorman_hostname
    1. base_domain
0. vim hosts
    1. Set management IP address under [doorman]
0. ansible-playbook -i hosts deploy_doorman.yml
0. https://<Hostname/IP addr of Doorman>
    1. Enter username
    1. Enter password

## Setup Graylog server
0. mv group_vars/graylog.example group_vars/graylog
0. vim group_vars/graylog set graylog_admin_password
    1. graylog_admin_password can not contain special characters: (,),;
0. vim hosts set ip addr for [graylog]
0. ansible-playbook -i hosts deploy_graylog.yml

## Setup Windows Server 2016 domain controller
0. mv group_vars/win_dc.example group_vars/win_dc
0. vim group_vars/win_dc set ad_domain_name(by default set base_domain) and ad_safe_mode_password
0. vim group_vars/windows set ansible_user and ansible_password
0. vim hosts set ip addr for [win_dc]
0. ansible-playbook -i hosts deploy_windows_dc.yml

## Setup Windows 10 clients/agents
0.


## Deploy OSQuery agents
### Deploy OSQuery agent on Windows, Linux
0. Browse to https://<Hostname/IP addr of Kolide>
0. Select "Add new host" in top right
0. Select "Reveal secret" and copy the string
0. vim group_vars/agents
    1. set osquery_enroll_secret with string from Kolide

### Deploy Windows OSQuery agent
** Windows hosts must be WinRM ready for Ansible**
0. Copy contents of /etc/nginx/ssl/kolide.crt
0. mv conf/agents/certificate.example conf/agents/certificate.crt
0. mv group_vars/win_agents.example group_vars/win_agents
0. vim group_vars/win_agents and set:
    1. ansible_user
    1. ansible_password
0. vim hosts
    1. Add hosts to win_agents
0. ansible-playbook -i hosts deploy_windows_osquery_agents.yml

### Deploy Linux OSQuery agent
0. vim hosts
    1. Add hosts to linux_agents
0.ansible-playbook -i hosts deploy_linux_osquery_agents.yml

### Deploy Mac OSX OSQuery agents
Coming soon

## Ansible Kolide OS support
* Ubuntu Server 16.04 64-bit
* CentOS 7 64-bit (coming soon)

# Resources/Sources
* https://github.com/kolide/fleet/tree/master/docs
* https://github.com/kolide/fleet/blob/master/docs/infrastructure/adding-hosts-to-fleet.md


To do:
* Set up Mac OSX deployment
* Docker setup
* Add DNS setup to DC
* Get Win DNS ip addr from hosts file
