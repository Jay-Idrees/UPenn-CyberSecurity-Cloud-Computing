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

## IaaS (Infrastructure Service)

1. **Resource Goup** Creating this is the first step so you can better organize resourses-logical grouping of resources. It consists of:
        - network - A virtual network is a collection of virtual machines - easy to re-configure
        - firewall
        - virtual computers




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
