# 5 - Networking

# VNets

A representation of your on prem network in the cloud. 

VNet::A logical isolation from the cloud dedicated to your subscription. Virtual network. Can be linked to other vnets. 
ID: 1670142766944


- Can link to other VNets if the CIDR's don't overlap
	- on prem and cloud

## subnets

A VNet can be subdivided into subnets. These provide logical subdivisions in the network. 

> [!note]
> Azure reserves the first four and last IP address for a total of 5 IP addresses within each subnet.

# security groups

NSG::network security group
ID: 1670142766955


NIC::network interface controller
ID: 1670142766962


ASG::application security group
ID: 1670142766969

# Azure Firewall

- stateful firewall as a service
- unrestricted cloud scalability
- across subscriptions & vnets
- static public IP 
- high availability
	- no additional loadbalancers required
- can have multiple public IP addresses

## hub-spoke

Beneficial when you have workloads in different environments that require shared services. Shared services are placed in the hub, such as DNS.

hub
- virtual network in Azure that acts as a central point of connectivity to your on-prem network

spokes
- virtual networks that peer with the hub
- used to isolate workloads

Traffic flows between on-prem datacenter and hub through ExpressRoute or VPN gateway connection.

Hub::Virtual network in Azure that acts as a central point of connectivity to your on-prem network
ID: 1670151949124


Spokes::Virtual networks that peer with the hub. Used to isolate workloads
ID: 1670151949130


- Benefits
	- cost savings by centralizing
		- DNS server in single location
	- overcome subscription limits by peering VNets to the hub


## firewall rules

- 3 kinds of rules
	- NAT rules
		- DNAT 
			- Azure Firewall Destination Network Address Translation
			- translates and filters inbound traffic to subnets
		- each rule translates public firewall IP and port to private IP and port
		- useful when publishing SSH or RDP applications to the internet
		- Needs network rule to allow traffic
	- Network Rules
		- any traffic that will be allowed to flow through firewall must have a network rule
	- Application rules
		- uses FQDN's
			- Fully Qualified Domain Names
		- defines which FQDNS can be accessed from a subnet
		- example: specify Windows Update network traffic through the firewall

Firewall Rules - 3 Types::NAT rules: translates the public firewall IP to the private IP and port. Network rules: any traffic that flows through the firewall must have a network rule. Application rules: Defines which FQDN's can be accessed from a subnet.
ID: 1670151949170


FQDN::Fully Qualified Domain Name
ID: 1670151949176


# Azure DNS

Manages and resolves domain names in a virtual network. 

Each domain has a DNS zone, and each DNS record for your domain is created in the DNS zone.

For example, contoso.com may contain a number of DNS records such as mail.contoso.com

DNS Zone::used to host the DNS records for a particular domain. 
ID: 1670151949181


> [!note]
> You do not have to own a domain name to create a DNS zone with that domain name. However, you need to own the domain to configure the domain.

## record sets

> [!important]
> There is a difference between DNS record sets and individual DNS records

A record set is a collection of records in a zone that have the same name and the same type. 

## private dns zones


Private zones provide name resolution for VM's within a virtual network and across VNets without having to create a custom DNS solution.

Name resolution across multiple virtual networks is probably the most common usage for DNS private zones.

They Allow you to use custom domain names within a VNet. 

Private DNS zones::Private DNS zones are used to configure name resolution between multiple vnets in Azure. They allow you to use custom domain names within VNets
ID: 1670175611848

DNS Apex domain::The highest level of your domain. AKA zone apex or root apex. Often represented by @ symbol in DNS zone records.
ID: 1670266082628


## alias records

A record and CNAME record don't support direct connection to Azure resources like load balancers. The solution is alias records. 

Enable a zone apex domain to reference other azure resources from the DNS zone. 

The main advantage is that you can assign DNS records to an azure resource such as a Public IP. When the Public IP changes, the DNS record will also be automatically updated. 

DNS - Alias records::CNAME and A record don't support direct connection to Azure resources like load balancers. Alias records are the solution. Can assign dns record to f. ex. an azure public IP. When public IP changes, the DNS record will be auto updated.
ID: 1670266082633


# VNet peering

VNet Peering::Connects VNets with each other. Once peered, VNets appear as one for connectivity purposes.
ID: 1670175611852


