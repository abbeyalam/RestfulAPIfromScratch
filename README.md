This is a mock RESTFUL API I built from scratch using Spring Boot, which is framework for building Java Applications.

![image](https://user-images.githubusercontent.com/13660252/162628639-9c7da55e-ac89-440d-9362-11dc990c502d.png)

The Client in this program will be simulated by Postman, however in most cases will be a Front-end language/framework like React.js, Vue.js, etc. 

An 'API' stands for Application Programming Interface. There are different types of API's used today such as SOAP, RPC, Websocket, and REST API's. In this case, we will be using REST API's as they are the most popular and flexible API's found on the web today. 

How does an API work? The client sends a request to the server as data. This request looks like GET, POST, DELETE, and PUT.  The server then uses this client request to carry out internal functions and returns an output back to the client. 

Another way of looking at API's is when you are at a restaurant and decide to order a meal. A waiter takes your order back to the kitchen and eventually returns the meal you requested. You don't know how the food was prepared and you don't need to, but you are returned with an output. 

An API has a few different layers that will translate to the packages made in this program. 

Initially the client, in this case Postman will send a request (GET, POST, PUT, DELETE) to the API/Controller. This request gets sent to the Service layer, which handles all business logic in the application. After performing business logic, the Data Access Layer will be responsible for connecting to a database (PostgreSQL, MongoDB). In the Data Access Layer/DB we are able to perform our CRUD operations. 

What does a response look like? A response can look like a JSON file, an image, a status code, etc. 

To start: Generate a Maven Project from https://start.spring.io/

Once you have the project opened, we can start building the API. 

I started by creating four packages (API, DAO, Model, Service) to mirror the API architecture I mentioned earlier. 

![image](https://user-images.githubusercontent.com/13660252/162629798-1d085370-8979-4d15-b225-3da9cd6aae64.png)

Once completed, I decided to go ahead and define our model. In this case my model will be a Person with attributes like ID and Name. In my model package, I created a class called 'Person' in which I created private variables for id and name, along with a constructor and a getter. 

![image](https://user-images.githubusercontent.com/13660252/162630739-b98a9ded-c080-4942-b456-e6cd506162bd.png)

At this point we have a model we can work with and can define the database layer. Inside of the Dao package, I created an interface called PersonDao and a class called FakePersonDataAccessService that implements PersonDao. PersonDao will contain the contract for anyone that wants to implement the interface. A few methods that I have created in this interface are: insertPerson, selectAllPeople, selectPersonById, deletePersonById, and updatePersonById. 

![image](https://user-images.githubusercontent.com/13660252/162630989-59d77b25-57e3-422b-ac36-8481ef58ee8d.png)

In FakePersonDataAccessService, we will define methods to access our data (CRUD operations)
Here we will implement and modify the methods created in the PersonDao interface.
We also create an ArrayList to hold 'Person' objects.

In the service layer (business logic), I created a class called PersonService, which will hold methods to add Person, get a list of people, get a person by Id, etc. 

Using dependency injection, I am able to access the methods in the PersonDao interface. 

<insert image of service layer>

In the API package, I created a PersonController which will reference the PersonService that we just created. Using the PersonService, I am able to create an addPerson method. 
  
<insert image of controller>
  
At this point all of our classes are just Java classes and do not implement Spring. To implement Spring, we add @Component to the FakePersonDataAccessService, PersonService. To identify that we are utilising dependency injection, simply add @Autowired above the constructor, for example above the PersonService constructor or PersonController constructor, which autowires into the PersonDao interface. 
In addition we need to specify that the PersonController is a REST Controller and we do this by using the @RestController annotation above the class.
  
 
The Rest Controller will expose methods and endpoints that the client can consume.
In this controller, Spring needs to be able to tell what kind of request the method is. For example, the addPerson method is a POST request because it is adding an object to the data. We do this by adding the @PostMapping annotation above the method. 
  
At this point, in our Rest Controller, we have an addPerson method, which takes in a person in it's parameter. However, when creating a request to addPerson from the client to the Server, we need to define the properties inside a model (id, name) so Spring knows that these will be JSON Properties.   
  
![image](https://user-images.githubusercontent.com/13660252/162632518-a783501f-d075-4fb6-bbb6-f41f86dc8ea1.png)
  
We also need to specify that we are recieving a JSON payload (id, name) from the body. We do this by adding the @RequestBody annotation in the method parameter. For example: public void addPerson(@Requestbody Person person)
  
Lastly, we need to specify the mapping of the endpoints. We do this by adding the @RequestMapping annotation along with a path above the class. 
  
![image](https://user-images.githubusercontent.com/13660252/162632683-67306f67-11a6-4b77-856d-a8f7ced71fe4.png)
  
At this point, we have created an endpoint and functionality in our API to POST or addPerson. 
  
Similarly, if we wanted to return a list of people from our DB, our client would submit a 'GET' request. 
In our PersonDao, we would create a method to return a list of people. Our FakePersonDataAccessService class would implement this method. 
    
In our service layer, we will create a method to return a list of people, that would reference the method created in our PersonDao. 
  
Lastly, in our PersonController, we create the endpoint which references the PersonService method and add the @GetMapping annotation above the method, so Spring can recognize that this is a GET request.  
  
  
 
  

