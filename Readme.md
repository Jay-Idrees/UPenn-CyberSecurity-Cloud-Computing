# Cloud Security Configuration

> Benifits

- **Ground-up security**: From a security perspective, the cloud presents an opportunity to build a secure system from the beginning, as opposed to trying to implement new security measures on old systems.

- **Easy configuration**: Instead of having to learn the many different tools that will be included on a network, an administrator can use the cloud service provider's website portal or command line to create all their needed items.

- **Quick turnaround**: Compromised and insecure machines can be discarded and replaced quickly, at no additional cost to the organization.

- **Personalized networks from cloud providers:** Software and configuration-as-code allows cloud providers to deploy the specific network that engineers need for their circumstances. Security specialists can define the secure network they need, and the cloud provider can create and test the deployments, making the networks more dependable.

- **High availability and fault tolerance**: Without their own physical data centers, engineers can focus on deploying their machines in multiple places, and the provider can maintain the data center. This way, cloud networks are more robust against power outages, DoS attacks and other threats, as long as they are configured correctly.

- **Easy implementation**: Security controls can be implemented more easily, because they require only modifications to software-defined networking patterns. No complicated rearrangements of physical wiring are necessary.

- **Affordability**: Organizations can use powerful machines that they would not be able to afford if they had to purchase and maintain them themselves. For example, GPU processing units, which are very expensive to buy and very expensive to lose to an attacker.


## Cloud Services

- **IaaS (Infrastructure as a Service)**: Storage, networking, servers, basic access management and other provider-enforced security controls

- **PaaS (Platform as a Service)**: Cloud environment for building an delivering applications

- **SaaS (Software as a Service)**: Secure User applications through internet- Allows you to give access to users to certain applications

- **DaaS/DBaaS (Data as a Service/Database as a Service)**: Allows company's data to a user

- **CaaS (Communications as a Service)**: Instant Messaging (IM), collaboration and video conferencing, Internet telephony (VoIP)

- **XaaS (Anything as a Service)**: Any combination of service

- **Modern Technologies**: 
- `Containers` - A small scale VM in Mbs- affordable
- `Provisioners` - Tool to configure VMs, main use configuring thousands of machines once with same settings - Main benifit is to eliminate human error
- `Infrastructure as code` Also called **IaC** files
- These files capture the description of the entire designed network in code
                 This is the code used by Provisioners for configuration or re-configuration of VMs
- `Continuous Integration/Continuous deployment` Also called **CI/CD** Systems
- Allows you to update Network by updating the IaC files. 
- The Glue that binds all the things together
- Allows automated configuration and infrastructure management
- Pen testers actually perform the security testing

## Setting up a firewall for virtual machineS
- Can set rules on multiple ports
**Network Security group (NSG)** to create an NSG that blocks all traffic to and from the network
- Thhis model can allow creating a template NSG which can be modified for future VNs
- Allow creation of NSGs for different traffic profile
- A security group is a unique network into which you can deploy machines
- We can use this to deploy a remote desktop on a specific port
- Before setting up devices in the cloud the budget limits and the cost-control policies should be defined

**AVailability Set** <br>
**Availability Zone**

Aure limits free tire users to only 4v CPUs per region

## IaaC (Infractructure as code)

- We can code such that a VM is a server and has many containers. `Provisioners` are used for 

- Using code to configure the network and keeping track of changes to configuration. The changes are also reversible

- Servers can send data to a central database, thus the servers are only composed of a small text files of code. We can even send the logs to a database

- It also makes it easy to turn back an update if there are errors with new updates 
- **Provisioners** are used for automation (making automated changes to configuration) - bring servers to a **state of operation**

- Once once server has reached that **state of operation**, this can be applied to multiple other servers

- **provisioners** ansible, puppet and chef




## IaaS (Infrastructure Service)

1. **Resource Goup** Creating this is the first step so you can better organize resourses-logical grouping of resources. 
- It consists of:
        - `VNet` or Virtual network - A virtual network is a collection of virtual machines and related virtual tools to substitute for normal setup - easy to re-configure - the organizaion is usually task oriented. The VMs will have their own software version of vNICs(Network Interface Cards), ip addresses, subnets- the subnets are an independent resource that can be configured separately. 
        - firewall
        - virtual computers
