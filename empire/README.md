# Powershell Empire red team scripts
This folder contains scripts that are used to simulate red team activity.

## Setup Powershell Empire
0. Start Kali Linux VM
0. cd /opt
0. git clone https://github.com/EmpireProject/Empire.git
0. cd Empire
0. ./setup/install.sh
0. ./empire --resource listeners.rc

## Post-exploitation
0. agents
0. autoruns autoruns.rc powershell
