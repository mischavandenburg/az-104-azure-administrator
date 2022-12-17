# Gateway Transit
Links: 

https://learn.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-peering-gateway-transit

In a hub and spoke model, the spokes can use the gateway of the hub. 

For example, if I have a s2s gateway in the hub, the spokes can use the hub gateway and they don't need their own gateway. 

They do need to have a peering with the hub Vnet, obviously. 

The hub Vnet needs the "-AllowGateWayTransit" property

The spokes need the "-UseRemoteGateways"

```powershell
$SpokeRG = "SpokeRG1"
$SpokeRM = "Spoke-RM"
$HubRG   = "HubRG1"
$HubRM   = "Hub-RM"

$spokermvnet = Get-AzVirtualNetwork -Name $SpokeRM -ResourceGroup $SpokeRG
$hubrmvnet   = Get-AzVirtualNetwork -Name $HubRM -ResourceGroup $HubRG

Add-AzVirtualNetworkPeering `
  -Name SpokeRMtoHubRM `
  -VirtualNetwork $spokermvnet `
  -RemoteVirtualNetworkId $hubrmvnet.Id `
  -UseRemoteGateways

Add-AzVirtualNetworkPeering `
  -Name HubRMToSpokeRM `
  -VirtualNetwork $hubrmvnet `
  -RemoteVirtualNetworkId $spokermvnet.Id `
  -AllowGatewayTransit

```

>[!warning]
>You need to allow traffic forwarding in all VNets for this to work