- Many resources can be created independently and then attached later to an independent network

 2. **Virtual Network**       

- Only after a virtual network is created can the servers and the services be deployed

- Prices vary by region - Be cognizent of that

- We can create an initial large network and then sub-net inside of it. 

                ```
                10.0.0.0/16 for the large network.
                10.10.1.0/24 for the first subnet.

                or simply 192.168.1.0/24
                ```

- Azure Configurations are safe and secure and are automatically done- They are difficult to hack

- Cloud provider also give logging capabilities

- The resource group we created has been added to the virtual network

3. **Network Security Groups** - Adding a firewall
- The basic firewall is called `network security group (NSG)`
- Initially create a firewall and then attach it to a virtual network
- First create an NSG that blocks all traffic to and from the network (Including remote desktop or RDP and VNC traffic)
- This NSG is then be cloned and used as a template, and can be modified and added to a network

- There are 3 main rules 1) Allows the VMs in virtual network to communicate with eachother, 2) Allows the loadbalancer to route traffic and 3) Block all other traffic. 

> Adding a rule for inbound/outbound

- Source: can be a sigle IP, range, application security group, service tag
- Source IP addresses/CDIR ranges: Speify the ip address or range
- Source port ranges: * typically as the source ports are typically random
- Deatination: To virtual network or a specific computer
- Destination: We will specify as 3389 as this is the port that is usually assigned to 


4. **Configuring a `virtual machine` power**

> Components of a VM

- Ram, Storage
- Disks are either OS or Data disks (VM images,Media, Text, Forensic disk images) can be HDD or SSD

- Create Jump-Box

- Create 2 more new VMs with the following properties:
        1. Each VM should be named "Web-1" and "Web-2"
        2. Each VM must be created in the Red Team resource group.
        3. Each VM must be located in the same region as your resource and security groups.
        4. You should use the same admin name for all 3 machines.
        5. Make sure to take a note of this username, as you will need it to login later.




5. **Setting up a Jump Box, Connecting to a VM using SSH** This is the same as **Configuring a VPN - Virtual Private Network**

 - **Gateway router** or **Jump-box** between the VMs on a VNet forces all traffic through a single node `fanning inn`

- Notice the difference b/w `secure architecure` and `secure configuration`. Configuration refers to security rules or the intrusion detection of the VMs or network while the configuration refers to tolerance and redundency - how effective is the network in containng the effects of the breach. For example the use of ssh keys to access VMs is a configuration measure while Using a gateway router (jump-box) before the network is a confguration - this dramatically reduces the attack surface area 

- In the inbound security rules, consider the following. 
Source: Use the IP Addresses setting, with your IP address in the field.

- `Source port ranges`: Set to Any or * here.

- `Destination`: This can be set VirtualNetwork but a better setting is to specify the internal IP of your jump box to really limit this traffic.

- `Destination port ranges`: Since we only want to allow SSH, designate port 22.

- `Protocol`: Set to Any or TCP.

- `Action`: Set to Allow traffic.

- `Priority`: This must be a lower number than your rule to deny all traffic, i.e., less than 4,096.

- `Name`: Name this rule anything you like, but it should describe the rule. For example: SSH.

- `Description`: Write a short description similar to: "Allow SSH from my IP."

- Connect SSH using `ssh admin-username@VM-public-IP` This is done using gitbash


6. **Giving Jump-Box access to the VNet** - Done in Azure

- Get the private IP address of your jump box.

    - Go to your security group settings and create an inbound rule. Create rules allowing SSH connections from your IP address.

       - Source: Use the **IP Addresses** setting with your jump box's internal IP address in the field.

        - Source port ranges: **Any** or * can be listed here.

        - Destination: Set to **VirtualNetwork**.

        - Destination port ranges: Only allow SSH. So, only port `22`.

        - Protocol: Set to **Any** or **TCP**.

        - Action: Set to **Allow** traffic from your jump box.

        - Priority: Priority must be a lower number than your rule to deny all traffic.

        - Name: Name this rule anything you like, but it should describe the rule. For example: `SSH from Jump Box`.

        - Description: Write a short description similar to: "Allow SSH from the jump box IP."


