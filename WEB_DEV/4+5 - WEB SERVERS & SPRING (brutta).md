---
Date created: 02-09-26 • 16:06
tags:
  - Web
Related PDF/DOC:
  - "[[04-WebServers.pdf]]"
Related Pages:
---
# Dynamic pages
We want to obtain NON STATIC information from the server, so we build *dynamic pages*

First approach - Code build HTML :
The web server run a process that compute what you need and generate (*writes*) a HTML page. The page is inserted in the response body and sent to the client.
This is called Common Gateway Interface (CGI)  


## Servlets
Best approach - Embed code in HTML :
Augment the web server with an engine capable of parsing a web page, and executing code in it.


A Container is used to understand HTTP and translate the request / response HTTP to a Java object, thisis called a Servlet Container.
Each servlet is associated with a path. When a client sends a requests, the Container calls the servlet registered at that specific path.  
The servlet gets the values on the request and builds the response that the Container sends back to the client.

 Apache Tomcat is a Java servlet container, and is run on a Java Virtual Machine   

when the Container receives requests from a client, it creates two Java objects: 
- HTTPServletRequest 
- HTTPServletResponse 

and passes them to the invoked servlet  

---

## Frameworks
> a set of tools and functionalities on top of which you can build applications.   

you can choose what pieces of software you need according the application specifications: you do not need to use all the features the frameworks offers.


### Spring
>Spring it is the most used Java framework today

Spring is modular, you do not need to add the whole Spring to your web app  
![[EMBED/05-Spring_1.png]]

