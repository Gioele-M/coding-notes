# Azure Traffic manager
DNS-based traffic load balancer
Can distribute traffic to different regions using various load distributor managers

You have two methods
- Priority routing
	+ You use mainly a server, but if it fails you go to another one
- Weighted routing
	+ You distribute the weight of requests constantly between servers



# Azure Content Delivery Network
Helps deliver content to users across the globe by placing content on physical nodes placed across the world
The server then checks upon request if the node placed closest to the user has the requested file and it sends it to the user if yes. If no it downloads it from the main server, caches it and sends it to the user



# Azure DevOps services
Basically offers a service to smooth version upload deployment, giving different tools for continuous deployment such as:
First create a project!
- Azure Boards
	+ Makes you create a Kanban board with different flags and labels for each note added
- Azure repos
	+ Pretty much is managing your github repository
- Build piplines
	+ Creates a YML file to run your code for deployment. It then prepares the build for the code
- Release pipelines
	+ If you're happy with the result of the build pipeline, you can use release pipelines to deploy it in a continuous manner
	+ This keeps track of the different version stages
- DevTest Labs
	+ Services to create, using, and managing VMs environments for testing the apps. You can pick an image (with OS and software) dedicated for testing



# Machine learning
There is a Machine learning service on Azure Marketplace
You can use by launching it on a studio. You can use 'Designer' to use a pre-built model or create one.
You pretty much have a canvas which shows data interactively and allows you to 'piece together graphically' a pipeline without code

To run a pipeline you need to create an 'experiment' which runs on the Azure servers
It produces descriptive statistics as well as the requested results for each passage




# Cognitive services
You can add AI to your services ex:
- Speech
	+ Speech to text
	+ Text to speech
	+ Translation
- Language
	+ Entity recognition (identify commonly used terms)
- Vision
	+ Recognise elements from images 
- Decision
- OpenAI




# Bot services
Used to build conversational AI bots.
You need to do a bit of code by using the Bot framework SDK in C#, Java, JS and Python
The bot basically echoes pre-defined messages
(!Scam alert, you download it from GitHub)



# Azure Key Valut service
Service to store encription keys, certificates and secrets in general



# Azure logic apps
Provides a platform where you can create and run automated workflows
Don't need too much code
Can trigger a workflow on specific actions
You need to set up:
- Trigger
	+ Ex new blob added to storage account
- Following steps
	+ Ex send email to specific account





# IoT Hub
It is a managed service used as central message hub for bi-directional communication between managed devices and an IoT application
Provides a secure communication channel
IoT devices are stuff with sensors ex humidity, temperature etc




# Azure Batch Service
Used to run large and parallel high-performance computing batch jobs
This service manages a pool of VMs to complete your processes so you don't have to create and use the VMs on your own


# Azure content delivery network
Uses Point of presence (POP) locations to deliver content


































