## Database Management Systems (DBMS)
## What is DBMS?
Database Management Systems (DBMS) are software systems that store, retrieve, and run queries on data. A DBMS serves as an interface between an end-user and a database, allowing users to create, read, update, and delete data in the database.

Database management systems can be classified based on various criteria such as the data model, the database distribution, or user numbers. The most widely used types of DBMS software are relational, distributed, hierarchical, object-oriented, and network.

### Distributed database management system

A distributed DBMS is a set of logically interrelated databases distributed over a network that is managed by a centralized database application. This DBMS synchronizes data periodically and ensures that any change to data is universally updated in the database.

DDBMS is well-suited for applications that require:

**High availability**

The database and its data are available at all times

**Data resilience**

The database is resilient against failures and can adapt to growing data and user demands

**Geographical data distribution**

Data is stored closer to where it's needed, reducing the amount of data that needs to be transferred over the network

**Scalability**

The database can scale beyond the processing capacity of any single machine 


### Hierarchical database management system

Hierarchical databases organize model data in a tree-like structure. Data storage is either a top-down or bottom-up format and is represented using a parent-child relationship.

Hierarchical databases are well-suited for applications that require highly structured and organized data, such as inventory management systems or financial record-keeping. 

### Network database management system

The network database model addresses the need for more complex relationships by allowing each child to have multiple parents. Entities are organized in a graph that can be accessed through several paths.

### Relational database management system

Relational database management systems (RDBMS) are the **most popular data model** because of their user-friendly interface. It is based on normalizing data in the rows and columns of the tables. This is a viable option when you need a data storage system that is scalable, flexible, and able to manage lots of information.

### Object-oriented database management system

Object-oriented models store data in objects instead of rows and columns. It is based on object-oriented programming (OOP) that allows objects to have members such as fields, properties, and methods.

### Some of The Popular DBMS

Here are some of the most popular database management systems:

**Oracle**

Oracle Database is a commercial relational database management system. It utilizes enterprise-scale database technology with a robust set of features right out of the box. It can be stored in the cloud or on-premises.

**MySQL**

MySQL is a relational database management system that is commonly used with open-source content management systems and large platforms like Facebook, Twitter, and YouTube.

**SQL Server**

Developed by Microsoft, SQL Server is a relational database management system built on top of structured query language (SQL), a standardized programming language that allows database administrators to manage databases and query data.

### Difference Between Rational DBMS and NoSQL

**1. Data Model:**

- RDBMSs enforce a rigid structure with predefined schemas in tables.
- NoSQL offers more flexibility with various data models like document-oriented, key-value, and graph.
  
**2. Schema Flexibility:**

- RDBMSs demand a predefined, fixed schema, posing challenges for evolving data.
- NoSQL allows for dynamic schema changes and embraces unstructured or semi-structured data.

**3. Scalability:**

- RDBMSs traditionally struggle with horizontal scaling for massive data volumes.
- NoSQL excels in horizontal scalability, making it perfect for distributed architectures.

**4. Query Language:**

- RDBMSs leverage SQL (Structured Query Language) for powerful querying with complex joins and aggregations.

- NoSQL may have its query languages, sometimes with limitations compared to SQL.

**5. ACID Compliance:**

- RDBMSs guarantee ACID properties for transactions.
- NoSQL compliance varies, with some sacrificing certain ACID aspects for better scalability and performance.