- regional vnet peering
	- connects vnets in the same region
- global vnet peering
	- connects vnets in different regions
	- all public cloud regions, but not government cloud regions

- traffic between peered networks is private
- high performance
- no downtime when creating the peering

## service chaining

when VNET 1 and 2 are peered, and 2 and 3, 1 and 3 are not paired. In other words, peering is not transitive.

user defined routes and service chaining can be configured to provide the transitivity.


# VPN Gateway

- sends encrypted traffic between Azure virtual network and on-premise location over the public internet
- also encrypted traffic over microsoft network
- ==each vnet can have only 1 VPN gateway

## virtual network gateway
- 2 or more automatically configured VMs
- deployed to the gateway subnet (Specific subnet for this)
- contain routing tables 
- run specific gateway services
- can't directly configure these VMs
- can be VPN or ExpressRoute


Virtual Network Gateway::2 automatically configured VMs that run specific gateway services. Cannot directly configure these. Can be ExpressRoute or VPN Gateway 
ID: 1670151949135

### gateway subnet

contains IP addresses that are used by the Virtual Network Gateway.

If possible, best to use /28 or /27 to provide enough IP addreses for future requirements. 

**==must be named GatewaySubnet

>[!important]
>Never deploy other resources (VMs) to the gateway subnet.


## VPN gateway

- can configure gateway type when configuring virtual network gateway
- type determines how it will be used and which actions the gateway takes
- type 'vpn' is different from ExpressRoute gateway
- Vnet can have one VPN gateway and one ExpressRoute gateway

When a vpn gateway is created, the gateway VM's are deployed to the gateway subnet;. 

- Enables creation of:
	- VPN tunnel connection between that VPN gateway and another VPN gateway ( VNet-to-Vnet)
	- cross-premises VPN tunnel connection 
		- Site-to-Site 
			- between VPN gateway and on-premises VPN device
		- Point-to-Site
			- VPN connetion over OpenVPN f.ex
			- from remote location to anything located in the virtual network
				- conference
				- home

VPN Gateway::A type of Virtual Network Gateway. Enables tunnel between the VPN Gateway and another VPN gateway (VNet to VNet)
ID: 1670151949144


Site to Site::Tunnel between VPN gateway and on-premises VPN device, connecting the on-prem network to the Azure VNet
ID: 1670151949154


Point to Site::Connection over f. ex. OpenVPN from remote location to anything located in the VNet. For example from home
ID: 1670151949163

### VPN gateway type

- route-based
- policy-based

Type depends on the connection topology. 

point-to-site requires Route-based VPN type.

Can also depend on the hardware. Site-to-site require a on-prem VPN device. Some devices only support a certain type.

>[!note]
>Most VPN Gateway configurations require a route-based VPN

- route-based
	- uses routes in IP forwarding or routing table to direct packets to tunnel interfaces
	- tunnel interfaces decrypt packets in-out of the tunnels
- policy-based
	- uses IPsec policies
		- combinations of address prefixes between on-prem and azure vnet
		- policy
			- acces list in the VPN device configuration
	- limitations
		- only basic gateway SKU
		- only one tunnel
		- only for site-to-site connections


VPN Gateway type (2)::Route-based and policy-based. Most configurations require route-based VPN. Policy-based is only for site-to-site (S2S).
ID: 1670175611857


### high availability

Every Azure VPN gateway consists of two VM's in an active-standby configuration. 

If one fails, the other VM takes over. 

#### active-active

this is when you have both instances of the Gateway VMs establishing connections

# ExpressRoute & Virtual WAN

## ExpressRoute

==A service that provides a direct connection from the on-premises datacenter tot he microsoft cloud. 

Used for enterprise-class and mission critical workloads. 

A direct, private connection from your WAN (not over the public Internet) to Microsoft services, including Azure. 

Works with approved connectivity provider to establishes the connections via a dedicated circuit.

ExpressRoute::Direct connetion from on-prem datacenter to the Microsoft Cloud. Not over public internet. Uses an approved connectivity provider and a dedicated circuit. Used for mission-critical and enterprise-class workloads.
ID: 1670175611861


Difference with VPN
- site-to-site VPN traffic travels encrypted over the public internet
- S2S vpn is a secure failover path for ExpressRoute

