## A guide to MySQL commands/syntax
- **root-password**: admin

### [MySQL cheetsheet](https://devhints.io/mysql)
**Note**: For reference, prefer ***Company database*** from **[[companydb]]**

### Basic data types
- **INT** - whole numbers
- **DECIMAL (M,N)** - deciaml numbers. (**M** is total number of digits to store, **N** is total number of digits to store after decimal )
- **VARCHAR (l)** - string of text of length l
- **BLOB** - Binary Large Object, stores large data (e.g.images)
- **DATE** - 'YYYY-MM-DD'
- **TIMESTAMP** - 'YYYY-MM-DD HH:MM:SS'

### Comments in SQL
`--` before any statements makes it a comment.

### Basic commands

#### Database
##### Show existing databases
`show databases;`
##### Create a database
`create database <databasename>;`
##### Check active database
`select database();`
##### Delete existing database
`drop <databasename>;`

#### Tables
##### Create table
- `not null key` must have a value while inserting an entry.
- `unique key` must have a unique non-repeating value while inserting an entry.
- `primary key` is combination of `unique` & `not null` keys.
- `default <value>` denotes that if value for this attribute is not mentioned then default given value will be accepted as default.
```syntax-1
create table <tablename> (
	<attrib 1> <data_type>,
	<attrib 2> <data_type>,
	<attrib 3> <data_type>,
	key(<attrib 1>),
	key(<attrib 2>)
);
```

```example-1:
create table students(
	student_id INT,
	name varchar(20),
	subject varchar(20),
	primary key(student_id)
);
```

```syntax-2
create table <tablename> (
	<attrib 1> <data_type> key,
	<attrib 2> <data_type> key,
	<attrib 3> <data_type> key
);
```

```example-2:
create table students(
	student_id INT primary key,
	name varchar(20) not null,
	subject varchar(20) default 'undecided'
);
```

##### Show existing tables 
`show tables;`

##### Get description of a table
`desc <tablename>;`
`describe <tablename>;`

##### Delete existing table
`drop table <tablename>;
`
##### Altering a table
- **Alter**
	- **Add a column** `alter table <tablename> add <columnname> <datatype>;`
	- **Add a column after existing column** `alter table <tablename> add <attrib 1> <datatype> after <attrib 2>`
	- **Delete a column** `alter table <tablename> drop column <columnname>;`
	- **Change column name** `alter table <tablename> rename <prevname> to <newname>;`
	- **Modify a column** `alter table <tablename> modify <columnname> <datatype>;`
	- **Modify a column with key** `alter table <tablename> modify <columnname> <datatype> <keytype>;`
	- **Droppign an existing key** `alter table child drop foreign key <attribute>`;`
	- **Add default field** `alter table <tablename> alter <columnname> set default <value>;`
	- **Drop default field** `alter table <tablename> alter <columnname> drop default;`
- **Change order**
	- **Moving a field to first position** `alter table <tablename> modify <columnname> <datatype> first;`

##### Inserting data to a table
`insert into <tablename> values(<value>, <value>, <value>);`

##### Inserting data to specific columns
- unmentioned values will be shown as null in the table
`insert into <tablename> (<attrib 1>,<attrib 2>) values(<value>,<value>);`

##### Inserting multiple entries in same linee
```
insert into 
	<tablename>(<attrib 1>,<attrib 2>) 
values 
	(<value>,<value>),
	(<value>,<value>),
	(<value>,<value>);
