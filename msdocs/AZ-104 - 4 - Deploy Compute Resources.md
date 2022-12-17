# AZ-104 - 4 - Deploy Compute Resources

# VM Availablilty

==Azure doesn't automatically update / patch VM OS. 

- Availability Sets
	- logical group of VMs
	- not upgraded at the same time during datacenter host upgrade
	- ensures deployment
	- should have identical functionalty and software
	- Azure ensures they run across multiple physical servers, storage and switches
	- only subset of VMs is inpacted in case of failure

Availability Set::Logical grouping of VMs which ensures they are always deployed. Identical tasks and software, only subset is impacted in case of failure. Redundancy.
ID: 1669745066200

- update domain
	- group of nodes that are upgraded together
	- can be rebooted at the same time
	- default: 5

VM Update domain::Group of nodes that are upgraded together. Can be rebooted simultaneously.
ID: 1670597879203


- fault domain
	- VM group that shares common point of failure. 
	- Example: server rack

VM Fault domain::VM group that shares common point of failure. For example, a server rack.
ID: 1670597879208


- availabilty zones
	- unique physical locations within region
	- made up of 1 or more datacenters

Availability zone::Unique physical location within a region, made up of 1 or more datacenters. Combination of fault domain and update domain. 
ID: 1669745066209

Vertical scaling::scaling the vm size up or down. Requires a stop and restart.
ID: 1669745066213

horizontal scaling::When the number of vm's is scaled up or down
ID: 1669745066217

- scale set
	- deploys a set of **identical** vms
	- autoscale
	- manual or automated
	- up to 1000 vm instances

scale set::deploys a set of identical VMS. Can scale manual or atuomated. 
ID: 1669745066221



# Azure Container Service

Windows or Linux "Container as a Service"

Have a container running in seconds. 

- container group
- share a lifecycle, resources, local network, and storage volumes.

Container Group::Similar to a pod in Kubernetes
ID: 1669991594603

## container

- instance of a docker image
- execution of a single application, process or service
- consists of 
	- Docker image
	- execution environment
	- standard set of instructions
- scaled by creating multiple instances of the same image

# Azure Automation State Configuration

DSC::Descired State Configuration. Declarative model of PowerShell
ID: 1670142766974


AAS has a built in pull server. Target nodes will automatically pull configuration changes from here to conform to the desired state. 

LCM::Local Configuration Manager. Component of Windows Management Framework (WMF) on a Windows OS. Responsible for updating state of node (e.g. VM) to match desired state. 
ID: 1670142766979
