
# Azure virtual network
Virtual networks have IP address, on which the linked VMs IPs are based. You can define subnets and insert VMs into subnets

Defaults to the region of the resource group assigned


*Communication across VMs in a virtual network*
By default communication in different subnets is allowed, so we can make requests to different VMs using the PRIVATE IP address

To access public IP via browser you need to enable port 80 (HTTP)!



*Network security groups*
Created by default when we create a VM, it gets attached to a network interface which is then mounted on the VM
This is a firewall to filter malicious traffic, most basic option
Filters data from and to azure resources
You can have Network Security Groups for machines or subnets, mainly works in terms of settings

Rules can be changed in the Networking setting, just add inbound/outbound security rules
You can change accessibilitu be based on IP addresses, or by Application Security Groups for ease



*If the VMs don't have a public IP address and customers want to connect to it.*
You can establish a Point-To-Site VPN connection, so that machines can be accessed under the _Azure virtual network gateway_ (basically for accessibility)
Another option is Azure ExpressRoute (allows to connect to on-premises networks to microsoft cloud over a private connection, it's more reliable and faster)


*Load balancing*
Distributes incoming traffic
Basic load balancer is free, but does not give 100% certainty (they don't take resposibility)
Standard load balancer has hourly charge but microsoft takes responsibility. You can set a Health probe (to monitor the machines) and Load Balancing rules, used to direct incoming traffic

_Requirement:_
Machines have to be part of an Availability Set or a Scale Set

Azure load balancer can also create a public IP as 'landing ip' for all machines

NAT rules to direct traffic to backend instances


*Azure DNS*
'Domain Name System service'
Pick your own domain name. You need to buy the domain name first.
Search for DNS zone in the marketplacem and set it up based on the resource group. You need to add a Record Set, and add the public IP address of the load balancer (pretty much redirects from your domain name to the public IP so that they get redirected properly)



*Virtual network peering*
By default networks are isolated, if you want them to communicate you need to connect them using the VM settings, go to Peering-add-<virtual machine you want to connect to>




*Virtual network*
- Isolated
	+ Isolated network in Azure cloud
- Managed
	+ You don't need infrastructure
- Resources
	+ You can place resources such as VMs
- Internet
	+ By default all resources can communicate outbound with the internet


*Security groups*
- Used to filter network traffic in Azure virtual network
- You define different rules for inbound and outbound traffic
- For each rule specify source, destination of traffic, port and protocol


*Application security groups*
- When you want to apply network filtering rules for a groups of machines 

*Connectivity options*
- Azure VPN gateway
	+ Used to send encripted traffic between Azure virtual network and on premises location
- Pint-to-Site VPN
	+ Creates secure connection from the Azure virtual network to an individual
- Site-to-Site VPN
	+ Provides connectivity between an on-premises network and an Azure virtual network


*Load balancer*
- Used to distribute incoming traffic
- It can be public or private (private is to manage traffic inside a virtual network)
- You have standard or basic tariff 
	+ Basic is free but is your responsibility
	+ Standard is not but better management











