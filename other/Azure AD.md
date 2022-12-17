# Azure AD

https://learn.microsoft.com/en-us/azure/active-directory/fundamentals/active-directory-whatis

### devices
https://learn.microsoft.com/en-us/azure/active-directory/devices/overview

# What is Azure AD?

> [Azure Active Directory (Azure AD)](https://learn.microsoft.com/en-us/azure/active-directory/) is Microsoft's multi-tenant cloud-based directory and identity management service.

Azure AD is an identity provider. A full identity solution.

> Azure AD is the underlying product that provides the identity service

### for admins

Azure AD is used for controlling access to your applications and resources. 

For example, you can use Azure AD to require MFA when accessing important resources. 

### for developers

You can use Azure AD to add SSO to your application. This allows your app to work with pre-existing credentials. 

### b2b

manage your guest users and external partners

### b2c

customize and control how users sign up, sign in and manage their profiles when using your apps

- function
	- provides user access to resources and applications
		- internal resources on corporate network
		- external resources such as 365, Azure portal, SaaS applications
		- cloud apps developed for your organization

Azure AD provides identity authentication and authorization for cloud resources such as Azure resources, azure apps and microsoft 365

## features

- SSO 
	- single sign on to web apps in the cloud and on-prem apps
	- can use same set of credentials to access all apps
- supported on many devices: iOs, windows, android, macos
- remote access
	- secure remote access for on-prem apps from anywhere
	- secure access
		- MFA
		- conditional access policies
		- group based access management
- consistency
	- Microsoft calls this "cloud extensibility"
	- ==A consistent set of users, groups, credentials and devices across environments
- customizing the experience
	- banners, fonts etc
- self service
	- SSPR
	- delegate tasks from admins
- Uses REST API 
	- HTTP/HTTPS protocols
		- OIDC
		- OAuth
		- SAML
	- Does not use Kerberos
	- Cannot be queried by LDAP
- Managed Service
	- you only manage users, groups and policies
	- for AD, you would also manage configuration and patching etc

# AD and Azure AD

- Azure AD is NOT Active Directory in the cloud. 
- AD speaks kerberos, Azure AD doesn't
- AD is primarily a directory, Azure AD is a full identity solution

# tenant

>An Azure _tenant_ is a single dedicated and trusted instance of Azure AD. ==Each tenant (also called a _directory_) represents a single organization.== When your organization signs up for a Microsoft cloud service subscription, a new tenant is automatically created. Because each tenant is a dedicated and trusted instance of Azure AD, you can create multiple tenants or instances.

Azure Tenant::Single dedicated & trusted instance of Azure AD. Also called directory. Automatically created when creating a subscription.
ID: 1669478872747

# notes savill video

https://youtu.be/Jd3IzN9x2as

- main difference free and P1 is ability to use MFA
- most organizatins will use p1
- AD is very fiddferent from azure AD
- AD is the source of truth
- password changes are not replicated unless you make special configuration
- azure ad does not live in a subscription
	- az subscriptions trust tentnats (ad instances)
	- resources need to trust azure ad before it can do anything
	- subscriptions are moved to trust an azure ad

- managed identity
	- azure resource such as vm can have an identity in azure ad
	- within that resoruce i can act as that identity to authetintacate and authorized to use other resources in azure

# savill line between ad and azure ad

https://youtu.be/uts0oy8NlUs

- azure AD has a huge library of federations
- with AD, you had to confiugre all of these yourself
- conditional access
	- detecting strange activity
	- diffetent ip address, non regular times
	- can for example require you to MFA when you log in from a different IP
- collaboration
- 

## writeback

Situation: on prem AD and azure AD

When a user changes its password, its changed in on prem AD. 

Azure AD Connect only synchronizes in one direction: Azure AD to on prem AD. 

To enable synching to Azure AD, you need to enable password writeback. 

Which license do you need for password writeback?::Writeback requires Azure AD Premium P1 or P2.
ID: 1670771143839


Password writeback::On prem AD to Azure AD only syncs in one direction: Azure AD to on prem AD. When a user changes a password on the on prem AD, it won't sync to Azure AD. You need to enable password writeback to sync to Azure AD. 
ID: 1670771143847


Is Azure AD Connect required for writeback::Writeback requires Azure AD connect to be installed. 
ID: 1670771143851


Is MFA required for password writeback?::MFA is recommended but not required for writeback. 
ID: 1670771143855


Azure Front Door::Global access solution for web applications. It is not on the AZ 104 outline. It will always be an incorrect answer for the exam. 
ID: 1670771143859

### device options

- registered devices
	- bring your own device
		- not owned by organization
	- does not require organizational account to sign in to device
	- Azure AD is not authenticating, but personal accounts
	- can use personal accounts e.g. Microsoft Outlook account to sign in and access resources
	- this limits 2FA because they are personal accounts
		- must be supported by the identity provider for the personal account
	- FIDO is unavailable in this case because it's not Azure AD that's authenticating, but the personal account

- hybrid devices
	- Owned by organization
	- uses on prem AD as identity provider
	- joined to on prem AD and registered with Azure AD
	- requires organizational account to sign in to device
	- cannot use FIDO because it requires keys to be generated by Azure AD
	- Only possible if Azure AD is the identity provider

- joined devices
	- owned by organization
	- enable all authentication configurations are applied when device requests authentication token
	- changes the local state of the device
	- requires organizational account to sign in to the device
	- Administrators can use MDM or Microsoft Intune
		- can enfore configurations like
			- requiring encryption
			- password complexity
			- software installation
			- software updates

- On prem Active Directory joined devices
	- Devices never read Azure AD config because they reach out to on-prem AD for authentication
	- Doesn't work because the requirement is that they need to log in with the company Azure AD credentials

### FIDO

- Fast Identity Online
- authentication without username or password
- Uses external security key
	- typically usb device
	- or platform key built into device
- solution when employees are not willing to use their phone to authenticate

### SSPR

SSPR::Self Service Password Reset
ID: 1670771143863


Which license is required for SSPR?::Azure AD Premium 1 or higher.
ID: 1670771143866

## administrative units

A container within a tenant. 

Say you have a branch office. you create an AU for that office. You add all the managers and the employees to that AU. 

Then you assign a branch manager role to the branch manager. Now this manager can perform the actions from the branch manager role to all the users in the administrative unit. 

So this is useful 

## identity

Hybrid identity::A common identity for authentication and authorization to all resources, regardless of location. Merging on-prem AD with Azure AD through Azure AD connect. This makes SSO possible.
ID: 1670957141121


## Conditional Access

- Can be thought of as an if-then statement
- uses conditional access policies

For example, If I want to access the payroll application, Then I need to use MFA.

You can block access from IP ranges or locations.

Common uses:

-   Requiring multi-factor authentication for users with administrative roles
-   Requiring multi-factor authentication for Azure management tasks
-   Blocking sign-ins for users attempting to use legacy authentication protocols
-   Requiring trusted locations for Azure AD Multi-Factor Authentication registration
-   Blocking or granting access from specific locations
-   Blocking risky sign-in behaviors
-   Requiring organization-managed devices for specific applications

Conditional Access::Policies that regulate access to a resource or an app. Thought of as If-Then statements. For example, if I attempt to log in from Russia, I need to use MFA. 
ID: 1670957141133