7. **Docker, Container and Provisioners in the Jump-Box** - Low scale VMs - (Because they have no OS or hardware of their own)- Easily duplicated

- Some basic navigational commands. Note that the containers are setup using the docker (See below), that will need to be installed before the container can be installed. One of the container that we install is `Asible` Which helps with automation of the containers on large scale by arranging the VMs into the groups.

- Steps go like this. Connect to Jump Box, install docker, install container, open the container using the name of the container, run the terminal in the container, install ansible, obtain SSH key, Update the Azure SSH key, Make updates to the host file in Asible for automation. Configure a load balancer and then configure firewall. Then you can decide if you can want to make additional VMs

- `sudo docker container list -a` to see the name of your container
- `sudo docker ps` to view running containers
- `docker attach container_name` to open shell
- `cd /etc/ansible` `ls` To view fies

- Typically, you design a VM and then create containers inside it (e-g 100 containers), containers can also be automatically created. 

- The containers can be linked. There can be a database container, applicattion containers. Goal is to make a machine as stateless as possible. If the containers are task oriented rather than user oriented, then it makes easy to create and destroy containers that have not been changed. 

- `Horizontal scaling` Creating more containers to handle additional load. (preferred on cloud as its more flexble)
- `Vertical scaling` is when you add more RAM or CPU to the verticle scaling. 

- Additional containers can be created for to manage load as the number of users expand

- LAMP web server, you will need to: install Linux, a web server like Apache, a database like MYSQL, and a back-end codebase like PHP. This VM can be copied if another LAMP server is needed

- We could create a LAMP container image which is downloaded as an app, the container is cerated from the image that was downloaded - The image can be easily downloaded from computer to computer

- Containers can be attacked - and easily replaced w/o damage to the rest of the network, if they have not been changed- thats where the task oriented containers come into play. As you can have database containers that store only the data- the rest of the containers can be deleted or replaced if compromised. 

- The Purpose is to share common resources and create a new image of only what is different- Thats the purpose of the container

- The main different between the multiple containers and the multiple VMs which are copies of the same original image that the containers can share some files or be connected to the same resources, while the VMs are completely independent. Both the VMs and the conainers can share the same host (Virtual network).



8. **Docker** - Program to make and manage custom containers

- Makes it easy to install complex server configurations

- You can download containers created by other users- has a container hub

- Connect to Jump-box `ssh admin@jump-box-ip`

- Install Docker `sudo apt install docker.io` install docker and then verify `sudo systemctl status docker`, `sudo systemctl start docker` to start the program if not running

- List all the containers that are currently installed

- `docker container list -a` and `sudo docker ps` for getting the name of the containers and to list the containers that are actively running. The names of the containers are radomly generated after the names of the famous developers- these names are for your specific container that was downloaded and installed

> EXAMPLE: Installing bionic container

- Install container `sudo docker pull cyberxsecurity/ubuntu:bionic` for interpretation: `[image_maker]/[image_name]`

- Run container for the second time `sudo docker run -ti bionic/ubuntu bash`, `ti` is terminal interactive- allows to run a terminal on the container, `bash` or `bin/bash` gives shell control of the container. Note that if this is the first time that you are running the container then you will have to use `start` instead of run for example: `sudo docker start container_name`

- `sudo docker attach container_name` - opens shell/terminal inside the container, This is going one layer deeper from VM to container. Note that Jump-box itself is a VM -that is connected to the network

> Likewise installing ansible provisioner so we can automate tasks with YAML scripts

- `sudo docker pull cyberxsecurity/ansible` and switch to root user `sudo su`

- `docker run -ti cyberxsecurity/ansible:latest bash` or  `docker run -it cyberxsecurity/ansible /bin/bash`  and then `exit` to leave. Selecting bash opens terminal and give you an opportunity to create a key in the container



