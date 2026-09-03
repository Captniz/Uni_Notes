---
Date created: 03-09-26 • 12:02
tags:
  - Web
Related PDF/DOC:
Related Pages:
---
# Database

## Teoria
A database (DB) is a collection of data   
A Database Management System (DBMS) is a software application conceived to store, retrieve, manage data in DBs  

Most of today’s DBs are relational DBs. The key components are: 
- Structure: how data are represented 
- Integrity: putting constraints on data 
- Language: providing ways for accessing / manipulating data  

a **schema**: defining the relation  
`Student(ID: integer; Name: string; Country: string)`

an **instance** of the schema: being the content of the relation at a given time  


There are two classes of constraints: 
- Intra-relational that is constraints involving just one relation: 
	- Domain constraints 
	- Primary key constraints 
- Inter-relational constraints, that is constraints involving more than one relation:
	-  Foreign key constraints  

The Structured Query Language (SQL) is the universal language for accessing relational DBs  


## Java
JDBC is an interface which allows Java code to: 
- Establish a connection with a DBMS 
- Send SQL statements 
- Process results  

The JDBC driver : 
- Implements the JDBC API interfaces for different DBMS
- Executes the methods invoked by the Java application
- Translates these operations into commands specific to the DBMS
- Manages communication with the DBMS
- Receives results from the DBMS and returns them to the application  

Java sql objects :
- Driver Manager: manages the registered JDBC drivers and choose the suitable one
- Connection:  represents an active connection between the application and the DBMS
- Statement: represents an SQL command to be executed on the DBMS
- ResultSet: represents the set of results returned by an SQL query  

![[EMBED/06-Access_To_DB_1.png]]

[[06-Access_To_DB_1.pdf#page=8&rect=178,490,456,673|06-Access_To_DB_1, p.8]]

## Spring
In a web app a DB connection could be opened for each request or for each session  results in a lot of traffic over the network  

Spring uses a **data source** object to efficiently retrieve and manage the connections to minimize the number of unnecessary operations  

A data source is a spring bean that manages connections: 
- provides the web app with connections when it’s requested; 
- creates new connections only when it is necessary;
- closes connections when they are not more neeeded.  

Connection pooling is a strategy to manage and optimize the usage of connections to a DB

1. **Pool creation and inizialization**: at the starting of the web app, the connection pool is created and initialized based on the configuration provided. A few idle connections are pre-created. An idle connection is a connection ready to be used but not in use.
2. **Connection allocation**: when the web app needs a connection, the pool checks if any idle connections are available: if yes, it's allocated to the application. If not, and the maximum number supported by the pool is not reached yet, a new connection is created and allocated. If all the connections supported by the pool are in use, the pool waits for a connection to become available.
3. **Connection Reuse**: After the application finishes using the connection, it returns it to the pool and it is re-used. Connections are closed automatically after a certain period of idle time.  

HikariCP  is the default connection pooling used in Spring Boot  

Spring work on data through beans annotated with `@Repository` and `JDBCTemplates`.

`@Repository` beans: 
- Indicate Spring that they are a data-access component 
- Organize methods to access data 
- Use JDBC Template 

JDBC Template:
- It is a Spring helper class provided as a bean
-  Spring Boot will create it automatically if a DataSource exists 
- Executes SQL queries and statements
-  Manages DBMS connections (obtained from the DataSource) 
- Maps query results to objects  

Summary :
>A request arrives at a Controller, which eventually calls a Service; the Controller/Service calls the Repository, which uses JdbcTemplate; JdbcTemplate obtains a connection from the DataSource, executes the query on the DBMS, and the result is returned back up the chain  

![[EMBED/06-Access_To_DB_1 2.png]]

[[06-Access_To_DB_1.pdf#page=16&rect=111,444,491,730|06-Access_To_DB_1, p.16]]

### H2
H2 is a relational DBMS written in Java and It can works in-memory, embedded, or in client– server mode. Spring Boot automatically configure H2 in in-memory mode

![[EMBED/06-Access_To_DB_1 1.png]]

[[06-Access_To_DB_1.pdf#page=14&rect=126,236,473,331|06-Access_To_DB_1, p.14]]

### Prepared statements
> precompiled SQL statements that can be executed multiple times efficiently, with different parameters. So, they are parametric SQL statements

```java
String sql = "INSERT INTO coffee (brand, price) VALUES (?,?)";  
```

They can be **reused multiple** times with different parameters, reducing the need to re-compile the statement, thus improving performance for frequently executed statements  

They also prevent SQL injections.
Using prepared statements, the SQL engine binds the values to the placeholder (?) before executing the statement. So, the values are treated as data and not as a part of SQL syntax  

