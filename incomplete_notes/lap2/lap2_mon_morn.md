# Cloud
A remote, virtual pool of on-demand shared resources
- Cloud computing is based on virtualisation
	+ *Virtualisation* is the using of different OS on the same hardware (like virtual machines)
	

Types of services:
- Compute
	+ Process workload
		* AWS EC2, GCP compute angine

- Storage -> For storing 'heavier data' which gets linked and referenced for quick access
	+ Store data and assets
		* AWS S3, GCP Google storage 
	
- Database -> for storing ordered data and quickly access
	+ Structure sets of data
		* AWS dynamo DB, GCP cloud spanner

- Network
	+ Figure how resources connect
		* AWS elastic load balancer, GCP cloud load balancing
		


# System Architecture
Conceptual model for software to structure and access pieces of information
*A system architecture is the conceptual model that defines the structure, behavior, and more views of a system.*

You have to think about:
- Languages used
- Database engine used
- Where should database be stored
- Where will the client be hosting
- What APIs will you be interacting with
- If any sensitive data, how are you protecting


---

## C4 model for structuring architecture
- Context
	+ Includes users preferences, workframe, actions/functions required
	+ Send users alerts/users

- Containers
	+ Implementation of context
	+ Separate all the different functions in 'containers' eg web/mobile app - api - database etc
	
- Components
	+ Break down of containers
	+ Eg for using account you need to authenticate, if you want to reset password need to follow specific process etc


- Code
	+ Once everything is set move into coding and building components to be enclosed in containers to present to the user (at context level)

---

## Scaling

- Issues
	+ Compatibility across platforms
	+ Computing/connection power


### The cube
- X axis
	+ horizontal duplication
		* scale by cloning 
		* Clones the full code base share workload as distributed by a load balancer

- Y axis (increasing power)
	+ functional decomposition
		* scale by splitting different things
			- a service just for autenthication-payment etc
		* The product features are split over multiple services that can run independently and are very good at one single thing

- Z axis
	+ data partitioning 
		* scale by splitting similar things
		* Clones the full code base, manage the workload between them but each clone handles just one subset of data
		

### Approaching structures
Microservices vs monoliths
(monoliths have everything grouped, microservices split by functionality)
Microservices structure is better as if anything goes wrong, not all website is down 



## Layering
Approach to split applications in manageable parts:
- Presentation
- Application
- Database


### Design patterns
- *MVC* pattern -> Model - View - Controller
	+ Model is business logic (bottles)
	+ View is the presentation logic (menu)
	+ Controller is the application logic (bartender)


- *FLUX* -> the workflow only moves in one direction (easier to debug - but many storage units so increases complexity)
	+ Dispatcher
		* Is the controller of website
	+ Store
		* (multiple stores)
	+ View (show user)
	+ Action (input of user )

- *REDUX*
	+ Action
	+ Reducer
	+ Store
		* (centralised and single storer)
	+ View













---

# DataBases
Organises data storage (read-write-search-modify-delete data)

## Relational databases
Like in SQL: 
- Tables connect information and store data
- Each table require a unique ID


## Non-relational databases
JSON files -> very likely you're working with JS (JSON -> JS object notation)


## Considerations
- Schemas
	+ Familiarity
	+ Query requirements
	+ Attributes
	+ Grouping

- Scaling
	+ Horizontal (could also clone things in non-relational and give different access points)
	+ Vertical (only way to scale relational eg. increase power)

- Constraints
	+ Restrictions (user can have so many posts etc)
	+ Types (string, integer etc)
	+ Requirement (age has to be integer, > 1 etc)


## Security workflow
For storing data etc
`https://www.dataversity.net/a-brief-history-of-non-relational-databases/#:~:text=The%20acronym%20NoSQL%20was%20first,referred%20to%20as%20SQL%20systems`

*ACID*
- Atomicity
	+ If any transaction fails, you roll back don't force
- Consistency 
	+ Consistently moving from point a to point b
- Isolation 
	+ Each transaction should not interact with one another
- Durability
	+ Once transaction has been made, you want it to persist


*GDPR* (obsolete workflow)-> there's new version (Its like a european workflow)
- Transparency
- Data minimisation
- Accuracy
- More!! ...







