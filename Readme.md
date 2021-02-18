# Cloud Computing

## Examples of Web Services

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
