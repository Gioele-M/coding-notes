# Azure databases

*Azure SQL Database*
Platform as a Service (PaaS)

To set up you can:
- Setup a VM and install the Microsoft SQL Server there
	+ You have to manage it yourself
- Setup Azure SQL database
	+ Completely managed by microsoft



*Can also do with PostgreSQL (from the marketplace)*
Best used with azure Data Studio, with the extension for PostgreSQL


*Pricing based on*
- DTU (Database transaction uints)
- CPU-Memory
- Different Tiers for DTU
	+ Basic - Standard - Premium
	+ vCore-based model (indipendently scale compute and storage and can use own SQL license)




*Azure Cosmos DB*
Fully managed NoSQL database which scales on demand automatically

You can pick different database logic such as:
- SQL API
- MongoDb
- Gremlin
- Cassandra
- Table

You need a Database Account, which has in it Databases, having Containers, with inside Items




*Azure SQL vs Cosmos DB*
Azure SQL for:
- Need relationship between tables and need stuff like foreign key constraints
Cosmos DB:
- Flexible schema
- No need of joins between data structures



*Azure Databricks*
Useful for using spark based clusters and interactive notebooks to visualise and analyse the data. Get the service on the markeplace





*Azure Synapse Analytics*
- Helps host your data warehouses and get insight into hosted data
- Can use Spark for Big data needs
- Can also use pipelines for data integration




























