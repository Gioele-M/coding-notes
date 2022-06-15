# Internet protocols and API designs
Standard procedure to obtain a reproducible result

HTTP is the protocol of interaction between the client and the server - client makes request - server responds
Hyper Text Transfer Protocol (Secure)



# Requests types
- SOAP
	+ Simple Object Access Protocol
	+ Strict protocol
	+ XML-type request
		Requires:
		* Envelope
		Encapsulaltes:
			- Header
				+ Info 
			- Body
				+ Main content
			- Fault
				+ Errors return


- REST
	+ Rapresentational State Transfer
	+ Architectural style (less strict)
		* Can return multiple file style as response (SOAP only XML)
		* Requests are lighter and request less internet strenght
	+ Requires:
		* HTTP verb
			- CRUD routes eg. get - post - patch - delete
		* Headers
			- Info about the request
		* Path 
			- Where we're accepting data from 
		* Body
			- Content of the request aka payload


### Design considerations
- Endpoints
- Params & pagination
- Status codes
- Documentation



## URL components
- Unique identifier (ex city glasgow)
- Resource (listing of indexes)
- Index (element of list)
http:localhost/cities/glasgow/attractions/7







---


# Performance

Installing only packages you use
Specify what packages are developer only


Choose optimal placement of server


Refactoring code greatly increases performance

Removing all white spaces and return characters
	- Helps by reducing required allowance for storing code
	

Images no good
- gif png best for web 
- Store with bse-64 (binary format)


SASS LESS for CSS preprocessing 



# Test performance
- Google PageSpeed Insights!!
	+ Chrome extension 
		* LightHouse
		

