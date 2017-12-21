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

## Deploy OSQUery agent on Windows, Linux
0. Browse to https://<Hostname/IP addr of Kolide>
0. Select "Add new host" in top right
0. Select "Reveal secret" and copy the string
0. vim group_vars/agents
    1. set osquery_enroll_secret with string from Kolide

### Deploy Windows OSQuery agent
** Windows hosts must be WinRM ready for Ansible**
0. vim hosts
    1. Add hosts to win_agents
0. ansible-playbook -i hosts deploy_osquery_agent.yml

### Deploy Linux OSQuery agent
0. vim hosts
    1. Add hosts to linux_agents
0. ansible-playbook -i hosts deploy_osquery_agent.yml

### Add OSQuery agents
0. vim hosts
    1. Add hosts to mac_agents
0. ansible-playbook -i hosts deploy_osquery_agent.yml

## Ansible Kolide OS support
* Ubuntu Server 16.04 64-bit
* CentOS 7 64-bit (coming soon)

# Resources/Sources
* https://github.com/kolide/fleet/tree/master/docs
* https://github.com/kolide/fleet/blob/master/docs/infrastructure/adding-hosts-to-fleet.md


To do:
* Set up Linux and Windows deployment
