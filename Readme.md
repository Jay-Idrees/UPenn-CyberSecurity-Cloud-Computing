# Cloud Computing

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

5. **SSH setup**

- Generating the SSH key
- Pasting the SSH key inside the Setting of the VM, so that it maybe connected

- A network behind the gateway is more secure



6. **Connecting to a VM using SSH** This is the same as **Configuring a VPN - Virtual Private Network** or **Setting up a jump box**

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



8. **Containers** - Low scale VMs - (Because they have no OS or hardware of their own)- Easily duplicated

- Typically, you design a VM and then create containers inside it (e-g 100 containers), containers can also be automatically created. 

- The containers can be linked. There can be a database container, applicattion containers. Goal is to make a machine as stateless as possible. If the containers are task oriented rather than user oriented, then it makes easy to create and destroy containers that have not been changed. 

- `Horizontal scaling` Creating more containers to handle additional load. (preferred on cloud as its more flexble)
- `Vertical scaling` is when you add more RAM or CPU to the verticle scaling. 

- Additional containers can be created for to manage load as the number of users expand

- LAMP web server, you will need to: install Linux, a web server like Apache, a database like MYSQL, and a back-end codebase like PHP. This VM can be copied if another LAMP server is needed

- We could create a LAMP container image which is downloaded as an app, the container is cerated from the image that was downloaded - The image can be easily downloaded from computer to computer

- Containers can be attacked - and easily replaced w/o damage to the rest of the network, if they have not been changed- thats where the task oriented containers come into play. As you can have database containers that store only the data- the rest of the containers can be deleted or replaced if compromised. 

- The Purpose is to share common resources and create a new image of only what is different- Thats the purpose of the container

- The main different between the multiple containers and the multiple VMs which are copies of the same original image that the containers can share some files or be connected to the same resources, while the VMs are completely independent. Both the VMs and the conainers can share the same host (Virtual network)

9. **Docker** - Program to make and manage custom containers

- Makes it easy to install complex server configurations

- You can download containers created by other users- has a container hub

## IaaC (Infractructure as code)

- We can code such that a VM is a server and has many containers. `Provisioners` are used for 

- Using code to configure the network and keeping track of changes to configuration. The changes are also reversible

- Servers can send data to a central database, thus the servers are only composed of a small text files of code. We can even send the logs to a database

- It also makes it easy to turn back an update if there are errors with new updates 
- **Provisioners** are used for automation - bring servers to a **state of operation**


10. **Load Balancer** 

- Three step process

        1. Add Load balancer using +Add to the resource group
        2. Configure healthprob
        3. Add your VM to the backend pool so the VMs are behind the loadbalancer's external IP

- Provides an external IP for the website to be accessed over the internet and points that ip to a VM. The name of this IP should be the most uniquie in all of Azure.

- Distributes traffic over multiple servers. More servers can be added to the group as traffic increases

- Helps mitigating DoS attacks by having multiple servers running the same website behind a load balancer- so the traffic can go to the other server which is not hacked 

- THe load balancer can perform a health probe to check whether a server is fully operational, before delegating the resoursces to it. 
- 

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


## Activities:

- Setting up virtual network
- Setting up inbound an outbound rules


## Setting up a virtual machine using Azure

## Setting up a firewall for virtual machine
- Can set rules on multiple ports
**Network Security group (NSG)** to create an NSG that blocks all traffic to and from the network
- Thhis model can allow creating a template NSG which can be modified for future VNs
- Allow creation of NSGs for different traffic profile
- A security group is a unique network into which you can deploy machines
- We can use this to deploy a remote desktop on a specific port
- 

- Before setting up devices in the cloud the budget limits and the cost-control policies should be defined

**AVailability Set**
**Availability Zone**

Aure limits free tire users to only 4v CPUs per region

In summary, an Availability Set is a group of VMs, where each VM is in a different Availability Zone. If one of the Availability Zones goes down, the VM in the other picks up the workload.

Note: Azure limits free tier users to only 4 vCPUs per Region. Notice that the default machine will normally have 2 vCPUs and 8 GiB of memory. For this jump box, the machine size should be changed to a smaller machine to conserve resources.

The Jump-Box machine only needs 1 vCpu and 1 GiB of memory.

 SSH key pair to access our new machine 

 IMPORTANT: Windows users should be using GitBash to use these commands and create ssh connections.

Open a terminal and run ssh-keygen.

You will be prompted to save the SSH key into the default directory ~/.ssh/id_rsa. DO NOT CHANGE THIS LOCATION. Press the Enter key.
Run cat ~/.ssh/id_rsa.pub to show our id_rsa.pub key:


Copy the SSH key string and paste it into the Administrator Account section on the Basics page for the VM in Azure.

For SSH public key source select Use existing public key from the drop down.
You will use the same SSH key for every machine you create today. Later we will change the key on a few machines.


We created the network and blocked all traffic before placing any VM's inside of it.

This ensures that it is truly impossible for an attacker to gain access during the configuration process.
Then, we added an SSH key through secure methods, ensuring that only the user with the correct private key (you) will be able to connect to the machine.

Still, this private key is essentially useless until the NSG is updated to allow SSH traffic.

This further protects the machines on the network by ensuring they can't be accessed, by the private key owner or any attacker who intercepted it, until the cloud administrator explicitly allows such access.

In the next class, we'll update the NSG, following the principle of least privilege, to allow only inbound SSH traffic.

We will configure the NSG so it only allows one IP address to open connections. This will ensure that if an attacker steals the key remotely, they will only be able to connect to the VM if they also successfully compromise your development machine.
