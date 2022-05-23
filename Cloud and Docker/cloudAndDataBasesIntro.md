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

---

*extra - connect to AWS*
- Sign up to AWS
- Services -> compute EC2
- Launch instance
- Add settings template (if any)
	+ Add key pair
- Connect to local machine
	+ `ssh -i <filename> ubuntu@<ip_address>` -> file constaining SSH identification
	+ `exit` -> leaves cloud
- Shut cloud in AWS
	+ Instances -> select instance -> shut down

---	


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
- Should you be worried about future scaling


---

## C4 model for structuring architecture
*https://c4model.com* -> All diagrams and more
The C4 model is an "abstraction-first" approach to diagramming software architecture, based upon abstractions that reflect how software architects and developers think about and build software. The small set of abstractions and diagram types makes the C4 model easy to learn and use. Please note that you don't need to use all 4 levels of diagram; only those that add value - the System Context and Container diagrams are sufficient for many software development teams.

- Context
	+ Includes users preferences, workframe, actions/functions required
	+ Send users alerts/users

- Containers
	+ Implementation of context
	+ Separates all the different functions in 'containers' eg web/mobile app - api - database etc
	
- Components
	+ Break down of containers
		* Eg for using account you need to authenticate, if you want to reset password need to follow specific process etc

- Code
	+ Once everything is set move into coding and building components to be enclosed in containers to present to the user (at context level)

---

## Scaling
*resource - the art of scalability*
http://theartofscalability.com

- Issues with scalability
	+ Compatibility across platforms
	+ Computing/connection power


## The cube -> Scaling principles
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
		

---

### Approaching structures
Microservices vs monoliths
(monoliths have everything grouped, microservices split by functionality)

- A monolithic application keeps the entire code in one big code base. It is a good way to get prototypes up and running and can be suitable for start-ups who need something quick and functional.

- Indepedent microservices are very good at one thing and can talk to each other using API calls. In a product team, each service can have it’s own subteam and should be able to be deployed separately from other services thus providing shorter test and iteration times. A distributed system such as this can, however, bring it's own issues - what if one microservice is down? Is there a fallback process? Examples of naming/responsibilities of microservices might be: 'checkout', 'shipping', 'account'.
	+ Microservices structure is better for larger projects as if anything goes wrong, not all website is down 



### Layering
Approach to split applications in multiple services therefore more manageable parts:
- Presentation -> where everything someone sees happen
- Application -> where all of the processing and thinking happen
- Database -> where all of our data is stored


---

## Design patterns
- *MVC* pattern -> Model - View - Controller
	+ Model - where all data and business logic are handled (bottles)
	+ View is the presentation logic eg GUI and product interaction (menu)
	+ Controller is the application logic and handler of _model_ and _view_ (bartender)


- *FLUX* -> the workflow only moves in one direction (easier to debug - but many storage units so increases complexity)
	+ Dispatcher
		* Is the controller of website, processes actions and updates accordingly
	+ Store
		* Where data is kept
	+ View 
		* What is presented to user, continually refreshes presented data taking it from store
	+ Action 
		* Small objects representing data tokens inputed by users (eg click-form-etc)
		

- *REDUX*
Redux was designed to simplify Flux to have one centralized store and a reducer responsible for all business logic thus making debugging a smoother process.
	+ Action
	+ Reducer
	+ Store
		* (centralised and single storer)
	+ View














---

# DataBases
A database is a collection of structured information generally held and accessed electronically
Organises data storage (read-write-search-modify-delete data) (CRUD routes)

## Relational databases
A relational database can define set categories, put the data into established tables and define the relationship between them
_Best for storing structured data_
Like in SQL: (SQL is the language)
- Tables connect information and store data
- Each table require a unique ID called _key_
- Headers are known as _attributes_
- Rows are known as _records_

With a relational database we can place constraints on an attribute to restrict the type of data that we are storing and the data is optional or not.



## Non-relational databases
A common type of non-relational database is a document data store, which looks a lot like a JavaScript object, typically stored in JSON files (JS object notation). Often referred as NoSQL
_Best for hierarchical unstructured data storage_

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
	+ Consistently moving from point a to point b (being a and b valid 'points')
- Isolation 
	+ Each transaction should not interact with one another
- Durability
	+ Once transaction has been made, you want it to persist (no missing data allowed)


*GDPR* (obsolete workflow)-> there's new version (Its like a european workflow)
- Transparency
- Data minimisation
- Accuracy
- More!! ...








