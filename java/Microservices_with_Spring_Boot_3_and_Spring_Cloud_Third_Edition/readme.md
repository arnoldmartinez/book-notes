## Design patterns for microservices

The concept of design patterns is actually quite old; it was
invented Christopher Alexander back in 1977. In essence, 
a design pattern is about describing a reusable solution to a 
problem when given a specific context.

### Service Discovery

##### Problem

How can clients find microservices and their instances?. When 
running in containers. This makes it difficult for a 
client to make a request to a microservice that, for example,
exposes a REST API over HTTP. Consider the following diagram:

![alt text](img/ch1_figure_1.9.png)

##### Solution

Add a new component - a **service discovery** service - to the
system landscape, which keeps track of currently available
microservices and the IP addresses of its instances.

##### Solution requirements

* Automatically register/unregister microservices and their
  instances as they come and go.
* The client must be able to make a request to a logical
  endpoint for the microservice. The request will be routed to
  one of the available microservice instances.
* Requests to a microservice must be load-balanced over
  the available instances.
* We must be able to detect instances that currently are
  unhealthy so that request will not be routed to them.


* **Client-side routing:** The client uses a library that
  communicates with the service discovery service to find
  out the proper instances to send the request to.
* **Server-side routing:** The infrastructure of the service
  discovery service also exposes a reverse proxy that all
  requests are sent to. The reverse proxy forward the 
  requests to a proper microservice instance on behalf of the
  client.

### Edge Server

##### Problem

In a system landscape of microservices, it is in many cases
desirable to expose some of the microservices to the outside of 
the system landscape and hide the remaining microservices from
external access. The exposed microservices must be protected
against requests from malicious clients.

##### Solution

Add a new component, an **edge server**, to the system landscape
that all incoming requests will go through:

![alt text](img/ch1_figure_1.10.png)

**Implementation notes:** An edge server typically behaves like a 
reverse proxy and can be integrated with a discovery service to 
provide dynamic load-balancing capabilities.

##### Solution requirements

* Hide internal services that should not be exposed outside
  their context; that is, only route requests to microservices
  that are configured to allow external requests
* Expose external service and protect them from malicious
  requests; that is, use standard protocols and best practices
  such as OAuth, OIDC, JWT tokens, and API keys to ensure 
  that the clients are trustworthy

### Reactive microservices

##### Problem

As Java developers, we are used to implementing
synchronous communication using blocking I/O, for example, a
RESTful JSON API over HTTP. Using a blocking I/O means that a 
thread is allocated from the operating system for the length of
the request.

If the number of concurrent request goes up, a server might run
out of available threads in the operating system, causing
problems ranging from longer response times to crashing servers.
Using a microservice architecture typically makes this problem
even worse, where typically a chain of cooperating microservices
is used to serve a request. The more microservices involved in
serving a request, the faster the available threads will be drained.

##### Solution

Use non-blocking I/O to ensure that no threads are allocated 
while waiting for processing to occur in another service, that is, a
database or another microservice.

##### Solution requirements

* Whenever feasible, use an asynchronous programming
  model, sending messages without waiting for the receiver
  to process them.
* Use reactive frameworks that can execute synchronous
  requests using non-blocking I/O, without allocating a 
  thread while waiting for a response. This will make the 
  microservices easier to scale in order to handle an
  increased workload.
* Microservices must also be designed to be resilient and
  self-healing. Resilient meaning being capable of producing
  a response even if one of the services it depends on fails;
  self-healing meaning that once the failing service is
  operational again, the microservice must be able to resume
  using it.

##### Note

In 2013, key principles for designing reactive systems were 
established in The Reactive Manifesto https://www.reactivemanifesto.org/

### Central configuration

##### Problem

An application is,traditionally, deployed together with its configuration, for example, a set of
environment variables and/or files containing configuration information.

##### Solution

Add a new component, a **configuration server**, to the system landscape to store the configuration of all
the microservices, as illustrated by the following diagram:

![alt text](img/ch1_figure_1.11.png)

##### Solution requirements

Make it possible to store configuration information for a group of microservices in one place, with
different settings for different environments (for example, **dev**, **test**, **qa** and **pro**).

### Centralized log analysis

##### Problem

Traditionally, an application writes log events to log files that are stored in the local filesystem of the
server that application runs on.

##### Solution

Add a new component that can manage **centralized logging** and is capable of the following:

- Detecting new microservice instances and collecting log events from them
- Interpreting and storing log events in a structured and searchable way in a central database
- Providing APIS and graphical tools for querying and analyzing log events

##### Solution requirements

Some solution requirements are as follows:

- Microservices stream log events to standard system output, **stdout**. This makes it easier for a log
  collector to find the log events compared to when log events are written to microservice-specific log files.