- extends on-prem network to Microsoft cloud services
	- Azure
	- 365
	- CRM Online

When connecting to Microsoft in Amsterdam through ExpressRoute, you'll have access to all Microsoft cloud services in Northern & Western Europe

### connection

- colocated at cloud exchange
	- use provider's Ethernet exchange at colocation
- point-to-point ethernet connections
	- cable to the datacenter
- any-to-any IPVPN networks
	- Integrate your WAN with the Microsoft cloud

- point to site/vnet connection is fine for dev, test and lab
- Vnet site-to-site for small production workloads
- ExpressRoute for enterprise-class and mission-critical production workloads & big data solutions

## Virtual WAN

Basically the hub-spoke model on steroids.

Simplifies complex hub-spoke VNet WAN deployments.

The virtual WAN is the hub  and you can connect VNets as spokes. 

> Azure Virtual WAN brings together many Azure cloud connectivity services such as site-to-site VPN, User VPN (point-to-site), and ExpressRoute into a single operational interface. Connectivity to Azure VNets is established by using virtual network connections.


Virtual WAN::Hub-spoke on steroids. Simplifies complex hub-spoke  WAN Deployments. The Virtual WAN is the hub and you can connect VNets as spokes. Can connect to the hub via expressroute, S2S vpn or P2S vpn. 
ID: 1670175611866


# routing and endpoints

- system routes
	- refers to the default routing that is automatically done by Azure
	- as opposed to User Routes
	- directs network traffic between
		- VMs
			- in same subnet
			- different subnets in same VNet
			- data flow from VM to internet
		- on-prem networks
		- internet

- routing tables
	- contain information about system routes
	- set of rules that specify how packets should be routed
	- associated to subnets
	- matched by using destinations, which are:
		- IP address
		- VNet gateway
		- Virtual appliance
		- internet
	- if matching route can't be found, the packet is dropped


UDR::User Defined Routes. Control network traffic by defining routes that specify the next hop of the traffic flow. As opposed to system routes
ID: 1670175611870


System Routes::routing that is done automatically done by azure. Directs traffic between internet, on-prem networks and VMs in same subnet, different subnets in same VNet and data flow from VM to internet.
ID: 1670175611874


Routing tables::contain information about system routes. Set of rules that specify how packets should be routed. associated to subnets
ID: 1670175611878


- User Defined routes
	- as opposed to system routes
	- custom configuration, for example:
		- directing traffic to a VM that performs routing or firewalling


>[!note]
>Each route table can be associated to multiple subnets, but a subnet can only be associated to a single route table.

## endpoints

Used for services such as Azure AD, Cosmos DB, Storage

>A virtual network service endpoint provides the identity of your virtual network to the Azure service.

Normally traffic from an Azure VNet uses a public IP address as a source address. 

Service endpoints switches this traffic to use a virtual network private address as the source IP when accessing the azure service from a virtual network. 

This switch allows you to access the services without the need for reserved, public IP addresses used in IP firewalls.

- Why?
	- improved security for azure service resources
		- VNet private address spaces can overlap, therefore not suitable to identify traffic origin
			- hence public IP's are used normally
		- fully removes public internet access to resources
			- allowing traffic only from your virtual network
	- optimal routing from VNet
	- keeps traffic on Azure backbone network
	- no need for reserved public IP adresses in VNets
	- no NAT or gateway devices required to set up service endpoints

Service Endpoint::Provides identity of your VNet to the Azure service (Storage, CosmosDB). Normally public IP's are used to identify VNet. With a service endpoint, you use a private VNet IP as the source IP, thus shielding everything from the internet. 
ID: 1670175611881


## Azure Private Link

Azure Private Link::Private connectivity from a VNet to Azure PaaS or Microsoft partner services
ID: 1670175611885


- Simplifies network architecture
- eliminates data exposure to public internet
	- traffic remains on Microsoft network
- global, no restrictions
- accessed  via
	- private peering
	- VPN tunnels from on-prem or peered VNets

# Azure Load Balancer

Distributes inboud traffic to backend resources. 

Two types: public and internal

- public
	- maps public IP and port of incoming traffic to private IP and port of the VM
	- mapping response traffic from VM
	- load balance rules to distribute across multiple VMs or services

