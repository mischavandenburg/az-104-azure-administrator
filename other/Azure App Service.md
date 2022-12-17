# Azure App Service

An app runs in an App Service plan. 

- App Service plan
	- defines set of compute resources for a web app to run
	- can have one or more apps
	- each app plan defines:
		- region
		- number of VM instances
		- size of VM instances

### how it works

In the Free and Shared tiers, an app receives CPU minutes on a shared VM instance and cannot scale out.

From Microsoft Docs:

- Apps run in service plan
- multiple apps in the same plann will share the same VMS
- multiple deployment slots also run on the same VMs
- The plan is the scale unit
	- autoscaling will scale all apps in the plan
- Because apps use the same resources, be careful to add more apps to your plan
	- understand the capacity of the plan
	- understand the load of the new app

## deployment slots

- live apps with their own hostnames

==A deployment slot is an instance of an app service, running on the same app service plan.

Must be swapped to deploy the instance to production. This allows deployment without any downtime.


Deployment slot::A deployment slot is an instance of an app service, running on the same App Service plan. 
ID: 1670597879212

# Savill Video

https://youtu.be/_E73_SQN8ZU

- can run a container or app
- you purchase nodes, its paas
- app service plan
	- certain size
	- you deploy app servcies into the plan
	- it runs across 3 nodes if size is 3
	- you buy guaranteed resource
	- if you scale out, all of the apps in the plan scale out
	- you scale the plan, not the app
	- when you scale the plan, everything in the plan will be scaled

# pricing

How many minutes of CPU time do you get with a Free F1 App Services plan?::60
ID: 1670957141154


How many minutes of CPU time do you get with a Shared D1 App Services plan?::240
ID: 1670957141160


How many hours of CPU time do you get with a Basic B1 App Services plan?::24
ID: 1670957141167