- Microservices tag the log events with the correlation ID described in the next section regarding the
  *Distributed tracing design pattern*
- A canonical log format is defined, so that log collectors can transform log events collected from
  the microservices to a canonical log format before log events are stored in the central database.
  Storing log events in a canonical log format is required to be able to query and analyze the
  collected log events.

### Distributed tracing

##### Problem
It must be possible to track requests and messages that flow between microservices while processing and external
request to the system landscape.

![alt text](img/ch1_figure_1.13.png)

##### Solution

To track the processing between cooperating microservices, we need to ensure that all related requests and
messages are marked with a common **correlation ID** and that the correlation ID is part of all log events. Based on a
correlation ID, we can use the centralized logging service to find all related log events. If one of the log events also
includes information about a business-related identifier, for example, the ID of a customer, product, or order, we
can find all related log events log events for that business identifier using the correlation ID.

To be able to analyze delays in a call chain of cooperating microservices, we must be able to collect timestamps for
when request, responses, and messages enter and exit each microservice.

##### Solution requirements

- Assign unique correlation IDs to all incoming or new request and events in a well-known place, such as a
  header with a standardized name
- When a microservice makes an outgoing request or sends a message, it must add the correlation ID to the 
  request and message
- All log events must include the correlation ID in a predefined format so that the centralized logging service
  can extract the correlation ID from the log event and make it searchable
- Trace records must be created for when requests, responses, and messages both enter and exit a 
  microservice instance

### Circuit braker

##### Problem

A system lanscape of microservices that uses synchronous intercommunication can be exposed to a
**chain of failure**. If one microservice stops responding, its client might get into problems as well and stop
responding to request from their clients. The problem can propagate recursively throughout a system
landscape and take out major parts of it.

This is especially common in cases where synchronous requests are executed using blocking I/O, that is,
blocking a thread from the underlying operating system while a request is being processed. Combined
with a large number of concurrent requests and a service that starts to respond unexpectedly slowly,
thread pools can quickly become drained, causing the caller to hang and/or crash. This failure can 
spread unpleasantly quickly to the caller's caller, and so on.

##### Solution

- **Open** the circuit and fail fast(without waiting for a timeout) if problem with the service are
  detected.
- Probe for failure correction(also known as a **half-open** circuit); that is, allow a single request to go
  through on a regular basis to see whether the service is operating normally again.
- **Close** the circuit if the probe detects that the service is operating normally again. This capability is very important
  since it makes the system landscape resilient to these kinds of problem.

The following diagram illustrate a scenario where all synchronous communication within the system landscape of
microservices goes through circuit breakers. All the circuit breakers are **closed**; they allow traffic, except for one circuit breaker
(for **Microservice E**) that has detected problems in the service the requests go to. Therefore, this circuit breaker is open and
utilizes fast-fail logic; that is, it does not call the failing service and waits for a timeout to occur. Instead, **Microservice E** can
immediately return a  response, optionally applying some fallback logic before responding:

![alt text](img/ch1_figure_1.14.png)

### Control loop

##### Problem

In a large number of microservices instances spread out over a number of instances, it is very difficult to manually 
detect and correct problems such as crashed or hung microservice instances.

##### Solution

Add new component, a **control loop**, to the system landscape.

![alt text](img/ch1_figure_1.15.png)

##### Solution requirements

The control loop will constantly observe the **actual state** of the system landscape, comparing it with 
a **desired state**, as specified by the operators. If the two states differ, it will take action to make the
actual state equal to the desired state.

### Centralized monitoring and alarms

##### Problem

If observed response times and/or the usage of hardware resources becomes unacceptably high, it can 
be very hard to discover the root cause of the problem.

##### Solution

We add a new component, a **monitor service**, to the system landscape, which is capable of collecting
metrics about hardware resources usage for each microservice instance level.

##### Solution Requirements

The solution requirements are as follows:

* It must be able to collect metrics from all the servers that are used by the system landscape, 
  which includes autoscaling servers
* It must be able to detect new microservice instances as they are launched on the available
  servers and start to collect metrics from them
* It must be able to provide APIs and graphical tools for querying and analysing the collected
  metrics
* It must be possible to define alerts that are triggered when a specified metric exceeds s specified 
  threshold value

The following screenshot shows Grafana, which visualizes metrics from Prometheus, a monitoring
tool

![image 16](img/ch1_figure_1.16.png)

## Introduction to Spring Boot
### Convention over configuration and fat JAR files

Spring Boot targets the fast development of production-ready Spring applications by being strongly
opinionated about how to set up core modules from the Spring Framework and third-party
products, such as libraries that are used for logging or connecting to a database. Spring Boot does that
applying a number of conventions by default. Whenever required, each convention ca be overridden by writing
some configuration, case by case.

