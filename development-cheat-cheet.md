# Some quick notes about software development


## Cloud Architecture

 Topic        | Description   | Reference 
 -------------|---------------|-----
  Web Services | Very small service which provides a single functionality with it's own database or table and which were consumed by other services. Used in service oriented architecture.
  Microservices | Small services which have there own database and but sharing tables by an ID. They are tied by the business domain. They are connected with an event bus.  
  SOA | Service Oriented Architecture was popular between 2003 and 2009. Reusabal web services with SOAP, REST or RPC was the key focus. 
  DDD | Domain Driven Design is currently very popular. It the design pattern which Microservices following.
  Event Sourcing | It is used to store events in a database table which is more or less a state of a system. An event could be a set of change events of all microservices. It is possible to recreate a state from the past to reproduce or analyze bugs. EventStore is currently very popular tool and a key component of DDD.


## RabbitMQ

 Topic        | Description   | Reference 
 -------------|---------------|-----
  vhost      | Used to separate tenants virtually so that they have their own RabbitMQ instance with queues, etc. A physical alternative is to host multiple RabbitMQ instances.| https://www.cloudamqp.com/blog/2020-09-10-what-is-a-rabbitmq-vhost.html
  Exchange | Used by the producer as single point where the events gets delivered to. An exchange can be connected between 1 or N receiver queueus. 
  Queue | Used by the receiver of events. After reading and acknowledging the reception the event will be purged from the queueu.
