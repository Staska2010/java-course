JDBC
----

JDBC is a Java API to work with databases. As any other Java specification, JDBC provides a number of interfaces and
rules that every implementation must follow. Working with JDBC looks like this:

- You work with JDBC interfaces and classes that ship with any Java installation. Examples: Connection, Statement, 
ResultSet.
- You add an implementation lib which is called a JDBC Driver (could be mysql, oracle, postgre or any other driver) and
you tell to JDBC that you want to use that implementation. 
- JDBC loads the classes from that driver and while you continue working with Connection, Statement, ResultSet 
interfaces you actually work with the implementations from the driver you chose.

JDBC and databases is one of the most critical and complicated parts of the enterprise apps. There are technologies
built on top of JDBC (ORMs like Hibernate, simpler wrappers like SpringJDBC and MyBatis) but there is no way you can 
understand and use them effectively without understanding JDBC first.

# NoSQL Storage

JDBC is used to access RDBMS's (relational databases) that utilize tables, columns, rows. But there is another group 
of databases - NoSQL DBs. These include:
- Document-oriented DBs that can store unstructured hierarchical data 
- Graph-oriented DBs that can store relations between lots of interconnected objects. 
- ... 

For several years (somewhere around 2012-2015) there was a lot of hubbub around NoSQL storage but eventually it all 
calmed down. Today most companies utilize both traditional RDBMS and NoSQL. So JDBC continues to play a major role in 
most of the modern apps.

# Step 1 - Preparations before working with DB

- Read about DAO (Data Access Object) concept. Create a DAO called InMemoryDogDao.
- Add same static collections defined in your Controllers layer to DAO and create all necessary CRUD (create, 
read, update, delete) methods there
- Use Spring IoC to inject DAO into Controller
- Change the Controller to use DAO instead of its own internal collection and remove the old collection. Make sure that
no tests are failing.

*Tip*: You've probably heard a term "Design Pattern" - this is a broad technique/approach to implement some particular
behaviour. Each of these techniques has to have a name. It simplifies and standardizes the communication as we don't 
need to explain the concept over and over again - we just tell our colleagues "Hey, let's create a DAO for this" and 
they already know what we're talking about. DAO is one of the examples. The problem is - there are many classifications 
and author of Design Patterns:
- Gang of Four (GoF) Design Patterns - are classical patterns that are known and used in every programming environment
be it Java, .Net or Python. Examples: MVC, Singleton, Listener, Proxy.
- Fowler's Enterprise Application Patterns (EAP) - these include more of architecture-level patterns. Examples are: 
Transaction Script, Value Object, Repository.
- Java EE Patterns - a set of design patterns Sun Microsystems came up with when they owned Java. Include terms 
like DAO, Value Object, DTO.
- Domain Driven Design (DDD) Patterns - came from the book "Domain Driven Design" written by Eric Evans. These include
Service, Repository, etc.
- Enterprise Integration Patterns (EIP) - those include techniques of building software that communicates with other
applications via MOMs (Messaging Oriented Middleware that leverages async communication). These are not relevant to
the current course as we use synchronous RESTful API.
- ...

Note, that some of the names (Value Object, Repository) are repeated in different classifications. Authors made their
best to come up with the terms that make sense. Unfortunately for different people terms still meant different
things. So while the names are the same they most often mean different things. The DAO class that we just implemented
can also bear names like DAL (Data Access Layer) and DDD's Repository.

*Tip*: Note how we changed our implementation step-by-step - first creating an alternative implementation and then
migrating the code to that implementation. This is how proper refactoring works. During refactoring there should be no
point in time when application doesn't work. E.g. instead of _moving_ the collection from the Controller to DAO we first
copied it and then slowly migrated to DAO. This kind of proper refactoring is described in Fowler's 
[Refactoring](https://martinfowler.com/books/refactoring.html) book. These are complicated techniques as they often
require extra small steps which programmers don't like to do. But this is how professionals work - it's very (!)
important to ensure that the app always works and that you can interrupt your refactoring in favour of high priority
tasks without breaking anything.

*Tip*: Note how tests ensure that we refactor without breaking anything. Having a solid set of tests gives us courage
to do refactoring - without them it's very dangerous. This is partially how Test Driven Development (TDD) works - 
you implement a quick dirty code which is well tested, and then you improve it. Tests make sure you don't break it
while improving. To dig deeper read [Test Driven Development](https://www.amazon.com/Test-Driven-Development-Kent-Beck/dp/0321146530)
by Kent Beck.


# Step 2

- Add H2 JDBC Driver as a compile-time dependency - for the sake of this course we'll be using an in-memory DB
- Create a `JdbcDogDao` - this is going to store DB-related logic for your Dogs.
- Create an instance of `DataSource` in the constructor, run DDL statements to create `DOG` table with the columns to
fit all your Dog-related data.
- Implement all the CRUD methods that insert, update, select and delete your dogs
- Migrate `DataSource` creation to Spring's XML context and just inject it into your `JdbcDogDao` constructor
- Make it possible in your Controller to switch between InMemory and JDBC DAOs (think about interfaces). 
- Try switching to `JdbcDogDao` and run all the tests to ensure that the app still works.

*Think & research*: Constructor or Setter injection? Spring IoC allows you to inject dependencies both in constructors 
and in `setXxx(xxx)` methods. So why did we chose to inject DataSource into DAO constructor?