9. **Configuring Ansible (Provisioner) - specifically the `host` file and the `ansible.cfg`**

- `ssh-keygen` inside the container and then `ls .sh/` to view  the keys files 

- `cat .ssh/id_rsa.pub` to view the key

- Now change the SSH key on the virtual machine for the new SSH user in Azure. Note that if only TCP connections are establishes for SSH in the security group, it will not be possible to use `ping`
- `ping ipaddress` to test that the ssh key is working. Then `exit`

- `nano /etc/ansible/hosts` Add IPs and websites in the hosts file. 
- `nano /etc/ansible/ansible.cfg` Add a new user to the cfg file next to `remote_user` See below:
- `ansible_python_interpreter=/usr/bin/python3` - Testing the Ansible connection - Notice how this code line is added in the hosts file next to the ip- this is for testing

- Testing : `ansible webservers -m ping` or `ansible all -m ping`

- hosts file
- The machines can be grouped `[webservers]` or `[databases]` or `[workstations]`
    ```bash
        # This is the default ansible 'hosts' file.
        #
        # It should live in /etc/ansible/hosts
        #
        #   - Comments begin with the '#' character
        #   - Blank lines are ignored
        #   - Groups of hosts are delimited by [header] elements
        #   - You can enter hostnames or ip addresses
        #   - A hostname/ip can be a member of multiple groups
        # Ex 1: Ungrouped hosts, specify before any group headers.

        ## green.example.com
        ## blue.example.com
        ## 192.168.100.1
        ## 192.168.100.10

        # Ex 2: A collection of hosts belonging to the 'webservers' group

        [webservers] This is where the new ips are added
        ## alpha.example.org
        ## beta.example.org
        ## 192.168.1.100
        ## 192.168.1.110
        10.0.0.6 ansible_python_interpreter=/usr/bin/python3
				10.0.0.7 ansible_python_interpreter=/usr/bin/python3
        ```

- `ansible.cfg`
    ```bash
    # What flags to pass to sudo
    # WARNING: leaving out the defaults might create unexpected behaviours
    #sudo_flags = -H -S -n

    # SSH timeout
    #timeout = 10

    # default user to use for playbooks if user is not specified
    # (/usr/bin/ansible will use current user as default)
    remote_user = sysadmin

    # logging is off by default unless this path is defined
    # if so defined, consider logrotate
    #log_path = /var/log/ansible.log

    # default module name for /usr/bin/ansible
    #module_name = command

    ```

There are Ansible modules for almost anything we can think of. For example:
- Create files and folders.
- Start, stop, and download Docker containers.
- Change system settings.
- Download code from Github.
- Create compressed archives of files.

- **Summary of Ansible Configuration**

1. `docker container list -a`
2. `docker start [container_name]`
3. `docker attach [container_name]` Now we are inside the container
4. `nano /etc/ansible/pentest.yml`
5. Paste the code below in the pentest.yml file and run `ansible-playbook /etc/ansible/pentest.yml`
6. `ssh sysadmin@10.0.0.6` ssh to the container
7. `curl localhost/setup.php` to test the connection

 Your final playbook should read similar to:

    ```YAML
    ---
    - name: Config Web VM with Docker
      hosts: webservers                     // all the tasks will be run on all the VMs listed under webservers
      become: true
      tasks:                               // Notice all the tasks listed below for Ansible to run
      - name: docker.io
        apt:
          force_apt_get: yes
          update_cache: yes               // This will run apt update - required for installing docker.io
          name: docker.io                // This will install docker.io
          state: present

      - name: Install pip3
        apt:
          force_apt_get: yes
          name: python3-pip               // This will install python3-php- package manager for python
          state: present

      - name: Install Docker python module
        pip:
          name: docker               // It lets Ansible control containers by controling docker
          state: present

      - name: download and launch a docker web container
        docker_container:
          name: dvwa                 // This is installing the dvwa(dam vulnerable web app)
          image: cyberxsecurity/dvwa
          state: started
          restart_policy: always     // Instructs container to restart automatically when the VM restarts
          published_ports: 80:80        // publish port `80` on the container to port `80` on the host

      - name: Enable docker service
        systemd:                        // Restarts docker when the machine reboots
          name: docker
          enabled: yes
    ```

