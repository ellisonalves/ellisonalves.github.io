---
layout: post
title: Functional Interfaces in Javals
---

# Service discovery
It is a solution for discovering services in the cloud environment

-- Application services will fail so a service discovery will help in these cases

Service discovery 
- A way for a service to register itself
- A way for a service to deregister itself
- A way for clients to find other services
- Provides a way to check the health of a service and remove unhealthy service

## How spring cloud does it?
Spring cloud implements service discovery by:
- Spring cloud consul
- Spring cloud Zookeeper
- Spring cloud netflix (Netflix OSS + Spring + Spring Boot)
    - It is a collection of projects: SpringCloud Netflix Eureka Server, Netflix Eureka Client

### How all components work together?
There are 3 components in Service discovery:
- Discovery Server
- Server
- Client 

1. Application server starts up and calls the discovery server and register itself and tells its location and port
2. Client looks up service location
3. Discovery server knows where the server is and responds the location back
4. Client requests service at location (reaching its endpoints)
5. Service sends response

# Discovery Server
At this core, the discovery server is an actively managed registry of service locations. It is responsible for allowing others to find services and allowing application servers to register and deregister themselves.

Source of truth

Typically run one or more instances of the discovery server as it is the key component to locate all the other services. Important piece of the overall architecture because if can't find the other server you can't use them.

Spring cloud Eureka Server is an implementation of Discovery Server

# Application Server
This is whatever is providing the functionality.
It is receiving the request from clients and giving responses
It is a dependency for other services 
Typically run on one or more instances 
User of the discovery client. It is going to use that client to call the discovery server and:
    - Register
    - Deregister 

# Application Client
It is the piece that would call out to another "application service" to implement some piece of funcionality.
It is the issuer of the requests 
Depends on other services
Also a user of the discovery client but uses it in a different way. It doesn't use the discovery client to register or deregister anything.
- Use to find service locations

It is perfectly resonable for an application to be both a service and a client.
An application can be a service, which provides services to others and at the same time it can be a client, which depends on other services

# Configuration
Eureka server => the discovery server; contains a registry of services that can be discovered.
All configuration under eureka.server prefix.

Eureka client => anything that can discover services. Responsible for controlling how the discovery client interacts with the Discovery Server.
All configuration under eureka.client prefix

Eureka Instance => anything that registers itself with the Eureka Server to be discovered by others. It controls how the instance registers itself with the Eureka Server
All configuration under the eureka.instance prefix

# Eureka Server: Health and High Availability. Are my servers Healthy?
Eureka is always ensuring that application services are healthy and available and it also ensures that in the event that the Discovery Server goes down all the clients can still continue to operate.

- Regularly checks the status of the servers
- Clients send heartbeats every 30sec (default)
- Services removed after 90sec of no heartbeats (default)
- Can customize configuration to use /health endpoint  (eureka.client.healthcheck.enabled)

When a client requests a service location from the Discovery Server, the Discovery server actually sends back a copy of the registry. So, the registry is distributed (cached locally on every client) and if the Discover Server goes down, the services can still operate.

The client fetches deltas to update its local registry, meaning it only get the changes and never the full registry.




