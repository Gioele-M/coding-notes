# Azure architecture and services - Azure compute

For virtual machines
Windows 10/11
MacOS

For servers 
Windows server 2022
Ubuntu server 22.04


*Create a virtual machine*
The virtual machine is dependent on many resources. 
For virtual machines: 
- It gets its own IP. 
- Are connected to a virtual network similar to the home environment. you get:
	+ Virtual machine - Network interface - Disk (Storage) - Network security group - Subnet - IP address range
For servers:
- You get a network
- You can do subnets to separate processes


*To connect to virtual machine*
Connect using the RDP (remote desktop protocol) (can also do the ssh)
- Download RDP file
- Install and use the credential to access the machine



*For starting a server from virtual machine*
You need to use Server Manager, add roles and features, add role (web server) and install the required packages.
To access the server: in azure settings -> networking -> add inbound port rules -> http -> add
! now if we access the IP from browser we see all the virtual machines services
Data stored in the :D drive is at risk of being deleted (by machine reboot or redeploy)
Data in the :C drive does not risk that (but is the system info drive lol)

VM sizes:
- There are different types ex general purpose, compute optimized, memory optimized or storage optimized


*Pricing dependency*
- Software - Memory - Storage
- Region of deployment


*IP ADDRESS*
It could be _static or dynamic_. Static does not change, dynamic does in case the machine gets stopped or re-allocated 

*Choosing a region*
Some services are available at a global level, not VMs
- Make sure the service is available in the region
- Difference in cost depending on the hosting region
- Company might have a restriction based on compliance for specific regions
- Want to ensure the resources are the closest to the users


*In VM we can deploy another VM in the same network with different OS, disk, IP and interface*

*Connect to server with PuTTY*
You have to give IP, username, password and you get a CLI (remember to give networking security access)
In CLI, to install server use
sudo apt-get update
sudo apt-get install nginx




*Deleting resources*
You can by resource and resource group, for some you might need to remove the lock


A resource can be mapped to only 1 resource group.
Resources cannot be part of multiple subscriptions either


*Use the marketplace to use pre-built images with set uses for MVs*


*Availability sets*
It is a logical grouping of all VMs. They come for free.
Useful when there are faults of the system or updates required to apply. It assigns a 'fault domain' and an 'update domain'.
- Update domain
	+ Azure applies updates to the physical infrastructure one update domain at the time
	+ Most likely each machine will have one (so not to limit too many machines at the same time)
- Fault domain
	+ Virtual machines share common power source and network switch 
	+ Ex you have 4 VMs, you can have 2 fault domains, containing 2 machines each. If a fault happens in one of the two, you can still use the other machines

You add it in the Availability options when creating a new Service (can't add it later lol)


*Availability Zones*
Same as sets but in different areas of the world, so to mitigate against datacenter failure. 
Again no cost. 
Cost is in data transfer between machines (per GB)


*Azure dedicated host*
Azure will give an exclusive physical host for you. 
You have hardware isolation and control over the maintenance events that happen on your server



*Virtual machine Scale Set service* (from marketplace)
In case your machines are using more power than it is allocated for it. 
You can deploy the application on a 'SET' of machines, so that the user can access that set. 
In case the demand is higher, you can add MVs to that set.

The Scale Set service manages the number of virtual machines based on the current demand!  

Usually you use also a load balancer

You need to set the scaling yourself (the number of machines u start with, how many to reach and under what usage conditions)





*Azure web apps*
In case we want to host an app on Azure
They manage the server and the MV (you cannot access it), this is _Platform as a Service (PAAS)_
This MV has .NET, java, ruby, node.js, php and python.
This scales up and down on its own.

App service plan is a service for billing which gets instantiated when deploying web app

Application insights is an additional service which is not really required

You get a URL

You can deploy the apps straight from VS Code, you need to access with your account

The web apps can also be scaled, but you need an app service plan that allows it. We can scale-up from the side menu, and we can do based on pre-set metrics


 


*Azure Functions*
In case you want to host just a function code instead a whole web app. (Like an API)
Both infrastructure and VMs are managed for you.
Can also be billed based on consumption of the function or by instance if it has to run continuously.
If on consumption we can pick the triggers.
Supports C#, Java, JS, PowerShell, Python




*Azure Containers*
Done as a Docker container with an image and all. The benefit is to isolate dependencies of apps or softwares running on the same machine, and to be able to easily export the environment.

As part of Azure platform you have _Kuberneets_ to orchestrate containers and dependencies.
The containers are run on nginx image 



*Use virtual desktops if you need VMs*
The pricing changes based on if you have licences for mass distribution.




















































































































































