10. **Load Balancer** 

- Three step process

        1. Add Load balancer using +Add to the resource group
        2. Add and Configure healthprob to the load balancer
        3. Create a backend pool and add Vms so the VMs are behind the loadbalancer's external IP
        4. You can decide how many loadbalancers you can use

- **backend pool** is the servers that the load balancer is connected to and can transfer the traffic

- Provides an external IP for the website to be accessed over the internet and points that ip to a VM. The name of this IP should be the most uniquie in all of Azure.

- Distributes traffic over multiple servers. More servers can be added to the group as traffic increases

- Helps mitigating DoS attacks by having multiple servers running the same website behind a load balancer- so the traffic can go to the other server which is not hacked 

- THe load balancer can perform a **health probe** to check whether a server is fully operational, before delegating the resoursces to it. 

11. **Configuring  firewall**

- Create a load balancing rule 

- This can be done by opening load balnacer and then adding **load balancing rules**

- You have to define rule such that only port `80` is exposed ot the internet

- Create a new security group rule to allow port 80 to your internal VNet and remove the security group rule that blocks all traffic

- Now run the ip on a web-browser to see that you can get to DVWA


12. **Redundancy**

- Create a new VM and add it behind a load balancer-Redundancy is have an exact copy of something- in this case DVWA server

- See activity- good. 

## Key terminal commands

1. `ssh admin@jump.box.IP` To connect to the jump box
2. `sudo docker container list -a` To check the list of all the containers currently installed 
3. `sudo docker start container_name` Run the latest version of the container that has been previously installed (`run` installs the first time and `start` installs the second time)
4. `sudo docker ps` Check how many containers are running actively right now

5. Now you are inside the container, to open up a shell `docker attach container_name`
6. `cd /etc/ansible` and `ls` Ansible is a **Provisioner** 
7. Ansible reads YAML files and can execute tasks
8. `nano my-playbook.yml`
9. `ansible-playbook my-playbook.yml` To run the yml script

- At this point, we have created a virtual network, deployed a jump box running an Ansible Docker container, and used that container to configure another VM running a DVWA container.

## Sample YAML-YAML ain't markup language 

- These are used by Asible or similar provisioners for automation

