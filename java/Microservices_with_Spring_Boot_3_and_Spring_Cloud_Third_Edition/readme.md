## Design patterns for microservices

The concept of design patterns is actually quite old; it was
invented Christopher Alexander back in 1977. In essence, 
a design pattern is about describing a reusable solution to a 
problem when given a specific context.

### Service Discovery

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