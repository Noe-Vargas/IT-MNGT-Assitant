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