Added to the usage of convention over configuration, Spring Boot also favors a runtime model based
on a standalone JAR file, also known as a **fat JAR file**.

A fat JAR file contains not only the classes and resource files of the application itself but also all the
JAR files the application depends on. This means that the fat JAR file is the only JAR file required to run
the application; that is, we only need to transfer one JAR file to an environment where we want to run the
application instead of transferring the application's JAR file along with all the JAR files the application 
depends on.

Starting a fat JAR requires no separately installed Java EE web server, such as Apache Tomcat. Instead,
it can be started with a simple command such as *java -jar app.jar*.

### The magic @SpringBootApplication annotation

With the @SpringBooApplication annotation. The following code shows this:

```
@SpringBootApplication
public static void main(String[] args) {
  SpringApplication.run(MyApplication.class, args);
}
```
The following functionality will be provided by this annotation:

* It enables component scanning, that is, looking for Spring components and configuration
  classes in the package of the application class and all its sub-packages.
* The application class itself becomes a configuration class.
* It enables autoconfiguration, where Spring Boot looks for JAR files in the classpath that it can
  configure automatically. For example, if you have Tomcat in the classpath, Spring boot will
  automatically configure Tomcat as an embedded web server.

### Component scanning

Let's assume we have the following Spring component in the package of the application class:

```
@Component
public class MyComponentImpl implements MyComponent { ... }
```

Another component in the application can get this component automatically injected, also known as 
**auto-wired**, using the @Autowired annotation:

```
public class AnotherComponent {
  private final MyComponent myComponent;
  
  @Autowired
  public AnotherComponent(MyComponent myComponent) {
    this.myComponent = myComponent;
  }
}
```

```
NOTE: 
I prefer using constructor injection(over field and setter injection) to keep the state in my
components immutable. An immutable state is important if you want to be able to run
the component in a multithreaded runtime environment.
```

If we want to use component that are declared in a package outside the application's package, for
example, a utility component shared by multiple Spring Boot applications, we can complement the
@SpringBootApplication annotation in the application class with a @ComponentScan annotation:

```
package se.magnus.myapp;

  @SpringBootApplication
  @ComponentScan({"se.magnus.myapp", "se.magnus.util"})
  public class MyApplication { ... }
```

We can now auto-wired components from the se.magnus.util package in the application code, for
example, a utility component named MyUtility, as follows:

```
package se.magnus.util;

@Component
public class MyUtility { ... }
```

This utility component can be auto-wired in an application component like so:

```
package se.magnus.myapp.services;
public class AnotherComponent {
  private final MyUtility myUtility;
  
  @Autowired
  public AnotherComponent(MyUtility myUtility) {
    this.myUtility = myUtility;
  }
}
```

### Java-based configuration

If we want to override Spring Boot's default configuration or we want to add 
our own configuration, we can simply annotate a class with @Configuration.

For example, if we want to set up a filter in the processing of HTTP request 
that writes a log message at the beginning and the end of the processing, we 
can configure a log filter, as follows:

```
@Configuration
public class SubscribeApplication {
  CommonsRequestLoggingFilter filter = new CommonsRequestLoggingFilter();
  filter.setIncludeQueryString(true);
  filter.setIncludePayload(true);
  filter.setMaxPayloadLenght(5120);
  return filter;
}
```

```
NOTE:
We can also place the configuration directly in the application class since the
@SpringBootApplication annotation implies the @Configuration annotation.
```

### What's new in Spring Boot 3.0

The most important new items in Spring Boot 3.0 are the following:

* **Observability**
  The new distributed tracing support for metrics and logging in previous Spring
  Boot releases. The new Observability API in Spring Framework v6.0 and also 
  a new module named Micrometer Tracing. Micrometer is based on Spring Cloud Sleuth.
* **Native compilation**
  Support for compiling Spring Boot applications to native images, which are 
  standalone executables files.
* **Virtual threads**
  Support for lightweight threads called virtual threads. Virtual threads are expected
  to simplify the programming model for developing reactive non-blocking microservices.

### Spring WebFlux

Spring Boot 3.0 is based on the Spring Framework 6.0 which has built-in support for developing reactive
applications. The Spring framework uses **Project Reactor** as the base implementation of its reactive
support and also comes with a new web framework, **Spring WebFlux**, which supports the development of
reactive, that is, non-blocking, HTTP clients and services.

Spring WebFlux supports two different programming models:

* An annotation-based imperative style, similar to the already existing web framework, Spring Web
  MVC, but with support for reactive services.
* A new function-oriented model based on routers and handlers.

Spring WebFlux also provides a fully reactive HTTP client, **WebClient**, as a complement to the existing
**RestTemplate** client.