- internal
	- directs traffic to resources inside a VNnet ==**or that use a VPN to access Azure
	- frontend IP addresses and VNets are never directly exposed to internet endpoint
	- However, example internal load balancer:
		- receiving database requests and distribute to backend SQL servers
	- cross-premise VNet
		- load balancing from on-prem to VM's in the same VNet


- Session persistence
	- how traffic from client should be handled
	- ==important! example: shopping cart
	- default: none
		- handled by any VM
	- client IP
		- successive requests from same  client IP are handled by same VM
	- client IP and protocol
		- same combined with protocol

- health probes
	- allows load balancer to monitor app status
	- removes or adds VMS from load balancer rotation based on response to health checks
	- when probe fails, LB stops sending new connections to it

Load Balancer - Session Persistence::Example: shopping cart. If you need session persistence, successive requests from the same client IP must be handled by the same VM
ID: 1670175611888


Load Balancer types (2)::internal and public
ID: 1670175611893

# Application Gateway

Application Gateway::Provides load balancing and application routing capabilities across multiple websites.
ID: 1670266082638


Manages requests that client applications send to a web app.

- Uses application layer routing
	- routes to pool of web servers
	- based on URl of a request
	- can be 
		- VMs 
		- scale sets 
		- Azure App service

WAF::Web Application Firewall. Checks for threats based on OWASP. 
ID: 1670266082643


# IP Address Schema

Three ranges of non-routable IP addresses for internal networks:
-   10.0.0.0 to 10.255.255.255
-   172.16.0.0 to 172.31.255.255
-   192.168.0.0 to 192.168.255.255

- VNet is different from on-prem network
	- ability to scale up and down
	- provisioning happens in seconds
	- no physical devices
	- virtual

## VNets

- A network in the cloud
- can be divided in subnets

- By default, all subnets can communicate with each other in a VNet
- deny communication using NSG

> [!note]
> the smallest subnet that is supported has a /29 subnet mask. The largest has a /2 subnet mask

Smallest Azure VNet subnet::/29 subnet mask. Largest is /2
ID: 1670266082647



## planning an IP scheme

- requirements
	- how many devices do you have on the network?
	- How many devices are you planning to add in the future?
- Based on the services running on the infrastructure, what devices do you need to separate?
- How many subnets do you need?
- How many devices per subnet will you have?
- How many devices are you planning to add to the subnets in future?
- Are all subnets going to be the same size?
- How many subnets do you want or plan to add in future?

# VNet Peering

- peering = connecting VNets together
- when peered, VM's in these networks can connect as if they're in the same network
- private traffic on Azure network
- can peer across subscriptions

- 2 types of peering
	- Virtual network Peering
		- VNets in the same Azure region
	- Global VNet peering
		- VNets in different regions

- reciprocal connections
	- when connecting through Azure CLI, only one side of the peering gets created
	- when connecting through Azure Portal, both sides get created

- transitivity
	- VNet peering is nontransitive
	- only directly peered networks can communicate
	- cannot communicate with peers of peers

- Gateway transit
	- can configure a hub with a gateway to connect with on-prem network
	- Using "Allow gateway transit" option, you can connect to all the spokes when connected to the hub from the on-prem
	- prevents need to deploy gateways to all VNets

# Routing

BGP::Standard routing protocol for routing two or more networks. Typically used to share on-prem routes to Azure when connected through ExpressRoute. 
ID: 1670266082651


If multiple routes are available in a route table, the most specific option is used. 

So for 10.0.0.0/16 and 10.0.0.0/24, the /24 is used because it has less IP addresses. 

## routing priority
1. user defined routes
2. BGP routes
3. System routes

This learn module has a lot of examples of creating subnets with cli:

https://learn.microsoft.com/en-us/training/modules/control-network-traffic-flow-with-routes/3-exercise-create-custom-routes?activate-azure-sandbox=true

## NVA

NVA::Network Virtual Appliance. Secures and monitors traffic between front-end public servers and internal private servers. 
ID: 1670266082655


Virtual machines that control the flow of network traffic by routing. Typically used to manage traffic flowing from a perimeter-network environment to other networks or subnets. 

Acts as a router that forwards requests between subnets on the VNet. The requests are inspected by the firewall on the NVA. 

