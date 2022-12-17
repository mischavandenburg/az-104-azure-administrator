# 6 - Monitor and Back Up Azure Resources

# File and Folder Backups

## Azure Backup Center

- single unified management experience
- supports:
	- azure vm backup
	- sql azure vm backup
	- azure files
	- azure blobs
	- managed disks
	- PostgreSQL server backup

## recovery service vault

- a storage entity that stores backup data
	- vms
	- sql databases
	- azure file shares

Recovery Service Vault::storage entity that stores backup data, such as VM snapshots or Azure SQL databases
ID: 1670266082659

# VM backup

- several options:
	- snapshots
		- quick and simple
		- for vms using managed disks
		- billed based on size
	- azure backup
		- production workloads
	- azure site recovery
		- for major outage due to natural disaster
		- when whole region is out
		- replicates entire workload structure to secondary location

# Azure Alerts

Action group::collection of notification settings defined by an Azure subscription. List of emails that will be alerted when metrics are above a certain level
ID: 1670266082663


# Network Watcher

- Network diagnostics
- for solving network problems
- elements
	- monitoring
	- network diagnostic tools
	- metrics
	- logs

Network Watcher::Network diagnostics for solving problems. Provides monitoring, diagnostics, metrics and logs.
ID: 1670361824265


- IP Flow verify
	- checks if packet is allowed or denied to/from VM
	- identify security groups

IP Flow verify::checks if packets are allowed/denied to/from VM. Identifies security groups. 
ID: 1670361824268


- next hop
	- determine if traffic is directed to intended destination
	- verify routing configuration
	- return value none:
		- valid system route to destination
		- no next hop to route the traffic

- network watcher topology
	- generate visual diagrams

Network Watcher Topology::generates visual diagrams of your network
ID: 1670361824272


Next Hop::verifies routing configuration by giving the Next Hop and the associated routing table
ID: 1670361824276

# Kusto query language

heartbeat table::For Azure monitor querying with Kusto. Contains data on everything from OS type, os version, resource ID and resource group. Acts like an inventory of all VMs reporting to a specific workspace.
ID: 1670597879320



