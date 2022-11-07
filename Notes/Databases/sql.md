## A guide to SQL (structured query language)

- **root-password**: admin

### What is a database?
- Any collection of related information e.g.phonebook
- Can be stored in different ways e.g.computer system

### Database management system (DBMS)
- A special software program to create and maintain a database
	- Easy to handle large data
	- Security
	- Backups
	- Import/export data
	- Concurrency
	- Interaction with programming languages
	
### C.R.U.D operations
- **Create**
- **Retrieve**
- **Update**
- **Delete**

### SQL (Structured Query Language)
- A standardized language for interacting with RDBMS
- Used to define tables & structures
- Used to perform **CRUD** operations as well administrative tasks (e.g. user management, security, backup etc.)
- SQL code used on a RDBMS is not always portable to another without slight modifications
- SQL is hybrid of 4 types of languages
	- **Data Query (DQL)**: to query database information and retrieve stored information
	- **Data Definition (DDL)**: to define database schema
	- **Data Control (DCL)**: to control access to stored data, user & permission management
	- **Data Manipulation (DML)**: to perform insert, delete operations


### What are database queries ?
- A **request**/**set of instructions** made to dbms for specific information
- more the complexity of data, difficult to retrieve specific information, hence queries
- examples: a google search

### Types of databases
1. **RELATIONAL (SQL)**
	- RDBMS
	- organizing data into one or more tables
	- each table has rows and columns
	- each row is identified by a unique key (id)
	- examples: [[mySQL|MySQL]], Oracledb, postgreSQL, mariadb
2. **NON RELATIONAL (noSQL/not just SQL)**
	- organizing data in other than a table structure
	- key-value pairs, documents(JSON,XML), graphs, flexible tables
	- no set language standard
	- most NRDBMS implement their own language
	- examples: mongodb, dynamodb, apache cassandra, firebase


### Tables & Keys
- #### Table structure
	- Column indicates a single attribute
	- Row indicates a single entry
	- Special column indicating a primary key attribute of each entry
	- example: [[sql#table 1 0|table 1.0]]

- #### Keys
	- **PRIMARY KEY**
		- An attribute to uniquely find an entry in a database
		- Multiple entries can have similar data but ***Primary key*** attributes for the entries are always unique
		- A table must have a ***Primary key***
		- must be **unique** & **not null**
		- examples: id is a ***Primary key*** & branch_id is a ***Foreign key*** in [[sql#table 1 1|table 1.1]]
	- **FOREIGN KEY**
		- Stores a ***Primary key*** of another table in order to express a relation
		- A table can have both keys at the same time
		- examples: branch_id is a ***Primary key*** & id is ***Foreign key*** in [[sql#table 1 2|table 1.2]]
	- **COMPOSITE KEY**
		- A combination of multiple columns i.e. ***Primary keys*** or ***Foreign keys***
	- **SUPER KEY**
		- A set of **unique** columns(attributes) that represent an entry
	- **ALTERNATE KEY**
		- A table can have multiple  unique attributes. But only one can be chosen as ***Primary key***. Rest are known as ***Alternate keys***
	- **UNIQUE KEY**
		- A unique attribute that represent an entry
		- Can be null unlike ***Primary key***
	- **CANDIDATE KEY**
		- A unique attribute that represent an entry
		- We select a ***Primary key*** from ***Composite keys***

### Tables
###### table 1.0
 |id|name|subject|
 |---|---|---|
 |**001**|Ryan|sub1|
|**002**|Lily|sub2|

###### table 1.1
|***id***|name|subject|_branch_id_|
|---|---|---|---|
|**001**|Ryan|sub1|1|
|**002**|Lily|sub2|2|
|**003**|Jack|sub3|3|
|**004**|Maya|sub2|2|
###### table 1.2
|_branch_id_|branch_name|***id***|
 |---|---|---|
 |**1**|IT|001|
|**2**|COMP|002|
|**3**|ETC|004|
