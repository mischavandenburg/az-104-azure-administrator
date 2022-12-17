# Azure - Governance
Links: 
https://youtu.be/cIh_Nfl67T0

Vnets are bound to a subscription. You cannot move vnets between subscriptions. 

Vnets from other subscriptions will need to be peered and this can involve a lot of work. 

- resource groups 
	- Resources live in RG
	- A resource can exist in only 1 RG. 
	- not boundaries
		- a vm can have its nic connected to a vnet in a different RG as long as they are in the same subscription
	- tags are not inherited