[[05-Spring_1.pdf#page=6&rect=146,465,469,691|05-Spring_1, p.6]]

#### IOC - Inversion Of Control
Wihtout IOC The application executes and controls the dependencies   

![[EMBED/05-Spring_1 2.png]]

[[05-Spring_1.pdf#page=8&rect=127,154,437,345|05-Spring_1, p.8]]

IoC describes a design in which custom-written portions of a computer program receive the flow of control from a generic, reusable external framework  

**Il framework controlla l'app**

![[EMBED/05-Spring_1 3.png]]

[[05-Spring_1.pdf#page=9&rect=131,503,467,666|05-Spring_1, p.9]]

Objects do not control their own lifecycle and dependencies, a framework do it.

Advantages:
- **Loose Coupling**: Components are less dependent on each other, more modular and easier to test.
- **Improved Maintainability**: Changes to one part of the application can be done without affecting other parts of the system.
- **Flexibility and Scalability**: New components can be added or existing ones replaced without changing the structure of the application.
- **Centralized Configuration**: Configuration of components is handled externally, making it easier to change and manage dependencies.  



> [!warning] DI - Dependency Injection
> concrete IoC  example.
> Spring injects dependencies at runtime
> 
> - **Constructor Injection**: Dependencies are provided to the class via the *constructor* (*preferred method*)
> - **Setter Injection**: Dependencies are set via *setter methods* after the object is constructed
> - **Field Injection**: Dependencies are injected directly into *fields using annotations*  

#### AOP
Object Oriented Programming (OOP) makes it hard to implement some horizontal activities called <mark class="hltr-orange">crosscutting concerns</mark> (*e.g. authentication, logging, security checks*), that have to be **isolated**.

The management of such activities <mark class="hltr-red">should not be delegated and repeated several times</mark> by the business logic.

Aspect Oriented Programming (AOP) is a complementary paradigm to OOP able to describe and manage such aspects without mixing them with the business logic  

> [!QUOTE] Aspect
> > A concern to be modularized and applied to various parts of a program. It defines where and when certain behaviors should happen, but it doesn't specify how they should happen.
>
> PDF : [[05-Spring_1.pdf#page=12&selection=8,8,14,23|05-Spring_1, p.12]]
> 

An aspect is implemented in a **join point** (*point in the execution of a program where an aspect can be applied*) trough an **advice** (*code that is executed when a certain join point is reached.*).

Advantages of AOP :
- **Separation of Concerns**: Cross-cutting concerns are isolated in separate aspects rather than being scattered
- **Reusability**: Since aspects are modular, they can be reused across multiple parts of the application.
- **Cleaner Code**: By moving cross-cutting concerns out of the core business logic, the code becomes cleaner 
- **Improved Maintainability**: Changes to cross-cutting concerns can be made just in one place  

#### Container and beans
The Spring Container... 
- is responsible for managing the objects in an application
- handles their lifecycle
- is built around the principle IoC, meaning that it controls the flow of the application, not the developer  

A type of Sping Container is the interface `ApplicationContext`.  

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

A Spring **Bean** is a Java object (*POJO*) whose lifecycle is managed by the `ApplicationContext`.

Beans can have different roles and scopes:
- Controller
- Service 
- Repository 

 A Spring Bean cannot survive outside the Spring framework  


> [!info] Annotations
> Annotations are a powerful mechanism introduced by Java 5 to add metadata to the code.
> 
> They provide metadata on a specific code element without changing its behavior. Their main purpose is to allow other tools, such as compilers or frameworks, to treat the code in a special way, without the programmer having to write explicit code to achieve that behavior.
> 
> They are declare using the symbol `@` followed by the annotation name and possible parameters  

A bean lifecycle is separated in :
- Creation (Instantiation and DI)  
- Post-initialization (optional)  
- Usage  
- Destruction  


##### Step 1: Creation (Instantiation)
Adding the Bean to the ApplicationContext so that it can *see* it.

We can do this in 2 ways ...
1. Using the `@Bean` annotation and a Java Configuration class  

Used When the class whose object we want to create is not defined in the app or  when we want to have a fine-grained control.

2. Using stereotype annotations  

The Bean is created as an instance of a class annotated using `@Component` (or its derivatves as `@Controller`, `@Repository`…)  

Used When you do not need to have a fine-grained control  

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

Let suppose now to have two Beans  We want to establish a relationship between them  (*delegate some functions*). We use the depencency injection.

1. With `@Bean` : in the Configuration class the `A` method takes a `B` as a parameter that then it is passed to the `A`’s Constructor.
2. With stereotype : `A`’s Constructor is called directly by String that passes also a `B` as a parameter. The `@Autowired` annotation is used.  



##### Step 2: Post initialization  
This is an optional step to complete the initialization of beans (*setting property of the bean*) <mark class="hltr-orange">only when stereotype annotations are used</mark>. 

Writing an init method in your bean class, and annotating it using `@PostConstruct`, and afterAdd in the pom.xml the dependency.

##### Step 3 and 4
Step 3: 
After creation, the bean is part of the Spring context and ready to be used.

Step 4:
When the application context is closed (`.close()`), the bean will go through the destruction phase. This phase is handled by the Spring container.



#### Architecture
A servlet Container take care of what is needed to communicate with the client using HTTP.

You do not need to write servlets anymore, Spring uses just an already-written servlet as entry point of the app ‘s logic.

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

Design patterns are are reusable solutions to common problems in software design  

The Model-View-Controller (MVC) is an architectural design pattern separating the code into three components...

- **Model** : Holds data and knows the rules for getting and updating state. It’s the only part of the application that talks to the database.  
- **Controller** : The entry point of the requests: it receives requests, it invokes the Model to obtain and modify data, it chooses the View  
- **View** : Responsible for preparing the response. It gets the state of the Model from the Controller  

every request first passes through a single central entry point: the<mark class="hltr-orange"> Front Controller  </mark>, it receives requests, apply common operations, send the request to the right Controller.

We can use more than a Controller in our web app, for ...
- Separation of functionalities 
- Different HTTP methods 
- User roles and permissions (admin vs. user; public vs. private)  

Sometimes, a HTTP response is prepared by more methods of the same Controller or more methods of different controllers. to do this we do `forwarding`.

Forward refers to the process of forwarding a request from one method (controller) to another one within the same web app.internal server-side routing mechanism performed by the Dispatcher Servlet.  The URL in the browser does not change.

Spring, under the hood, provides an infrastructure based on the Front Controller pattern.

Sometimes a Controller decides that it does not want to manage a request at all, so it forces the client to send a new request to somebody else  
Through `redirect` the server instructs the client's browser to make a new HTTP request to a different URL to the same/another web app in the same /another web server  
Done trough response to the request with a status code 301 and a “location” header with a URL as the value  


1. The client makes an HTTP request 
2. Tomcat accepts the request, translates and delivers it to the Spring app. The  servlet takes the HTTP request and manages the flow
3. The servlet first needs to find out what method of the Controller to call depending on the path and the HTTP method of the request. To find the method it delegates the Handler mapping
4. Once it knows what method of Controller to call, the dispatcher servlet calls it. After execution, the Controller method provides the view name and some data to the view  
5. The dispatcher servlet finds the view. To find the view to render, the dispatcher servlet delegates a component called View resolver (*incorporates data into the view, eg thymeleaf*)
6. Through Tomcat, the HTTP response is returned to the client 
7. The browser interprets the HTTP response and displays the data  


> [!info] Spring boot
> Spring Boot is a project of the Spring ecosystem
> Main features: 
> - Simplified project creation through the Spring Inizializr
> - Dependency starters 
> - Autoconfiguration based on dependencies  
>   
> A dependency starter is a group of dependencies added in the pom.xml file to configure a web app for a specific purpose (*group of dependencies*) 
> 
> Spring Boot adopts convention-over-configuration : The convention represents the most-used way to configure the app for a specific purpose. Spring Boot configures the app by convention.  
> 
> 
> SpringBoot at start performs: 
> - Creation of an instance of the ApplicationContext 
> - Component Scanning of beans in the package and the subpackages of the main application 
> - Auto-configuration of beans 
> - Start of the application (a embedded Tomcat is created and run in the JVM)
> - Creation of the DispatcherServlet and its registration in Tomcat  


 

> [!info] Thymeleaf
> Thymeleaf is a server-side Java template engine (*html templates and components*): 
> - Natural templates 
> - Template look and work like HTML 



#### Deploy
During development, local deployments are made on the Spring Boot’s Tomcat  

At the end of development, the final version of the web app is moved to the externally accessible web server   

Two ways: 
- Traditional deployment (*.war*): 
  the web app is copied to a Java application server (Tomcat, Jetty, WildFly). The server runs many apps and provides the web app runtime, lifecycle, and management
- Modern deployment: (*.jar*) : 
  the web app runs as a standalone process or container, and orchestrate with cloud PaaS (Platform-as-a-Service) providers. The web server is embedded in the .jar archive  

#### Sessions
>Sequence of http requests and responses logically correlated   

Interactions with a web app can be thought as a conversation. Such a conversation is called: **navigation session**  

<mark class="hltr-red">HTTP is stateless  </mark>, Three solutions :
- HTML hidden field
- Cookies 
- Session  

Sessions are a server-side solution to store information, whereas cookies are a client-side solution that stores information.

Sessions can be cookies dependent, whereas cookies are not dependent on sessions 

A session ends when the user closes the browser or logout, whereas cookies expire at a certain prefixed time

A session can store as much data you want, whereas Cookies have a limited size of 4KB  

##### HTML hidden field 
Information related to the state is passed as parameter though the Controller to give the illusion of persistence.

The server responds with some html with hidden fields with previously entered data.

##### Cookies
> A Cookie is a small amount of information sent by a server to a Web browser, saved by the browser, and later sent back to the server  

A cookie's value uniquely identifies a client and has proprieties :
- name
- a single value
- *optional* : 
	- comment
	- path 
	- domain qualifiers
	- maximum age
	- version number  

![[EMBED/05-Spring-5.png]]

[[05-Spring-5.pdf#page=6&rect=135,538,470,688|05-Spring-5, p.6]]
![[EMBED/05-Spring-5 1.png]]

[[05-Spring-5.pdf#page=6&rect=129,190,460,338|05-Spring-5, p.6]]

The first time a client and a web server meet The server prepares a new cookie and sends it to the browser by adding them to the HTTP response headers.

In spring this is done through a `ResponseCookie` object that builds the cookie and a `ResponseEntity` object that allows you to specify the response body, status and header on a HTTP response .

Next times the client meets the same web server The browser returns cookies to the server by adding fields to HTTP request headers. The server reads the cookies that come back using a parameter of the invoked method annotated with `@CookieValue`  

`@CookieValue` only works in Controller’s methods. 
It reads a specific cookie’s value in a HTTP request. Using `required = false`: if the specific cookies is not there, the variable for the cookie value is set to null.  

###### Regole italiane
**Cookie tecnici**: quelli utilizzati al solo fine di "effettuare la trasmissione di una comunicazione su una rete di comunicazione elettronica, o nella misura strettamente necessaria al fornitore di un servizio  

Possono essere suddivisi in:
- cookie di *navigazione o di sessione*, che garantiscono la normale navigazione e fruizione del sito
- cookie *analytics*, assimilati ai cookie tecnici laddove utilizzati direttamente dal gestore del sito per raccogliere informazioni  
- cookie di *funzionalità*, che permettono all'utente la navigazione in funzione di una serie di criteri selezionati (ad esempio, la lingua)

**Cookie di profilazione**: 
sono quelli volti a creare profili relativi all'utente e vengono utilizzati al fine di inviare messaggi pubblicitari

Un ulteriore elemento da considerare è tenere conto  che si tratti dello stesso gestore del sito che l'utente sta visitando o di un sito diverso che installa cookie per il tramite del primo.

##### Sessions
Sessions are higher level mechanisms than cookies.
For each conversation, the web server generates a unique *ID* and a *data structure in memory* which are associated both with that conversation.

The unique ID is exchanged with the client during its conversation with the web server and The data structure is used to keep track of the state 

![[EMBED/05-Spring-5 2.png]]

[[05-Spring-5.pdf#page=13&rect=149,115,462,344|05-Spring-5, p.13]]

In all the responses to the client, the web server automatically and transparently adds a cookie with the session ID, a **session cookie** wich ...
- has no expiration time 
- dies at the end of the session  

In spring Sessions are represented by an instance of a class that implements the `HttpSession` interface  

When a `@SessionScope` bean is used by the web app, Tomcat retrieves or creates the HTTP session: 
- If no session still exists: 
	1. A HttpSession object is created
	2. A JSESSIONID cookie is generated
	3. A timeout is configured
- If the session exists
	- The session is retrieved
- Tomcat returns the `HttpSession` object to Spring
- Spring creates a `@SessionScope` bean, binds it to the session, and injects it where needed 
- When the session ends (timeout or logout), the bean is destroyed  

Timeout is the maximum amount of time a HTTP session waits for an event or activity before it is automatically terminated from the server  and is reset on every user request.

When a session is inactive for the timeout period, Spring is notified about that, and it will trigger the destruction of the corresponding @Session-scoped bean:  
- If the bean has a `@PreDestroy` annotated method, such a method is invoked;  
- Reference to the bean is released.  


At logout : 
1. Clear the state in your Session-Scoped Bean: 
  Set the state of the bean to null, this removes the user's information from the session-scoped bean; 
2. Invalidate the Entire Session: 
  Inject the `HttpSession` object into the Controller managing the logout, this assures to also **invalidate the entire HTTP session upon logout**. Any other session-related data is also cleared;
3. Then `@predestroy` and release reference



---
## Codice spring
When a relative URL starts with a “/”, the browser interprets it as being relative to the root of the domain (the part before the first / after the protocol and domain name)  

```html
http://localhost:8080/compute  
```

Thymeleaf has attributes designed to handle URL generation while being aware of the web application's context path: th:action, th:ref, th:src,and so on  

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

Try to separate the business logic (computation) from the code that manages HTTP requests and responses. Use the @Autowired annotation.  

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

We know the @Controller annotation, a specialization of @Component  
introduce @Service, another annotation that specializes @Component  


![[EMBED/05-Spring-4.png]]

[[05-Spring-4.pdf#page=4&rect=143,126,459,276|05-Spring-4, p.4]]

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

Include is a mechanism to embed the content of another resource (e.g. a HTML fragment) inside the current res  
The included content is processed as part of the current response, but the original request isn't interrupted.  

Thymeleaf takes care to include content from another template or fragment using:  
- `th:include`
  inserts the fragment inside the tag where it’s applied: the original element stays in the final output and contains the fragment content  
- `th:replace`
  replaces the entire tag where it’s applied with the fragment content, removing the tag and inserting only the fragment.  

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

We came across various beans serving different roles • But, what about their scope and life time?  

 Spring we have (at least) the following scopes:
 - Singleton scope 
 - Session scope 
 - Request scope

Singleton scope:
It is the default scope of Spring beans ...
- Spring creates it during the startup of the application, and it stays alive until Spring is shut down
- Just one single instance of the bean, or multiple instances named in different ways are created
- The same instance is given or injected into all components that require it
- Since there is only one instance of a singleton bean, if the bean’s attributes are mutable and accessed concurrently by multiple threads, they must be thread-safe. Thus, making the bean’s attributes immutable (using final) or another synchronization mechanism is needed.  

we have just one controller shared in the web app receiving all the requests  
its methods are threaded for each request.   

Controller’s attributes: 
- Unmutable attribute + final: THREAD SAFE! 
- Mutable attribute: NOT THREAD SAFE! 

Solutions: 
- Local variables / parameters 
- Unmutable attribute + final  

Services injected in a Controller: 
- Stateless service: THREAD SAFE! 
- Shared state (mutable) : NOT THREAD SAFE!

Solutions: • synchronization  and syncronized methods in java


Session scope...
- It is created when a HTTP session is created. It is destroyed when the session expires or it is invalidated; 
- A new instance of the bean is created for each HTTP session (basically for each user), and this instance is shared across multiple requests within the same session;
- If multiple users are accessing the web app, each of them will get their own instance of the bean; 
- It is typically used for stateful beans that need to maintain state for the duration of a user's session (e.g., shopping carts).  

At the web app startup, Spring creates the singleton Controller • A session-scoped bean requires an active HTTP session …. the real bean cannot be injected • Spring injects a proxy instead of the real session bean:  

The controller calls a method on the bean
- The proxy intercepts the call
-  It retrieves the correct session bean instance 
-  Delegates the method call to that instance  


Request Scope...
- It is created at the beginning of each HTTP request and destroyed at the end of it, which means it’s short-lived and only available during the request lifecycle; 
- A new instance of it is created for each HTTP request, and this instance is shared across all components that need it within that same request; 
- It is typically used for stateless beans that don’t need to retain any state between requests. This scope is often used for request-specific data that doesn’t need to persist between multiple requests; 
- It is not proned to multithread related issues as only a thread (the one of the request) can access them  

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

Spring is configured trough a application proprieties file (*every bean config also here*)

<hr style="width: 70%; margin-left: auto;margin-right: auto;">
I
n src/main/resources: • Add a schema.sql file in (schema defines tables) • If you want having initial data, add a file data.sql with it  

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

In Thymeleaf, we can implement iteration using the th:each attribute  

```html
<tr th:each="user: ${users}"> <td th:text="${user.id}" /> <td th:text="${user.name}" /> </tr>  
```

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

Using POST on a form data will be put in the body of the request: 
By default, a browser automaticallty send the form data with the content type: `application/x-www-form-urlencoded` 

The controller reads each field of the form using `@RequestParam`  

In case the client wants to send a file  
The content file to be specified into the form is: `multipart/form-data`  

What about sending a JSON?  
The Controller will use `@RequestBody` to take data  