Spring WebFlux supports running on a servlet container based on the Jakarta Servlet specification v5.0 or
higher, such as Apache Tomcat, but also supports reactive non-servlet-based embedded web servers
such as Netty.

The Servlet specification is a specification in the Java EE platform that  standardizes how to develop
Java applications that communicate using web protocols such as HTTP.

When we begin to develop our microservices as a container, port collisions will no longer be a problem.
Each container has its own hostname and port range, so all microservices can use, for example, port
**8080** without colliding with each ot other.

### springdoc-openapi

The Swagger specification from SmartBear Software is one of the most widely used ways of documenting 
RESTful services.

**springdoc-openapi** is a open source project, that can create OpenAPI-based API documentation at runtime.
It does so by examining the application, for example, inspecting WebFlux and Swagger-based annotation.

### Spring Data

The two core concepts of the programming model in Spring Data are **entities** and **repositories**. 

Even though Spring Data provides a common programming model for different types of databases,
this doesn't mean that you will be able to write portable source code. For example, switching the
database technology from a SQL database to a NoSQL database will, in general, not be possible 
without some changes in the source code!

#### Entity

An entity describes the data will be stored by Spring Data.

For example, an entity that will be stored in a relational database can be annotated with JPA annotations
such as:

```
import jakarta.persistence.Entity;
import jakarta.persistence.Id;
import jakarta.persistence.IdClass;
import jakarta.persistence.Table;

@Entity
@IdClass(ReviewEntityPK.class)
@Table(name = "review")
public class ReviewEntity {
  @Id private int productId;
  @Id private int reviewId;
  private String author;
  private String subject;
  private String content;
```

If an entity is to be stored in a MongoDB database, annotations from the Spring Data MongoDB
subproject can be used together with generic Spring Data annotations.

```
import org.springframework.data.annotation.Id;
import org.springframework.data.annotation.Version;
import org.springframework.data.mongodb.core.mapping.Document;
@Document
public class RecommendationEntity {
    @Id
    private String id;
    @Version
    private int version;
    private int productId;
    private int recommendationId;
    private String author;
    private int rate;
    private String content;
```

The **@Id and @Version annotations are generic annotations, while the **@Document** annotation is specific
to the Spring Data MongoDB subproject.

This can be revealed by studying the **import** statements; the **import** statements that contain
**mongodb** come from the Spring Data MongoDB subproject.

#### Repositories

Repositories are used to store and access data from different types of databases. A repository can be declared
as a Java interface, and Spring Data will generate its implementation on the fly using opinionated conventions.
These conventions can be overridden and/or complemented by additional configuration and, if required, some Java
code.

Spring Data also comes with some base Java interfaces, for example, **CrudRepository**, to make the 
definition of a repository even simpler. The base interface, **CrudRepository**, provides us with standard
methods for **create**, **read**, **update**, and **delete** operations.

To specify a repository for handling the JPA entity **ReviewEntity**, we only need to declare:

```
import org.springframework.data.repository.CrudRepository;
public interface ReviewRepository extends 
  CrudRepository<ReviewEntity, ReviewEntityPK> {
  
  Collection<ReviewEntity> findByProductId(int productId);
}
```

We use a class, **ReviewEntityPK, to describe a composite primary key.

```
public class ReviewEntityPK implements Serializable {
    public int productId;
    public int reviewId;
}
```

We added an extra method, **findByProductId, which allows us to look up **Review** entities
based on **productId** - a field that is part of the primary key. The naming of the method follows a
naming conventions defined by Spring Data that allows Spring Data to generate the implementation of
this method on the fly as well.

If we want to use the repository, we can simply inject it and then start to use it.

```
private final ReviewRepository repository;
@Autowired
public ReviewService(ReviewRepository repository) {
 this.repository = repository;
}
public void someMethod() {
  repository.save(entity);
  repository.delete(entity);
  repository.findByProductId(productId);
```

Spring Data also provides a reactive base interface, **ReactiveCrudRepository, which enables reactive
repositories. The method do not return objects or collections of objects; instead, they return **Mono** 
and **Flux** objects are reactive streams that are capable of returning either **0...1** or **0...m**
entities as they become available on the stream.

The reactive-based interface can only be used by Spring Data subproject that support reactive
database drivers; that is, they are based on non-blocking I/O. The Spring Data MongoDB subproject
supports reactive repositories, while Spring Data JPA does not.

Specifying a reactive repository for handling the MongoDB entity, **RecommendationEntity**.

```
import org.springframework.data.repository.reactive.ReactiveCrudRepository;
import reactor.core.publisher.Flux;
public interface RecommendationRepository extends ReactiveCrudRepository<RecommendationEntity, String> {
    Flux<RecommendationEntity> findByProductId(int productId);
}
```
