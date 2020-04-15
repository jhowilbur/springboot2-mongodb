# workshop-springboot2-mongodb
> Welcome to this brief tutorial, future questions please call me by git.

## Main goal:
1. Understand the main differences between document-oriented and relational paradigm
1. Implement CRUD operations
1. Reflect on design decisions for a document-oriented database
1. Implement associations between objects
   * Nested objects
   * References
1. Perform queries with Spring Data and MongoRepository
##
## how the system would work if it were in the relational database model
####
![1](https://user-images.githubusercontent.com/59379254/79361956-5a5bd100-7f1c-11ea-9888-15113301e38c.png)
####
![2](https://user-images.githubusercontent.com/59379254/79361963-5c259480-7f1c-11ea-8749-3ef32715096c.png)
####
![3](https://user-images.githubusercontent.com/59379254/79361973-5def5800-7f1c-11ea-96fa-59493364f6b5.png)
####
##
## how the system works based on a non-relational database
####
![1 1](https://user-images.githubusercontent.com/59379254/79362348-dc4bfa00-7f1c-11ea-8505-315d60bf3a63.png)
####
![2 1](https://user-images.githubusercontent.com/59379254/79363467-4fa23b80-7f1e-11ea-989c-986587ca6e24.png)
####
![3 1](https://user-images.githubusercontent.com/59379254/79363470-503ad200-7f1e-11ea-8bc2-d5e9dfcd7007.png)
####

# Hands-on
- Install MongoDB - [MongoDB](https://www.mongodb.com/products/compass)
- Installing Mongo Compass - [Mongo Compass](https://www.mongodb.com/products/compass)

## Firts commit - project created - [in commits]
Check list:
- [ ] File -> New -> Spring Starter Project
- Choose only the Web package for now
- [ ] Run the project and test: http: // localhost: 8080
- [ ] If you want to change the default port of the project, include in application.properties: server.port = $ {port: 8081}

## Entity User and REST working - [in commits]
#### Checklist for creating entities:
- [ ] Basic attributes
- [ ] Associations (start collections)
- [ ] Constructors (do not include collections in the constructor with parameters)
- [ ] Getters and setters
- [ ] hashCode and equals (default implementation: id only)
- [ ] Serializable (default: 1L)
#### Check list:
- [ ] In the domain subpackage, create the User class
- [ ] In the resources subpackage, create a UserResource class and implement the standard GET endpoint:

```java
  @RestController
  @RequestMapping(value="/users")
  public class UserResource {
  @RequestMapping(method=RequestMethod.GET)
    public ResponseEntity<List<User>> findAll() {
        List<User> list = new ArrayList<>();
        User maria = new User("1001", "Maria Brown", "maria@gmail.com");
        User alex = new User("1002", "Alex Green", "alex@gmail.com");
        list.addAll(Arrays.asList(maria, alex));
        return ResponseEntity.ok().body(list);
      }
  }
```

## Connecting to MongoDB with repository and service - [in commits]
####
![Screenshot_2](https://user-images.githubusercontent.com/59379254/79355665-56c44c00-7f14-11ea-96be-a1682c5a87ce.png)
####
#### References:
https://docs.spring.io/spring-boot/docs/current/reference/html/common-application-properties.html
https://docs.spring.io/spring-boot/docs/current/reference/html/boot-features-nosql.html
https://stackoverflow.com/questions/38921414/mongodb-what-are-the-default-user-and-password
#### Check list:
- [ ] In pom.xml, include the MongoDB dependency:
~~~xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-mongodb</artifactId>
</dependency>
~~~
- [ ] In the repository package, create the UserRepository interface
- [ ] In the services package, create the UserService class with a findAll method
- [ ] In User, include the annotation @Document and @Id to indicate that it is a MongoDB collection
- [ ] In UserResource, refactor the code, using the UserService to search for users
- [ ] In application.properties, include the connection data with the database:
- spring.data.mongodb.uri = mongodb: // localhost: 27017 / workshop_mongo
- [ ] Up MongoDB (mongod command)
- [ ] Using MongoDB Compass:
- Create database: workshop_mongo
- Create collection: user
- Create some user documents manually
- [ ] Test the endpoint / users


## Database instantiation operation - [in commits]
#### Checklist:
- [ ] In the config subpackage, create an Instantiation configuration class that implements CommandlLineRunner
- [ ] Data to copy:
~~~java
User maria = new User(null, "Maria Brown", "maria@gmail.com");
User alex = new User(null, "Alex Green", "alex@gmail.com");
User bob = new User(null, "Bob Grey", "bob@gmail.com");
~~~

## Using DTO standard to return users - [in commits]
#### References:
https://en.stackoverflow.com/questions/31362/o-que-é-um-dto
- DTO (Data Transfer Object): it is an object that has the role of loading data from entities in a simple way,
it may even "project" only some data from the original entity. Benefits:
- Optimize traffic (traveling less data)
- Prevent data that is of exclusive interest to the system from being exposed (for example: passwords,
audit as creation date and object update date, etc.)
- Customize the objects trafficked according to the needs of each request (for example: to change
a product, you need data A, B and C; to list the products, I need data A, B and a
category of each product, etc.).
#### Check list:
- [ ] In the dto subpackage, create UserDTO
- [ ] In UserResource, refactor the findAll method

## Getting a user by id - [in commits]
#### Check list:
- [ ] In the service.exception subpackage, create ObjectNotFoundException
- [ ] In UserService, implement the findById method
- [ ] In UserResource, implement the findById method (return DTO)
- [ ] In the resources.exception subpackage, create the classes:
- the StandardError
- the ResourceExceptionHandler

## User insertion with POST - [in commits]
#### Check list:
- [ ] In UserService, implement the insert and fromDTO methods
- [ ] In UserResource, implement the insert method

## User selection with DELETE - [in commits]
#### Check list:
- [ ] In UserService, implement the delete method
- [ ] In UserResource, implement the delete method

## User update with PUT - [in commits]
#### Check list:
- [ ] In UserService, implement the update and updateData methods
- [ ] In UserResource, implement the update method

## Creating entity Post with nested User - [in commits]
#### Note:
- nested objects vs. references
#### Check list:
- [ ] Create Post class
- [ ] Create PostRepository

## Projection of author data with DTO - [in commits]
#### Check list:
- [ ] Create AuthorDTO
- [ ] Refactor Post
- [ ] Refactor the initial database load
- IMPORTANT: now it is necessary to persist User objects before relating

## Referencing user posts - [in commits]
#### Check list:
- [ ] In User, create the "posts" attribute, using @DBRef
- Tip: include the parameter (lazy = true)
- [ ] Refactor the bank's initial load, including post associations

## Endpoint to return a user's posts - [in commits]
#### Check list:
- [ ] In UserResource, create the method to return a user's posts

## Getting a post by id - [in commits]
#### Check list:
- [ ] Create PostService with the findById method
- [ ] Create PostResource with findById method

## Adding comments to posts - [in commits]
#### Check list:
- [ ] Create CommentDTO
- [ ] In Post, include a list of CommentDTO
- [ ] Refactor the initial database load, including some comments on posts

## Simple query with query methods - [in commits]
#### References:
https://docs.spring.io/spring-data/mongodb/docs/current/reference/html/
https://docs.spring.io/spring-data/data-document/docs/current/reference/html/
#### Query:
- "Search for posts containing a given string in the title"
#### Check list:
- [ ] In PostRepository, create the search method
- [ ] In PostService, create the search method
- [ ] In the resources.util subpackage, create URL utility class with a method to decode URL parameter
- [ ] In PostResource, implement the endpoint

## Simple query with @Query - [in commits]
#### References:
https://docs.spring.io/spring-data/mongodb/docs/current/reference/html/
https://docs.spring.io/spring-data/data-document/docs/current/reference/html/
https://docs.mongodb.com/manual/reference/operator/query/regex/
#### Query:
"Search for posts containing a given string in the title"
#### Check list:
- [ ] In PostRepository, make the alternative implementation of the query
- [ ] In PostService, update the query call

## Query with multiple criteria - [in commits]
#### Query:
"Search for posts containing a given string anywhere (in the title, body or comments) and in a given
date range "
#### Check list:
- [ ] In PostRepository, create the query method
- [ ] In PostService, create the query method
- [ ] In the URL utility class, create methods for handling received dates
- [ ] In PostResource, implement the endpoint

