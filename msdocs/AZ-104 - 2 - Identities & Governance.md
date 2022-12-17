# AZ-104 - 2 - Identities & Governance

# Management Groups

Management groups are a layer above subscription level. Can apply policies and access control on management groups. 

Subscriptions within a management group inherit the conditions applied to the management group

## things to consider when using management groups

- custom hierarchies
- policy inheritance
	- subscriptions inherit policies
	- F. ex. limit all resources to a particular region
- compliance rules
- cost reporting




Management Groups::A layer above subscription level. Access control and policies can be applied to these groups.
ID: 1669451669401


# Azure Policies

Azure Policies::Policies are used to enforce rules on resources to meet compliance standards and SLA's.
ID: 1669478872718

Can be used to standardize how cloud resources are configured. 

Can be applied at scale. 

There are many built in policy definitions, and you can create your own. 

## 4 steps 

1. create policy definition
	1. condition to evaluate & action to perform when condition is met
2. create initiative definition
	1. set of policy definitions to track resource compliance state to meet a larger goal
3. scope the initiative definition
	1. specific management or resource groups, or subscriptions
4. determine compliance

Initiative definition::Set of policy definitions to track resource compliance state to meet a larger goal
ID: 1669478872726


## initiative definition

Examples:

-   **Audit machines with insecure password security settings**
-   **Configure Windows machines to run Azure Monitor Agent and associate them to a Data Collection Rule**:  The definition deploys the Azure Monitor Agent extension and associates the resources with a specified Data Collection Rule. 
-   **ISO 27001:2013**: Use this initiative to apply policies for a subset of ISO 27001:2013 controls. 

# RBAC

RBAC Security Principal::the entity which is requesting access to something
ID: 1669478872731


RBAC role definition::set of permissions that list the allowed operations
ID: 1669478872736


RBAC assignment::an assignment attaches a *role definition* to a *security principal* at a particular *scope*
ID: 1669478872739


## role definition

- actions : what actions are allowed
- NotActions : what actions aren't allowed
- DataActions: how data can be changed or used
- AssignableScopes: list the scopes where the role definition can be assigned

### NotActions
-   `Authorization/*/Delete`: Not authorized to delete or remove for "all."
-   `Authorization/*/Write`: Not authorized to write or change for "all."
-   `Authorization/elevateAccess/Action`: Not authorized to increase the level or scope of access privileges.

The _Contributor_ role also has two _DataActions_ permissions to specify how data can be affected:

-   `"NotDataActions": []`: No specific actions are listed. Therefore, all actions can affect the data.
-   `"AssignableScopes": ["/"]`: The role can be assigned for all scopes that affect data. The backslash `{\}` wildcard means "all."

The contributor role does this: Allow all actions, except write or delete role assignment

RBAC Contributor role permissions::Can create and manage all types of resources, can create a new tenant in AD, but it cannot grant access to others. 
ID: 1670694451641


### things to know about role definition

- Azure RBAC provides built-in roles
- Owner built-in role has highest level of access
- AssignableScopes can be management groups, subscriptions, resource groups, or resources


### role scopes

-   Scope a role as available for assignment in two subscriptions:
    
    `"/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e", "/subscriptions/e91d47c4-76f3-4271-a796-21b4ecfe3624"`
    
-   Scope a role as available for assignment only in the Network resource group:
    
    `"/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/resourceGroups/Network"`
    
-   Scope a role as available for assignment for all requestors:
    
    `"/"`


## Azure RBAC roles vs Azure AD roles

==These are not the same. 

RBAC manages access to Azure resources, AD manages access to Azure AD resources. 

RBAC scope has multiple levels, such as management groups or subscriptions. AD is scoped at tenant level. 

RBAC roles can be defined by azure portal, cli, powershell, ARM Templates or REST API. AD roles: azure admin portal, 365 admin portal etc. 

Azure RBAC roles vs Azure AD roles::Azure AD admin roles manage users, groups and domains in Azure AD.  Azure RBAC roles provide more granular management for resources at multiple levels such as root, management groups, resource groups and resources.
ID: 1669478872743