- `apt`, found at [docs.ansible.com/ansible/latest/modules/apt_module.html](https://docs.ansible.com/ansible/latest/modules/apt_module.html).
- `pip`, found at [docs.ansible.com/ansible/latest/modules/pip_module.html](https://docs.ansible.com/ansible/latest/modules/pip_module.html).
- `docker-container`, found at [docs.ansible.com/ansible/latest/modules/docker_container_module.html](https://docs.ansible.com/ansible/latest/modules/docker_container_module.html).



```YAML
---
  - name: My first playbook
    hosts: webservers
    become: true
    tasks:

    - name: Install apache httpd  (state=present is optional)
      apt:
        name: apache2
        state: present
```
- '---' specifies that the file is YAML
- `become:true` indicates to run as root
- `tasks` Whatever is listed under tasks will be run- In this example I am suggesting to install the apache2 software as root 
- `state:present` IMP, Ansible will check to see if the package is already installed, if so then wont reinstall, but if the software is not installed then it will install it. Alternatively if `state: absent` then it will check to see whether the software is installed. If it is then it will delete and re-install

- Apache2 is a program to install webserver



## Network Redundency -Network Design

- If one server falls the network does not collapse. 
- If authentication server goes down employees wont be able to log on

- Using two gateways will offer redundency. If one gateway is down, then the second one will be on- the downside with this approach is that it requires 2 machines to secure and 2 targets for hackers
- Similarly adding multiple databases also increases the redundancy but increases **Attack Surface**
- We can also add a `SIEM - Securty infromation and event management` before the log database. These perform realtime analysis of the log and other services of inrusion detection


## Overall steps in Azure for configuring a secure cloud network for an organization

1. Create a **resource group** (Red-Team)

2. Create a **VNet**(RedTeamNet) and link it to the resource group(RedTeam)
     - Specidy the IP address and subnet and disable the security options if you do not want to pay extra

3. Setup a **network security group** (RedTeam-SG) - Add inboud security rule to block all traffic (deny)

**4.** Add a new inbound **security rule** to configure the network security group just created - Block all traffic- This is the default deny rule that should get the highest priority (4096) - This is the default deny- Everything is blocked. We can also block all ports by specifying range like `0-65535`

5. **At this point I have a VNet protected by the security group that blocks all traffic. No virtual machine is added yet, but the Goal is to create 3 machines in the same resource group**

6. Set up cryptographic **SSH keys-First** to access cloud servers, Passwords are weak because they can be brute forced. This is called `ground up security`. key commands: `ssh-keygen`, Do not enter any password and press enter twice,`cat ~/.ssh/id_rsa.pub`- to view the key. I did this on my laptop 

7. So far I only have a VNet-no VM, Create a new Virtual Machine in Azure using +Add: Specify the resource group(Red-Team), Call this first VM **Jump-Box-Provisioner VM**. Be consistent with the region selected. I am on track to create 3 VMs - imp-these should share the same resource group and the region and the security group. The first VM is called Jump-box the other 2 Web-1 and Wb-2

8. **Jump-Box-Provisioner VM settings**: Image: Ubuntu Server 18.04, Standard-B1s 1CPU and 1RAM (Jump-Box needs only 1 ram). Create a `username (RedAdmin)` and associate the SSH public key with it. Paste the public SSH key. I am not using password. Allow `SSH(22)` port for inbound -This is important as here I am allowing that an external ip can connect to the Jump-Box-Provisioner via the port 22. use advanced option for security group under the networking tab for custom configuration of the security group. You can also have the same username for all 3 machines

9. Create **2 more VMs (Web-1, Web-2)**. Same Resource group, Same Region, Same Network Security Group. For the time being I will be using the same SSH key,later these will be replaced. For specification select "Standard-B1ms". Free Azure account only allows 4 virtual machines. The new VMs should have the **same availability set**. This is important so all the machines can be added to the load balancer later- Use the same availability set name. Select public IPs for these virtual machines as none. 

**10.** Create **Security group rule-Allow IP** to allow SSH connection to my current IP address. I can get my current IP from the [whatsmyi.org](whatsmyip.org). This is done by selecting `inbound security rule`. Paste the ip under source IP addresses. Call this rule **SSH**. Note that the port is asigned to `22` Here I am configuring that all IPs should be blocked as previously configured, but this particular IP xx.xx.xx.xx should be allowed to connect to the VNet via the port 22. 

11. **Establish SSH Connection with Jump-Box-Provisioner VM** Then afer saving, test it with git bash `ssh admin-username@VM-public-IP`, `sudo -l` to check that your admin has sudo access without requiring password. This is allowing my laptop's ip to connect with the Jump-Box

12. **Now I have one is Jump-Box-Provisioner, others VMs to be created are Web-1 and Web-2 and together these three make the 3 VMs on the VNet and I have setup SSH on my laptop and added security rules to allow me to connect to the Jump-Box using my specific ip address**

14. Now I am connected to Jump-Box-Provisioner via SSH-So far it does not have any provisioner function- and everyting below will happen in the Jump-Box-Provisioner which right now is just a VM. I first have to install docker in this VM which will let me install containers. Then I am going to download a container called Ansible which itself will act as a provisioner and will control other containers. Note that right now I am going to install it manually, but later I will use the YAML files to install programs and containers. Because after Ansible provisioner is up and running it can read YAML files and that is how it automates the process of installing new programs and containers-By reading YAML files- where we can assign tasks including installing a particular container. As I do not have ansible installed yet so for the first time I have to do this manually. Note that if I do not want to use `sudo` every time I can switch to root with `sudo su`
- Run `sudo apt update` then 
- `sudo apt install docker.io`
- `sudo systemctl status docker` to check that docker is running before I can download containers
- `sudo systemctl start docker` if the docker is not running
- `sudo docker pull cyberxsecurity/ansible` to download Ansible provisioner
- `docker run -ti cyberxsecurity/ansible:latest bash` This goes a layer deep and switches to terminl in Ansible container (Note that `run` is a combination of create+start a container)


**13**. Add Network Security group rule to give **Jump-Box-Provisioner SSH access to VNet**. This is similar to the rule used in **10**. This is an important step as this is essentially connecting the remaining 2 VMs(to be created) to the Jump-Box-Provisioner. So I can use the Ansible in the Jumpbox to also control the containers in the other 2 VMs (Web-1 and Web-2). Note that this rule is tied to the VNet. By adding the rule, I am telling the network security 'guard' that let Jump-Box-Provisioner VM whose IP is xx.xx.xx.xx connect to the network via SSH using the port 22
- Use the PRIVATE IP of Jump-box-Provisioner as assigned when it was created- It can be seen at 'overview' in left column when you select the Jump-Box. 
- Destination will be the virtual network, Destination port 22 (with which the Jump-Box will connect with other VMs)
- Paste the PRIVATE IP. 

14. To quickly summarize. I used SSH `ssh admin@jump-box-ip` to connect to Jump-Box-Provisioner and then in it installed docker, and ansible container and I then granted Jump-Box-Provisioner SSH access to the VNet- which is in essense the remaining VMs (Web-1 and Web-2- that will be created soon in the VNet). Now I can use the terminal inside to Jump-Box to install containers on other VMs

15. Next, I am going to create a new VM called Web-1 in Azure

16. Then, After I am connected to the **Jump-Box-Provisioner**, in its Ansible container, I am going to create a **new SSH key- second** and then I will paste this SSH key in Azure for the Web-1 VM so that now this VM can only be accessed from within Ansible container in the Jump-Box-Provisional. This adds an extra layer of security. 

17. Connect with the Web-1 VM using the ansible in Jump-Box-Provisioner using  `ssh admin@jump-box-ip`. Recap: May need:  `sudo apt install docker.io`, `sudo docker pull cyberxsecurity/ansible` and switch to root user `sudo su`, `docker run -ti cyberxsecurity/ansible:latest bash` or  `docker run -it cyberxsecurity/ansible /bin/bash` (as mentioned below)  and then `exit` to leave. Selecting bash opens terminal and give you an opportunity to create a key in the container

17. Inside powershell of Web-1 VM type `docker images` and then `docker run -it cyberxsecurity/ansible /bin/bash` to create + open ansible along with powershell in the container after connecting to Web-1. This will change the prompt Notice that I used `run` here instead of `start` because run also installs the container in addition to running the container. I may have to pull it first, then this image will be installed by run (see 17). Alternatively this can also be automatically done by 

18. Inside container powershell of Web-1 VM **new SSH key- Third** `ssh-keygen` and `cat .ssh/id_rsa.pub `, copy this key and then update it on Azure, afterwards you can test it with `ping ipaddress`


- **SSH key summary** so far
- SSH key on your computer
- SSH key in the Jump-box
- SSH key in VM2-


19. Exit the SSH session `exit`

20. Go to the Jump-box again using SSH. Now **Configure the ansible**, in particular the **host file** and the `nano /etc/ansible/hosts` and the **ansible.cfg**file to assign a username `nano etc/ansible/ansible.cfg` in the same way as outlined above in point 9 (configuring Ansible(Provisioner))

21. Now that the Ansible is configured in Jump-Box for automation. I will create a YAML file for automation. See point 

22. You can *run the YAML file* in the cotainer by typing `ansible-playbook /etc/ansible/pentest.yml` Then it can perform scheduled tasks like installing doker, and other containers like dva(dam vulnerable app) and then `curl localhost/setup.php` to test the connection. 

23. **Adding a load balancer** then add **health probe**(to check whether the server is working before you delegate resources to it) and a backend pool so the traffic can be transferred to the servers that are part of the **backendpool**. All the virtual machines that are included in the backpool must be on the same network