```

##### Auto increment specifier
- auto_increment is used when value of an attribute is uniformly increasing.
- default step size is 1.
- starting value can be changed by using `alter table` query.
- `auto_increment` attribute must be a key and should exist only once in a table.
```example-3
create table students(
	id int auto_increment,
	name varchar(20)
);
alter table students auto_increment=45;
insert into students(name) values('abc'),('efg'),('pqr');
```

##### Update entries
- this updates all the entries in a table with given value
- to update specific entry, we use `where` clause
`update <tablename> set <attribute>=<value>;`
- multiple attributes can also be updated
`update <tablename> set <attrib 1>=<value>, <attrib 2>=<value>;`

##### Delete entries
- delete all entries
-  to delete specific entry, we use `where` clause
- we cannot delete a specific attribute from an entry like update method, whole entry will be deleted. To delete a specific attribute, set it to `null`.
`delete from <tablename>;`


##### WHERE clause
- for updating a specific entry/entries based on given condition
- comparison operators:
	- `=` equals
	- `<>` not equals
	- `>` greater than
	- `<` less than
	- `>=` greater than or equal to
	- `<=` less than equal to
```
update <tablename>
set <attrib 1>=<value>
where <attrib 2>=<value>;
```
```
delete from <tablename>
where <attrib 1>=<value>;
```
##### OR clause
- to combine 2 conditions used in `where` clause.
- value of condition is true if any one of the 2 condition is true.
```
update <tablename>
set <attrib 1>=<value>
where <attrib 2>=<value> or <attrib 3>=<value>;
```
```
delete <tablename>
where <attrib 2>=<value> or <attrib 3>=<value>;
```

##### AND clause
- to combine 2 conditions used in `where` clause.
- value of condition is true if both of the 2 conditions are true.
```
update <tablename>
set <attrib 1>=<value>
where <attrib 2>=<value> and <attrib 3>=<value>;
```
```
delete <tablename>
where <attrib 2>=<value> and <attrib 3>=<value>;
```

##### IN clause
- for checking an attribute is present in a given list
```
select * from <tablename>
where <attrib 1> in (value1, value2, value3);
```

##### Emptying/truncating the table
`truncate <tablename>;`

#### Views
##### View all entries
`select * from <tablename>;`
##### View all entries from multiple tables
`select * from <tablename1>,<tablename2>;`
##### View specific attibutes froma table
`select <attrib 1>,<attrib 2> from <tablename>;`
##### View specific attribute as desired column name
`select <attrib 1> as <your_attribute_name> from <tablename>;`
`select <attrib 1> as <your_attribute_name>, <attrib 2> as <your_attribute_name> from <tablename>;`
##### View distinct entries of an attribute
`select distinct <attribute> from <tablename>`
##### Order fetched entries
- `asc` for ascending, `desc` for decending.
- default order for ordering is ascending.
`select <attrib 1>,<attrib 2>,<attrib 3> from <tablename> order by <attrib 1>;`
`select <attrib 1>,<attrib 2>,<attrib 3> from <tablename> order by <attrib 1> desc;`
- `order by` for multiple attributes follows priority. First mentioned attribute is ordered first then second and so on. like order by first letter then second...
`select <attrib 1>,<attrib 2>, <attrib 3> from <tablename> order by <attrib 1>, <attrib 2> ;`
##### Limit the fetched entries
``` syntax
select * from <tablename>
order by <attrib 1> desc
limit <count>
```
``` example
select * from students
order by st_id desc
limit 3
```

#### Functions
- Functions help us simplify complex functionalities using queries
##### Count
`select count(<attribute>) from <tablename>;`
##### Average
`select avg(<attribute>) from <tablename>;`
##### Sum
`select sum(<attribute>) from <tablename>;`

#### Wildcards
- Wildcards are used to match results as per tricky requirements. e.g. matching a letter or substring in a record attribute
- **Like** is a special keyword used with wildcards
- **%** denotes any # characters
`select * from <tablename> where <attribute> like '%ing'` will return entry whose ending is like 'ing'.
`select * from <tablename> where <attribute> like 's%'` will return entry whose start with 's'.
`select * from <tablename> where <attribute> like '%on%'` will return entry containing 'on' in it.
- \_ denotes one character
`select * from <tablename> where <attribute> like '_.org'` returns entry with one starting missing letter and '.org' next to it.
`select * from <tablename> where <attribute> like '____.org'` returns entry with four (four underscores '\_') starting missing letter and '.org' next to it.

#### Union
- A useful functionality to combine results of multiple ***select*** statements into one
```
select <attrib 1> from <table 1>
union
select <attrib 1> from <table 2>
```
- **Removing anamolies**
	- In case of similar columns in two different tables, it might get confusing which column belongs to which table. To avoid this we use **dot operator**
```
select <attrib 1>, <table 1>.<attrib x> from <table 1>
union
select <attrib 1>, <table 2>.<attrib x> from <table 2>
```

#### Joins
- MySQL combines two or more rows in different tables on basis of a common column using **joins**.
- **Types** :
1. INNER JOIN
```
select <attrib 1>, <attrib 2>, <attrib 3> from <table 1>
join <table 2>
on <table 1>.<common_column> = <table 2>.<common_column>
```
2. LEFT JOIN
```
select <attrib 1>, <attrib 2>, <attrib 3> from <table 1>
left join <table 2>
on <table 1>.<common_column> = <table 2>.<common_column>
```
3. RIGHT JOIN
```
select <attrib 1>, <attrib 2>, <attrib 3> from <table 1>
right join <table 2>
on <table 1>.<common_column> = <table 2>.<common_column>
```
4. CROSS JOIN
```
select <attrib 1>, <attrib 2>, <attrib 3> from <table 1>
join <table 2>
on <table 1>.<common_column> = <table 2>.<common_column>
```

![[joins.png]]

#### Nested Queries
- Nested queries are used to use results of a select statement as information for another select statement.
- The inner statement is executed first then outer statements are executed.
- e.g. find names of all employees who have - sold over 30k to a single client
```
select employee.first_name, employee.last_name from employee where employee.emp_id in
(
select works_with.emp_id from works_with where works_with.total_sales > 30000
)
```
```
select client.client_name from client where .branch_id = 
(
select branch.brand_id from branch where branch.mgr_id = 102 limit 1
)
```

#### On Delete
- **on delete** is a reference option where effect of deletion on related tables is decided.
- **Reference options**:
1. **ON DELETE CASCADE**: delete complete row of child table when parent's record for related attribute is deleted is deleted
1. **ON DELETE SET NULL**: sets related attribute in child as ***null*** when parent's record is deleted

#### Triggers
- A database object associated with a table.
- It is activated when a defined action is executed for the table.
- Trigger can be executed when ISERT,DELETE,UPDATE statements are run.
- Can be used to automate some actions on above query execution.
- example 1: add first_name to a new table when a new employee  is added
```
>> DELIMETER $$
CREATE 
	TRIGGER my_trigger BEFORE INSERT 
	ON employee 
	FOR EACH ROW BEGIN 
		INSERT INTO trigger_table VALUES(NEW.first_name);
	END$$
>> DELIMETER;
```
- example 2: conditionals
```
>> DELIMETER $$
CREATE
	TRIGGER my_trigger BEFORE INSERT
	ON employee
	FOR EACH ROW BEGIN
		IF NEW.sex = 'M' THEN
			INSERT INTO trigger_test VALUES('added male employee');
		ELSEIF NEW.sex = 'F' THEN
			INSERT INTO trigger_test VALUES('added female employee');
		ELSE
			INSERT INTO trigger_test VALUES('added other employee');
		END iF;
	END$$
>> DELIMETER;
```
- The trigger can be dropped by `DROP TRIGGER <trigger_name>`