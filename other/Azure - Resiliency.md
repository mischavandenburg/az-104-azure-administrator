# Azure - Resiliency 
Links: 
https://youtu.be/zLMXu4rtlEk

## notes from John Savill video

- snapshots are stored on the same medium, so they won't be replicated. something to consider in the backup strategy
- asynchronous vs synchronous
	- asynchronous
		- app writes to postgres master
		- pg master instantly acknowledges the write
		- pg master then starts replicating to slave
		- risk: the replication might not work and you lose the replication
		- advantage: useful when there is a lot of distance (and therefore latency) between the master and the slave
	- synchronous
		- transactions are not comitted on primary (master) until acknowledged on the secondary
		- can impact primary performance
		- i.e. the app needs to wait until the data is replicated until it gets the acknowledgement
		- no risk of data loss
- usually you use synchronous when they are in the same location because latency is sub millisecond
- asynchronous when in different locations (Cross -site)

Asynchronous replication::App writes to primary and primary instantly acknowledges the write. After acknowledging, the primary starts replicating to secondary. Risk of data loss. Used for cross-site becuase of the higher latency. 
ID: 1670957141142


Synchronous replication::Transaction not commited on primary until acknowledged on secondary. Can impact performance, but no risk of data loss. Used when primary and secondary are in the same location. 
ID: 1670957141148


- fault domain
	- server rack
	- nodes in the rack can fail, or the entire rack can fail
- availability set
	- 99.95% sla
	- resources are added on the availability set
		- these are installed round robin
		- distributed over fault domains automatically
	- in the same facility
	- ==never mix workloads in the same availability set
	- different sets for different loads

- update domains
	- 5 to 20
	- host updates cause a vm freeze of a few seconds
- availability zones
	- data centers within the same region

- VM harddisk SLA
	- premium ssd 99.9
	- standard ssd 99.5
	- standard HDD 95

- availability SLA
	- set 99.95
	- zone 99.99

>[!quote]
>If there is no state, you don't need replication.
>Just have the process in place to create it via IaC

