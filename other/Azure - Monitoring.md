# Azure - Monitoring

# John Savill - Azure Masterclass part 9 - Monitoring and Security

https://www.youtube.com/watch?v=hTS8jXEX_88&t=1081s

Starting from Azure AD and moving down the hierarchy:

- Azure AD
	- always at the top
	- Sign in logs
	- audit log
	- 30 days
- Subscription
	- Activity log
		- creating resources
		- deleting resources
		- deleting a key
		- service health
		- 90 days
- Resources (ARM)
	- Metrics
		- Turned on by default
		- 90 days retention
		- Fast pipeline, sub 60 seconds
	- Logs
		- Must be configured
		- they do not exist until I configure the collection of logs
		- Numeric or Text
- OS
	- These also have metrics and logs.
	- ==NOTE these are different from the ARM logs
		- ARM VM metrics are things the VM sees on the Hyper-V host level, not the OS level
	- So metrics at this OS level are the metrics inside the OS
	- The actual CPU usage of the machine inside the VM
	- Diagnostics Extentions
	- Agents 
	- Metrics
	- Logs (syslog)
- App Insights
	- Metrics
	- Logs
	- How the application is performing
	- what it's talking to
	- Codeless attach
	- Can plugin to the runtime
- Custom things
	- you can configure custom things that collect logs

==All of the above are sources of data. 

You need to understand all of these things to have a good understanding of the application you are hosting. 

To activate logs, go in the portal. F. ex. subscription, and then go to Diagnostics settings. Here you can activate the different kinds of logs and where you want to send them. 

## Log Destinations

We have all these data sources. Where are we sending it to?

- Send to Log Analytics
- Archive to storage account
- Stream to an event hub

- Storage
	- Cheap
	- Long term retention
- Event hub
	- you can push events to it, and people can subscribe to it
	- 3rd party SIEM solution
	- OpsGenie
- Log Analytics
	- Azure Monitor Logs
	- ==The Superhero
	- Log Analytics Workspace
	- This breaks the data down in to tables and formats
	- can run against queries against these
	- 2 year max retention
		- configurable retention
	- Costs
		- Ingestion
			- getting the logs into Log Analytics has costs
		- Storage
			- Retention
		- There are a few exceptions, but typically you have to pay for the data that comes in and the data you store
		- You can set Caps
			- Ingestion cap: do not cost more than xxx per day
	- Aanalyze
		- Visualize trends

Which 3 destinations are there for logs?::Accessed in diagnostics settings. They are storage, event hub and Log Analytics.
ID: 1670859846171


What is Event Hub?::Used in monitoring. You can send logs and metrics to the event hub and people can subscribe to the events, or you can connect a SIEM solution such as Opsgenie or splunk. 
ID: 1670859846187


Generally you can set three options in diagnostics settings
- storage
- event hub
- Log Analytics

The exception is for OS logs (Agents), App Insights, and custom things. These can only go to Log Analytics.

Can you send App Insights logs to the Event Hub?::No, App Insights can only be sent to Log Analytics
ID: 1670859846194


Can you send OS agent logs to blob storage?::No, they can only be sent to Log Analytics
ID: 1670859846202


## Log Analytics

You can export Log Analytics tables to Storage or Event Hub. 

How can you get OS Agent logs to storage?::You can export the tables from Log Analytics to storage. 
ID: 1670859846209


## costs

Ingestion and retention have costs. So only turn on what you need, what you care about. Have a plan. 

## dashboard or workbook

dashboards can be shared and pinned, have rbac

workbooks can contain visualisations. more of a document. designed to go in an analyze a certain scenario. no native rbac. to share you need access to all underlying resources

## Azure Sentinel

- SIEM solution
- feeds of log analytics
- alerting, playbooks
- uses machine learning for detecting threats
- but also has its own connectors
	- syslogs
	- network devices
	- event forwarding
	- firewalls
	- other services
