---
title: 'Hexagonal Architecture - Understanding ports'
description: 'Exploring hexagonal architecture and why context matters more than syntax and structure'
pubDate: 'May 23 2025'
tags: ["hexagonal", "architecture"]
---

# Hexagonal Architecture - Understanding Ports

**What are Ports and Why Do They Matter?**

In Hexagonal Architecture, ports are an important part of the core of the application. They are essentially interfaces or contracts that govern the interactions between your application's core logic and the outside world. Their primary purpose is to protect this core business logic from all the external details it might need to interact with.

What do we mean by "external details"? This refers to any component or technology that isn't part of the core business logic itself, such as HTTP controllers, database interaction mechanisms (such as JDBC or an ORM), integrations with message queues, or clients for third-party APIs. The core should remain ignorant of *how* data arrives or *how* it's persisted. By clearly defining these interaction points as ports, we can achieve a more decoupled, maintainable, and adaptable system.

To better understand how ports fit in, let's look at a diagram illustrating the interactions and dependencies within Hexagonal Architecture.

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcR1TG0_d1UtisbTO0ke8HR7GXqcfl6_-sKpjUk7l8m9nLZgPk0omyZ9r3-N0_ENSOaWmDGwWSAoGcQ4Hazs-KhNWoHbaOpIOokZ23qzaSX0gMFWlFUP8BxsWdgHxA35AfqT-lbgA?key=nFhxI6_Q8dZxiYKwCHXuuQ align="left")

* The **Core** contains the application's business logic.
    
* **Driver/API** and **Driven/SPI** represent the ports themselves. 
    
* **Inbound Adapters** (e.g., HTTP controllers) use the Driver/API ports to send commands or queries to the core.
    
* **Outbound Adapters** (e.g., database connectors, external service clients) implement the Driven/SPI ports that the core uses to send data or request actions from external systems.
    

Notice the direction of the arrows – they all point towards the core. This is the Dependency Inversion Principle from SOLID, which states that low-level modules should depend on high-level modules, not the other way around. In this context, low-level modules are the concrete, technology-dependent parts like HTTP Controllers (an inbound adapter) or JDBC implementations (an outbound adapter). The high-level module is the abstract representation of the real world containing the business logic - that's the core.

Any cohesive unit of capability in the core should only be accessed through an API (inbound) port, and any external interactions the core needs must only be implemented as per an SPI (outbound) Port.

Ports facilitate two primary directions of interaction:

* **Inbound Interaction:** The outside world (users, other systems, automated tests) initiates communication with the application's core logic.
    
* **Outbound Interaction:** The application's core logic initiates communication with external systems (databases, messaging systems, third-party services).
    

**Inbound Ports (Driver/API Ports): The Gateway to Your Core Logic**

Inbound ports define how external actors can invoke the application's use cases or capabilities. In most programming languages, these ports are implemented as interfaces.

**Naming Conventions for Inbound Ports** 

Effective naming is key for clarity. Two common types of naming conventions that I have used are

**Use-Case Naming:**

Names that are derived from the use cases provided in requirements and used by the business in the real world. Examples include `CreateOrder`, `CreateOrderPort`, `OrderCreationService`, `ForOrderCreation`.

**Pros:** Excellent for smaller applications due to high readability.

**Cons:** Scales reasonably well up to a point, after which you might face too many ports or require refactoring.

**Bounded Context Naming:**

This convention groups related use cases into a single port, representing a specific bounded context from Domain-Driven Design (DDD). This helps organize ports in larger systems. It follows a domain-driven design approach. An example is `OrderManagementPort`.

**Pros:** Ideal for larger applications and aligns well with DDD principles.

**Cons:** Can have slightly lower initial readability compared to use-case naming.

A third style, **Actor-based naming**, is not covered in detail here. Please leave a comment if you would like to know more

These naming strategies help group methods or behaviours for adapters and keep the system aligned with real-world use cases or business language.

**Defining the inbound contract**

Since an inbound port is an interface and a contract, it must contain methods for the use cases or capabilities it exposes. Consider an order placement application that receives an HTTP request to create an order in a database.

Here's a sample inbound port in Java for creating an order:

```java
public interface CreateOrder {
    OrderCreated createOrder(CreateOrderCommand command);
}
```

Let's break down the input and output:

