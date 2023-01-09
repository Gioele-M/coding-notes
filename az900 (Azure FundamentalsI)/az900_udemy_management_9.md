# Pricing Calculator
The pricing calculator is a service to estimate charges based on the desired services


# TCO (Total Cost of Ownership) Calculator
Estimator of costs savings you can realise by migrating your workloads to azure


# Azure cost management
It's a dashboard that gives you a breakdown of what you're paying for. You can get an Azure usage file to get a CSV with a breakdown of everything

You can also create a budget in _Subscriptions_, which once it goes over the pre-defined amount (spent or estimated) triggers an action (ex send an email)

You can get support in case of billing/service problem, there are tiers
- Basic 
	+ Free
- Developer
- Standard
- Professional direct
Obviously the higher you go, the higher you pay, the better service you get


*Service level agreement*
Guaranteed uptime for your services, if they don't respect it, they refund the money
(Services need to offer guaranteed SLA) -> they get compounted by multiplying the SLAs (ex 99.99% • 99.99% = 99.95%)



# Services in preview
Services can be
- Available
- Preview (no SLA)
	+ Private
	+ Public
- In development





# Microsoft Cloud Adoption Framework
A framework to help companies to adopt Azure as a Cloud Computing Platform



# Service Trust portal
Shows all guidelines and regulations Microsoft adopts to protect your data (basicallly the contract you sign by using their products)




# Resource Tags
Useful for:
- Basics
	+ Add metadata
- Application
	+ Tags can be applied to resources, resources groups and subscriptions
- Costing
	+ Tags help when filtering costs
- Inheritance
	+ If a tag is applied at a resource group level, it is not applied to the resources in the same group (as implicit)




# Management Groups
In case companies have different departments with different subscriptions, management groups help you group these subscriptions for easier management
By default there is one 'admin' group called Tenant group





# Azure Blueprints
Used to orchestrate the deployment of artifacts to Azure
It has:
- ARM templates 
	+ JSON, YML files for setup
- Azure policies
- Resource groups
- Role-based access control

So that all of this can be assigned to a new subscription
(There are templates but you can do it yourself)




# Azure Policy
It is a service that offers a 'compliance dashboard'. It shows if there are any issues with your resources (for the Policies you or Azure have established (ex is this VM in an authorised location))





# Azure Resource Locks
Resource locks is a prevention form to avoid accidentally modifying/deleting resources




# Azure CLI
Used to automate tools and resources through pipelines
(esasier)



# Azure PowerShell and CloudShell
Basically the same as Azure CLI but more complex and complete for advanced operations
You can also use it from the browser using CloudShell straight on the Azure site

*ARM template*
(Azure Resource Manager Template)
Templates used for automating deployment of resources (JSON format)
*You can also use Bicep which is easier*





# Azure Arc Service
Used to manage hybrid environments




# Azure Advisor
Reccomendation service, gives insight into
- Cost 
- Security
- Reliability
- Operational excellence
- Performance


# Azure Service Health
Shows any issues that come up in any of the Azure infrastructure, to understand how it might affect you




# Azure Monitor Service
A dashboard for service usage (CPU, network utilissation), you also get alerts or perform actions based on different thresholds
You get to monitor all of your services




# Log Analytics workspace
Used to have a centralised system to send performance and log data from services, and visualise it in a dashboard



# Application insight
When deploying an app, if you chose to add options for monitoring, it adds an extra service to check the insights of your web app
You get different metrics such as traffic and performance 




# Azure Government
Specific Azure services for governments with extra level of compliance





















































