# Cloud Computing

_"On-demand computing resources, delivered over the internet"_

# Properties of CLoud Computing

__Elastic__

Allows us to easily scale our applications to accomodate the number of Users at any given point in time. We can quickly provision resources when traffic is high, or we can scale down our application when User count drops

__Metered__

Pay only for what you are using (not a subscription)

__Self-Service__

The consumer can provision the server themselves, no need for physical hardware or for specialists to setup the server configurations

## Benefits of Cloud Computing

- Fast access to resources or applications
- Pay for what you use
- No capital expenditure (no need to purchase physical hardware)
- Eliminate need for specialized employees for system administration
- Lower company costs
- Self-service deployment and provisioning of resources
- IT staff and system admins can focus on leveraging cloud technologies to increase the company's bottom line (net profit)

## Risks of Cloud Computing

- No access to data if the Cloud servers go down
- Potential for loss of data
- Potential for slow access to data (bottlenecked by network connection speed)
- Storing sensitive data must adhere to certain laws and regulations
- Loss of customization
- Unknown costs

# Types of Cloud Computing

__Software as a Service (SaaS)__
Accessing applications directly running in the cloud. Paying to use these applications that are already established

__Infrastructure as a Service (IaaS)__
Accessing a virtual machine that is running in the cloud. Virtual machine allows us to download applications, install database servers, etc..

__Platform as a Service (PaaS)__
Accessing a development platform that allows engineers to implement their changes/updates for deployment


# Infrastructure as a Service (IaaS)
___Virtualization___ - fully self-contained computers in a virtual machine.

- Required for Cloud Computing
- Scalability and Elastic Computing
- Resource Sharing and Pooling
- Load Balancing and High Availability
- Portability

|Term|Description|
|:-:|:-|
|Container|operating-system level virtualization that provides isolated user-spaces to run specific applications|
|Hypervisor|Runs a virtual machine|
|Virtualization|division of physical and computing resources|
|Virtual Machine|software-based instance of a physical server which provides access to emulated virtual hardware|

## Hypervisor
A _Virtual Host_, containing a hypervisor with virtual guests, provides the physical resources used by the virtual machines to run the operating systems and applications. The applications are oblivious to the fact that they are running on virtual hardware on the virtual machine  

## Networking

__Public Networking__ - connect via the internet with public IP addresses

__Private Networking__ - use VPNs (Virtual Private Networks) to connect to virtual machines in the public cloud (traffic is encrypted and secured)

__Virtual Private Cloud Networking (VPC)__ - Private network that can be connected to your network

- control private IP address networking
- Routing
- Network access lists
- VPN connectivity to on-premises infrastructure

## Storage

__Virtual Machine__ has its own storage 
- Raw files in disk
- Database servers

__Outside Virtual Machines__ 
- Virtual machines can access external storage in the cloud
	- _block storage_ - shared storage between all virtual machines running in the cloud
	- _object storage_ - store unstructured data (graphics, videos, etc..)


## Containers
___Containers___ - fully self-contained applications in a container

- Contain one operating system 
- Can be run on virtual machines
- Less overhead and faster startup times than virtual machines

# Private Cloud
Run on-premises using private company infrastructure. Not accessing data across the internet

- Virtualization
- Abstraction of infrastructure layer (do not need to know about the underlying hardware)
- Secure multi-tenancy (can support multiple areas or companies securely)
- Self-service portal
- Catalog of applications

# Hybrid Cloud
Connecting private and public cloud

# Public Cloud
Run off-premises, accessing data over the internet by paying a service fee


# Service Level Agreements (SLAs)
Defines the level of performance and availability that will be provided to the consumer. Also defines what the provider will do if they are unable to provide the agreed upon services.

# Platform as a Service (PaaS)
PaaS is for _developers_ and runs on top of infrastructure

- Develop, run and manage applications in the public cloud
- No needed awareness of servers, storage, network, middleware, databases

# Software as a Service (SaaS)
Complete applications provided by the Cloud

- No hardware to buy or install
- No software to buy or maintain
- Updated with new features  

