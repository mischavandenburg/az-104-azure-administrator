## Resource Groups

Resource Groups are a logical collection of resources.
Resource groups store metadata of resources. 

-   Resources can only exist in one resource group.
-   Resource Groups cannot be renamed.
-   Resource Groups can have resources of many different types (services).
-   Resource Groups can have resources from many different regions.

-   All the resources in your group should share the same lifecycle. You deploy, update, and delete them together. If one resource, such as a database server, needs to exist on a different deployment cycle it should be in another resource group.
-   Each resource can only exist in one resource group.
-   A resource group can contain resources that reside in different regions.

Deleting a resource group deletes all the resources contained within it.

## arm templates
arm templates are written in json
bicep is an abstaction layer over arm / json, simplified

when you deploy an arm template a second time, existing resources are not changed