* **Input:** The `createOrder` method takes a `CreateOrderCommand` object. This is a wrapper class containing all necessary attributes to create an order. It's called a "Command" here because this behavior is asking for a change in the application's state, which is a DDD principle (related to Command-Query-Response). You can also use `CreateOrderRequest` or `CreateOrderDTO` if those are more familiar to you.
    
* **Output:** For the `CreateOrder` behaviour, a successful response is named `OrderCreated`. This follows an event naming convention, indicating something that has happened in the past ("Do something, the adapter requested, core responds I did"). You can also use names like `CreateOrderResponse` or `OrderDTO` if those are more familiar to you. 
    

Sometimes, it's worth checking your team's "collective finger muscle memory" for naming.

**Outbound Ports (Driven/SPI Ports)**

Driven Ports, also known as Secondary Ports or Outbound Ports, define the contract for how your application's core logic interacts with external systems or infrastructure concerns. Think of them as the interfaces that your application *needs the outside world to implement*. These ports are on the "right side" or "driven side" of the hexagon, representing services your application *consumes*.

The primary role of a driven port is to abstract away the specific details of external dependencies. Your core application logic shouldn't care if it's communicating with a PostgreSQL database, a REST API, a message queue, or a simple in-memory store for testing purposes. It only cares that *something* exists that can fulfill its request, as defined by the port's interface.

Outbound ports are often defined in terms of your domain objects, not in terms of the technology used by the external service. For example, if your application manages Order entities, you'll likely define an `OrderRepository` or `OrderDataGateway` port. Here is an example

```java
public interface OrderDataGateway {
    public void save(OrderData order);
    public OrderData findById(FindOrderData order);
    public List<OrderData> findAll();
}
```

This approach keeps your core domain pure and free from infrastructure leakage, and that is the first step to anti-corruption layer. These ports frequently mirror key entities or aggregates within your domain model.

**Naming Conventions** 

Again, context matters! Here are a few tips to name the outbound ports.

* **Repository:** This term is popularized by Domain-Driven Design (DDD), is commonly used when the port specifically handles the persistence and retrieval of domain aggregates, mimicking a collection of entities. An `OrderRepository` is a very common and descriptive name if its sole purpose is managing Order persistence.
    
* **Gateway:** This term, like `OrderGateway`, describes a point of entry/exit for data or functionality related to a specific domain concept. It's a broader term that can encompass more than just data persistence. For instance, an `OrderNotificationGateway` wouldn't be a repository but would still be a driven port.
    
* **DataGateway:** Adding "Data" as a prefix, like `OrderDataGateway`, can be useful to be more explicit that this gateway is primarily concerned with data access, distinguishing it from other types of gateways (e.g., a `OrderNotificationGateway`).
    

Choose a convention that makes sense for your team and stick to it. The goal is to make the port's purpose immediately understandable.

**Best Practices for Designing Port Interfaces**

Whether defining inbound or outbound ports, certain practices lead to more robust and maintainable contracts.

**The Power of Wrapper Objects (Commands, DTOs, Request/Response Objects)** 

Using well-defined Data Transfer Objects (DTOs) or dedicated request/response objects as parameters and return types for port methods is crucial. Always try to use wrapper types, POJOs (Plain Old Java Objects), or Command/Query objects in interfaces. Do not use primitives and too many arguments, as they have a high tendency to break the contract and cause significant refactoring effort.

Let’s look at these two methods

```java
    public void save(String customerId, List<String> productIds, double totalAmount, Instant orderDate);
```

versus

```java
    public void save(OrderData order);
```

Which is better for clarity? What if you want to add a new attribute? Will you remember the order of attributes in the first method?

And Interface Segregation Principle from SOLID must be mentioned here - no client should be forced to depend on methods it does not use. This encourages the creation of smaller, more specific interfaces. This allows a variety of techniques to allow mixins, composition and strategies in your core that are just elegant (and more fun to work with), while addressing the requirements.

**The Value of Ports**

By adhering to these principles, your inbound and outbound ports become robust, maintainable contracts. They effectively decouple your application core from the never ending framework upgrades, database migrations while improving testability and evolution of the application. 

Well-defined ports are a cornerstone of building flexible, long-lasting software systems with Hexagonal Architecture. 

In the next post, we will explore the core of hexagonal architecture.
