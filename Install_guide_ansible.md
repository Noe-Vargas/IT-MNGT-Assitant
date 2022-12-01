## Ansible General installation Guide
1. Open up the Virtualbox and start up your CentOS Stream 9 virtual Machine(VM)
2. Access your VM via ssh or directly on the VM console, for my case I accessed it via SSH by typing the VM username and IP-Address, the commands for that is
    > ssh ansman@" ip_address_of_your_VM "
3. Once we have login to our VM; run the following commands to update the system repositories and install the Ansible dependencies (some of these command require root privileges, if that the case, ensure to type "sudo" before applying the command)

  #### 
    sudo dnf update -y
    sudo dnf install epel-release -y
    sudo dnf install pyhton3 -y
    pythong --version (check the pythong version to ensure it was installed)
    sudo dnf install python3-pip
    pip --version (Check the PIP version to ensure it was installed)
    pip install ansible-core
    ansible --version (Check the ansible version to  ensure it was installed)
    
  #### Now that Ansible is intalled we need to add the collection packages that will be used create the playbooks YAML comamnds
    ansible-galaxy collection install ansible.windows   (Collection used to apply Windows updates)
    ansible-galaxy collection install community.windows  (Collection used to push Windows file edits)
    ansible-galaxy collection install cisco.ios           (Collectio used to send commands to Cisco Switches)
    
  #### Lastly we need to install a package to be able to cummunicate  with the windows server, the packes is called "pywinrm"
    sudo pip install pywinrm
