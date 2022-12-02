# IT-Management Assistant
  > by [Noe Vargas](https://github.com/Noe-Vargas) - IT/CS-499 Class of Fall 2022

<p align="center">
  <kbd><img src="https://user-images.githubusercontent.com/119148009/204467370-37215f94-2e14-4f18-8789-780be3660254.png" alt="Title Picture2" width="740"/></kbd>
</p>

The IT-Management Assistant is an undergrad Capstone IT project at California State University Channel Island(CSUCI) performed by Noe Vargas. The project is Linux based system that uses the Ansible platform to automate maintenance tasks on multiple devices. The IT Management Assistant is intended for IT system server administrators to facilitate some of the most time-consuming maintenance tasks on multiple servers and Cisco network-managed switches.
## Overview
This project focuses on setting up the Ansible system on Centos Stream 9, standing up four servers in AWS cloud services, configuring the Windows servers for Ansible accessibility, and creating the maintenance tasks on YML files called Playbooks and applying them to the servers and network switches. This project focuses on automating two main tasks for all the OS systems; the first tackles installing server updates, and the second on cleaning up log files from the servers. The last part of the project physically adds two Cisco network-managed switches to the Ansible platform and creates a Playbook to configure network VLAN on both switches.
## Experience Gained ##
* Cloud Computing (AWS)
  *  Windows 2016 Print Server
  *  Windows 2022 Active Directory Server
  *  Ubuntu 2016 Samba File Server 
  *  Ubuntu 2022 Web server
  
+ Virtual Machine build and configuration
  + CentOS Stream 9 
  + Ansible 2.13
  
- Ansible
  - YAML Language Configuration
  - Python
  - Terminal bash commands
  - SSH and Windows Remote services commands
  
 + Cisco Network Switches
    - Basic switch configuration
      - Cisco Switch commands
 + Troubleshooting AWS server connections
 + Networking setup for Linux servers
 + Firewall setup for SSH and Winrm and ports configurations
 + Github and Wordpress 
## Prerequisites
  - A desktop or laptop computer
    - Hardware requirements may vary at least a multicore processor
    - Minimum 8GB of RAM
    - 8 GB of free Space for virtual machine
  - [VirtualBox latest version](https://www.virtualbox.org/wiki/Downloads)
  - A modern Web browser
  - Access to AWS cloud (CSUCI provided access)
  - Cisco Managed Network Switches, used in this project Cisco Catalyst 3560-CG
  - [CentOS Stream 9](https://www.centos.org/centos-stream/#tab-3) Image
 
## Server Installations and General Instructions 
  + General installation process of VirtualBox on [MacBook Pro Intel Chip](https://www.youtube.com/watch?v=lEvM-No4eQo)
  + General installation process for [Centos Stream 9](https://www.linuxcnf.com/2022/01/how-to-install-centos-stream-9-minimal.html)
  + General AWS Windows Server [instance setup](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/EC2_GetStarted.html) and accessing it via RDP
  + General AWS Ubuntu Server [instance setup](https://mobisoftinfotech.com/resources/mguide/launch-aws-ec2-server-set-ubuntu-16-04/) and accessing it via SSH
## AWS servers configuration
  - Windows Server 2022 Active Directory [setup](https://cloudinfrastructureservices.co.uk/setup-active-directory-domain-on-windows-server-2022/)
    - After setting up the AD role, you must go to the user profiles settings to change the administrator or admin user password to never expire. This will prevent connection problems with the Windows EC2 instance when the system ask to update passwords.
  - Windows 2022 print server [setup](https://www.tactig.com/install-configure-print-services-windows-server/)
    - Administrator password also needs to be set to never expire to prevent EC2 instances connections problems.
  - Ubuntu 2022 Web server [setup](https://www.digitalocean.com/community/tutorials/how-to-install-the-apache-web-server-on-ubuntu-22-04)
  - Ubuntu 2016 Samba file server [setup](https://ubuntu.com/server/docs/samba-file-server)
 
## Ansible Installation and configuration 
  | Dependencies  |               |               |              |
  | ------------- | ------------- | ------------- |------------- |
  | Epel-release  |    Python3    |     PIP       |   Pywinrm    |

#### Ansible Install
  - Click here for [General Instructions Guide](https://github.com/Noe-Vargas/IT-MNGT-Assitant/blob/main/Install_guide_ansible.md)
  - Ansible can be install in different ways with other dependencies packages and script but for this project I installed it as described in the Instruction Guide

#### Pywinrm install on the Windows Servers (This allows communication between Ansible and the Windows Servers)
    (This allows communication between the windows servers and the Ansible platform)
    Login to the Windows servers, Open Power-Shell by right clicking on it to run as an administrator, and run this code
        [Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12
        $url = "https://raw.githubusercontent.com/jborean93/ansible-windows/master/scripts/Install-WMF3Hotfix.ps1"
        $file = "$env:temp\Install-WMF3Hotfix.ps1"

        (New-Object -TypeName System.Net.WebClient).DownloadFile($url, $file)
        powershell.exe -ExecutionPolicy ByPass -File $file -Verbose
  The Winrm set up assumes you have PowerShell v3.0 or above, for other version follow the [ansible website recommendations](https://docs.ansible.com/ansible/latest/os_guide/windows_setup.html)

## Ansible Playbooks, Inventory, Programming
Finally after completing the main Ansible setup we can start creating playbooks to control multiple servers and other devices
Please [click here](https://github.com/Noe-Vargas/IT-MNGT-Assitant/blob/main/ansible_todo.md) to find the step to perform the following:
  * Creating directories for Playbooks, Inventory, and authorization keys
  * YAML file for servers and switches inventory
  * YAML files for Playbooks: Windows and Ubuntu system updates, and cleaning log files 
  * Ping command to check servers communication
  * Commands to run playbooks
  * Basic Cisco managed network switches configuration
  * Run playbook to create and remove VLANS from switches 

## Results 
1. Results of pinging the the Windows servers and Ubuntu servers:
  * [Ping_Results](https://user-images.githubusercontent.com/119148009/205192585-b6727283-0f11-4672-a7c1-6ebe4ff1569a.png)
3. Results from updating The Ubuntu servers: 
  * [Update_Results](https://user-images.githubusercontent.com/119148009/205192785-3db5d47c-5286-4a1b-a1a2-6c1c7a197d09.png)
5. Results from updating the Windows Severs (the task perfomed successfully but AWS keeps these server udpated therefore no missing upates were found to apply)
  * [Results_clip](https://user-images.githubusercontent.com/119148009/205193243-316857a0-c28c-4813-af05-9076ccc80d38.mov)
6. Results from cleaning up the log files older than 1 day from Windows servers 
  * [Results_Part1](https://user-images.githubusercontent.com/119148009/205201300-f49f48db-8f9e-4ca0-aacc-d633c3ee5ba9.png),  [Results_Part2](https://user-images.githubusercontent.com/119148009/205201510-83fc7ec0-3306-4dff-9416-ef1353f443ef.png)
8. Results from cleaning up the log files older than 1 day from Ubuntu servers 
  * [Results](https://user-images.githubusercontent.com/119148009/205203739-aed37dc6-9fd9-4473-8cd8-538e4ed844ef.png)
10. Results from adding and removing VLAN to the Cisco Switches
  * [Add-VLANS_part1](https://user-images.githubusercontent.com/119148009/205204452-8d9e3de8-91a0-4702-bc18-58f5c2b11336.png),  [Add-VLANS_part2](https://user-images.githubusercontent.com/119148009/205204621-48b6a370-870d-4898-b211-fdc1c4bf072a.png),  [Removed-VLANS_part1](https://user-images.githubusercontent.com/119148009/205204958-f63a582a-4963-4bb4-a7ef-b72c0c65de7a.png),  [Removed-VLANS_part2](https://user-images.githubusercontent.com/119148009/205205350-44b77fba-eb62-462c-b189-fe006d1b75d6.png)

## For future students looking to expand this project
  * Add security connection for Windows Severs to use encryption on the host file instead of plain passwords
  * Create Playbook that can send alert for hardware failures or system booting errors
  * Create Playboo to control other brands of Managed Network Switches such as Juniper to Brocade switches
  * Add more playbooks that are time-consuming for IT Admins. 

## Acknowledgments
* Professor Jasson Issacs (CSUCI Professor  & Capston Advisor)
* Kevin Scrivnor (CSUCI IT/CS Professor)
* Clark Devlin (HESD Director of Technology)

