1. We need to organize the directories that will be used with ansible, the directories can be placed in your prefered location, for my project I created a directory on my home folder called my_ansible and in it a created three deirectories to organize the Ububtu and Windows servers public keys, the playbook and the inventory of servers and switches. The commands to create these folder are the following
###
    cd ~              (Move to home directory)
    mkdir my_ansible
    cd my_ansible
    mkdir auth_keys inventory playbooks
    
 2. Now we can copy the downloaded Ubuntu and Windows public kesy to auth_key directory, in case I downloaded it to computer then used the follwing code from my Mac  laptop terminal to tranfer it to my VM
 ###
    sudo scp ~/Downloads/ubu_servkey.pem ansmam@'ip_address_of_the_vm':~/my_ansible/auth_keys
    sudo scp ~/Downloads/WinSrv_KP.pem ansmam@'ip_address_of_the_vm':~/my_ansible/auth_keys
 
 3. Next, move to the inventory directory to create YML file name host, this file will contain the information of the targeted servers and switches thate we are workind with. The host can contain all the servers and swithces sperated with a specifc tag but for my project I decied to put them in three separte host file. That is one gost file for the Ubuntu severs, one for the Windows server and another for the Cisco network switches.
 ###
    cd ~/my_ansible/inventory
    touch hosts_ubu hosts_win hosts_switches
4. Then I used the vim editor to edit and configure the host files, these file must be saved as YAML extension files and the configuratio uses YAML mark up language. Each one of this files contains the IP address of the severs and switches. The AWS cloud assings a random public IP address to the servers each time they are reboooted; therefore, I must edit the the IP addresses on the host files after every reboot of the servers. The same thing applies for the Cisco switches since I connected to my home router and they an IP assinged from the DHCP server. The configuration of the host files is programmed as follow:
###
  * [Hosts_Windows Servers](https://github.com/Noe-Vargas/IT-MNGT-Assitant/blob/001f49df4725b7c3ea72b68a2896ae554361b91c/inventory/hosts_win)
    + The Windows server uses an admin user and the decrypted password is obtained from the pem key in [AWS](https://aws.amazon.com/premiumsupport/knowledge-center/retrieve-windows-admin-password/)
  * [Hosts_Ubuntu Servers](https://github.com/Noe-Vargas/IT-MNGT-Assitant/blob/main/inventory/hosts_ubu)
    + For Ubuntu we will only need the server user and its pem key(the pem key was oabtained from AWS when the instance was created), I used the same pem key for both for both of the Ubuntu servers 
  * [Hosts_Cisco Switches](https://github.com/Noe-Vargas/IT-MNGT-Assitant/blob/main/inventory/hosts_switches)
    + The switches will require it IP addresses and an admin user created in the switch configuration.
 5. Now we navigate back to the Playbook directoy to creae the configuration task we want to apply to the servers and switches. I stareted by creating the files then programmed the tasks using YAML
 ###
    cd ~/my_ansible/playbooks
    touch arc_ubulogs.yml arc_winlogs.yml update_ubusrvs.yml update_winsrv.yml vlans.yml rem_vlans.yml win_ping.yml
 6. The YAML files contains the instructions of that want the severs and switches to do and this can be applied to multiple devices of the same by running a sigle command. The task confifuration of my project can be check in the follwing files
    *  [arc_ubulogs.yml](https://github.com/Noe-Vargas/IT-MNGT-Assitant/blob/main/playbooks/arc_ubulogs.yml) This Playbook archives and removes old log files of a specific directory that are older than 1 day
    *  [arc_winlogs.yml](https://github.com/Noe-Vargas/IT-MNGT-Assitant/blob/main/playbooks/arc_winlogs.yml) This Playbook archives and removes Windows old log files of a specific directory that are older than 1 day
    *  [update_ubusrvs.yml](https://github.com/Noe-Vargas/IT-MNGT-Assitant/blob/main/playbooks/update_ubusrvs.yml) This Playbook installs the latest updated for all the ubuntu servers
    *  [update_winsrv.yml](https://github.com/Noe-Vargas/IT-MNGT-Assitant/blob/main/playbooks/update_winsrv.yml) This Playbook installs Security Windows updates on all the Windows servers
    *  [vlans.yml](https://github.com/Noe-Vargas/IT-MNGT-Assitant/blob/main/playbooks/vlans.yml)  This playbook creates and names 3 diffrent vlans on all the cisco switches
    *  [rem_vlans.yml](https://github.com/Noe-Vargas/IT-MNGT-Assitant/blob/main/playbooks/rem_vlans.yml) This playbook deletes the 3 diffrent vlans on all the cisco switches
    *  [win_ping.yml](https://github.com/Noe-Vargas/IT-MNGT-Assitant/blob/main/playbooks/win_ping.yml) This Playbook is used to test ping the Windows servers
   
7. Before we can run the playbooks we must test the communciation with the server and switches by pinging them, if the server can not be ping we must troublshsoot the errors. The following lines were bused to ping the servers
###
    ansible-playbook playbooks/win_ping -i inventory/hosts_win.yml   This lines used a playbok to ping the Windows servers
    ansible all -m ping -i inventoty/hosts_ubu.yml
    ansible all -m ping -i inventoty/hosts_switches.yml
 8. Once we have confirmed successfull pings we can run the playbook using these commands
 ###
    ansible-playbook playbook/arc_winlogs.yml -i inventory/hosts_win
    ansible-playbook playbook/arc_ubulogs.yml -i inventory/hosts_ubu
    ansible-playbook playbook/vlans.yml -i inventory/hosts_switches
 and so on for the rest of the playbook, we run the same command and subtitute the playbook we desire to run and the correst hosts to apply it to.

 9. Lastly I have attched a Cisco Guide on how to set up its basic configuration, for my project I used two Cisco Catalyst 3560-CG, the switch must be configured before sending any tasks from ansible. I followrd the guide sugestions an set the basic confiuration, the only thing I set up different was the switch IP address; instead of setting a static IP I changed it to DCHP. I used DHCP so that I didn't have to mess with my home router and for presentation purposes but if desired and recomneded to use static IP specailly if the swithc is used in a enterprise company.
    * [Cisco configuration Guide](https://www.cisco.com/c/en/us/td/docs/switches/lan/catalyst3560/software/release/12-1_19_ea1/configuration/guide/3560scg.pdf)
 ###
    To run the playbook on the switches the same command applies
      ansible-playbook playbooks/vlans.yml -i inventory/hosts_switches.yml
      
