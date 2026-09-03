---
Date created: 03-09-26 • 13:32
tags:
  - Web
Related PDF/DOC:
Related Pages:
---
# REST
## Teoria
> REpresentational State Transfer is an architectural style for designing distributed network applications  


REST services are one of the most often encountered ways to implement communication between: 
- client-server, 
- two backend components (*avoid monolitic apps*) 
- two web apps  


REST architectural constraints: 
- **Client-Server**: Concerns should be separated between clients and servers. 
- **Stateless**: The communication between client and server should be stateless.
- **Layered System**: Multiple hierarchical layers such as firewalls, and proxies can exist between client and server. Layers can be added, modified, reordered, or removed transparently to improve scalability  
- **Cache**: Responses from the server must be declared as cacheable or noncacheable. 
- **Uniform Interface**: the interactions between client, server, and intermediary components are based on the uniformity of their interfaces. 
- **Code on demand**: Clients can extend their functionality by downloading and executing code on demand (e.g., JavaScript scripts). This is  optional  


A REST application that fully adheres to all the REST architectural constraints is called a RESTful application  

A REST-like application follows REST conventions but does not strictly satisfy all REST constraints  

A REST service is a backend module that exposes endpoints for specific resources and follows some REST conventions. It is not a web app.

A REST endpoint refers to a specific URL exposed by a REST service. 
It is the entry point for client interactions and Each endpoint corresponds to a unique operation and it is associated with a HTTP method  

## Spring
In Spring, an application can have both traditional controllers and REST controllers. Traditional controllers have endpoints that return Views (HTML pages), while REST controllers have endpoints that return data   


![[EMBED/07-REST_1 1.png]]

[[07-REST_1.pdf#page=6&rect=126,586,473,682|07-REST_1, p.6]]

When implementing REST endpoints, the Spring flow changes: 
- A View Resolver is not needed anymore (no page will be built) 
-  Controller directly returns data 
-  The Dispatcher returns the HTTP response without rendering a view 
-  The front-end takes data and render it to the user  

The `@ResponseBody` annotation informs the Dispatcher Servlet that this method doesn’t return a view name but the data sent directly in the HTTP  

![[EMBED/07-REST_1 2.png]]

[[07-REST_1.pdf#page=7&rect=139,524,346,628|07-REST_1, p.7]]

or we can use `@RestController` instead of `@controller` to avoid to repeat `@RequestBody` for each method

When we use an object to model the data transferred, we name this object a Data Transfer Object (*DTO*)  

We cannot send a raw Java object over the network. We need to convert it in something more appropriate: JSON (XML) (JavaScript Object Notation)

JSON is a lightweight data-interchange format for storing and transport data and a text format  

Jackson is a Java JSON library converting Java objects into JSON and vice versa  
JSON is built on two structures: 
- A collection of name-value pairs (*object*) 
- An ordered list of values (*array*)  

![[EMBED/07-REST_1 3.png]]

[[07-REST_1.pdf#page=11&rect=127,506,466,687|07-REST_1, p.11]]

Both JSON and XML: 
- are human readable
- are hierarchical (*values within values*) 
- can be parsed and used by lots of programming languages 
- can be fetched with an XMLHttpRequest

JSON can be parsed by a standard JavaScript function  

Spring has the interface `HttpMessageConverter` that maintains a list of converters that know how to transform Java objects into formats like JSON, XML.

Spring selects the converter looking at the `Accept` header in the request. If `Accept` is missing, Spring tries to determine the format using the *Content Negotiation* Strategy.

The converter handling JSON, the `MappingJackson2HttpMessageConverter`, uses the Jackson `ObjectMapper` : 
- Inspects the object
-  Reads all its fields and annotations 
-  Builds a JSON string from it  

to use XML
Add the required dependencies in the pom.xml  
Annotate the DTO with: 
- `@JacksonXMLRootElement` : root XML tag
- `@JacksonXMLProperty` : It defines the XML fields names  

### Backend communication
we need to know how to call a REST endpoint exposed by another service
Use OpenFeign, a tool offered by Spring Cloud  

OpenFeign works on the server playing the role of a client:
- prepares the request to be sent to the server: If it needs to send data, it converts Java obects in JSON or XML
- It receives data (e.g., JSON or XML) from the responses sent by the server and builds a Java obejct for such data  

How does it do it :
1. For each server the backend component wants to connect to, an interface is defined that contains methods, each corresponding to a possible request to the server (one action for each server’s endpoint). The method’s parameters specify what is sent in the request, the return value what came back with the response 
2. OpenFeign automatically write the class implementing the interface 
3. In the interface annotations are used to specify the server’s URL and the endpoint (the action)  

A Configuration class is needed to enable OpenFeign functionality and the search for interfaces. The annotation `@EnableFeignClient` is used.  


