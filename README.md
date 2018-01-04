# ThreatWaffle
Threat hunting repo for my independent study on threat hunting with OSQuery.

# Doorman or Kolide
This repo contains two branches which are Doorman and Kolide. Both are open source OSQuery fleet managers. Koide is used in the master branch.

# Setup Windows Domain Controller
## Setup domain
0. mv group_vars/all.example group_vars/all
0. Vim group_vars/all and set
    1. base_domain  
0. mv group_vars/win_dc.example group_vars/win_dc
0. vim group_vars/win_dc and set:
    1. ad_domain_name(by default set base_domain)
    1. ad_safe_mode_password
0. vim group_vars/windows and set :
    1. asnsible_user
    1. ansible_password
0. vim hosts set:
    1. ip addr for [win_dc]
0. ansible-playbook -i hosts deploy_windows_dc.yml
0. Shutdown and create snapshot

## Setup domain user(non-admin)
0. Login into Windows DC
0. Open server manager
0. Select “Tools” then “Active Directory Users and Computers”
0. Select “Users” on the left
0. Select “Acton” then “New User”
    1. User info
        2. Enter “Sherlock” for Firstname
        2. Enter “Holmes” for Last Name
        2. Enter “Sholmes” for logon
    1. Password
        2. Enter a password for user
        2. UNcheck “User must change password at next logon”
0. Shutdown and create snapshot

## Group policy settings
### Enable RDP through firewall

### Powershell script block logging

### Process creation logging

### SMB access via firewall

# Setup Kolide OSQuery fleet manager
0. openssl rand -base64 32
0. Copy the output from above
0. vim group_vars/kolide and set:
    1. kolide_jwt_key to output from above
    1. Set username and password for Kolide, MySQL
0. vim group_vars/all and set
    1. timezone
    1. fleet_hostname
    1. base_domain
    1. Set information for certificate
    1. Slack (optional)
0. vim hosts
    1. Set management IP address under [kolide]
0. ansible-playbook -i hosts deploy_kolide.yml -u <ubuntu local user>
0. https://<Hostname/IP addr of Kolide>
    1. Setup through Kolide setup

# Setup Graylog server
0. vim group_vars/graylog and set
    1. graylog_admin_password
0. vim hosts and set [graylog]
0. ansible-playbook -i hosts deploy_graylog.yml -u superadmin

## Deploy OSQuery agents
## Initial setup
0. Browse to https://<Hostname/IP addr of Kolide>
0. Select "Add new host" in top right
0. Select "Reveal secret" and copy the string
0. vim group_vars/agents
    1. set osquery_enroll_secret with string from Kolide

## Deploy Windows OSQuery agent
** Windows hosts must be WinRM ready for Ansible **
0. Copy contents of /etc/nginx/ssl/kolide.crt for Kolide server
0. mv conf/agents/certificate.example conf/agents/certificate.crt
0. vim conf/agents/certificate.crt and paste the contents from above
0. mv group_vars/win_agents.example group_vars/win_agents
0. vim group_vars/win_agents and set:
    1. ansible_user
    1. ansible_password
0. vim hosts
    1. Add hosts to win_agents
0. ansible-playbook -i hosts deploy_windows_osquery_agents.yml

## Deploy Linux OSQuery agent
0. vim hosts
    1. Add hosts to linux_agents
0.ansible-playbook -i hosts deploy_linux_osquery_agents.yml

## Deploy Mac OSX OSQuery agents
Coming soon

## Ansible Kolide OS support
* Ubuntu Server 16.04 64-bit

# Resources/Sources
* https://github.com/kolide/fleet/tree/master/docs
* https://github.com/kolide/fleet/blob/master/docs/infrastructure/adding-hosts-to-fleet.md


To do:
* Set up Mac OSX deployment
* Docker setup
* Add DNS setup to DC
* Setup log OSQuery agent logging
* Forward OSQuery logs to Graylog
