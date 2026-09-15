---
title: 'Hexagonal Architecture - Introduction'
description: 'Exploring hexagonal architecture and why context matters more than syntax and structure'
pubDate: 'Apr 17 2025'
tags: ["hexagonal", "architecture"]
---

*You're reading part one of a series on hexagonal architecture. Over the next few articles, I will cover everything that I have encountered so far from the basics to advanced strategies, failures and wtf moments included.*

Originally named the "Ports and Adapters Architecture," and widely referred to as Hexagonal Architecture first started shaping itself out of ideas that were working behind the scenes as far back as the 1990s, but not until the early 2000s really took shape as a formal framework. That’s what the LLMs tell me anyways, Dr. Alistair Cockburn’s site is down and the first entry in archive.org is in 2009 🥺 . The aim of this architectural style was to further insulate the inside of an application from the outside world - be it databases, user interfaces, or other external systems.

I must admit, I came across this pattern fairly recently. For a number of early years in my career as software developer, I was highly committed to attempting to master as many programming languages as possible. Language upon language, syntax upon syntax, semi-colons and no semi-colons; in pursuit of the mirage that the more languages I could master, the greater developer I would be. But in time, I stalled at a humbling moment: I was just repeating the same structural patterns, the same mental models, over and over in every new language I acquired. In effect, I wasn't really learning anything. I was merely reimplementing the same application in different dialects. It felt like progress, but it wasn't. What a wake-up call.

To all coders who read this: mastery is not just knowing everything about the language or many things in many languages; it's learning why we do things the way we do. Learn the context under which architectural and design solutions are best. Learn the problems they were intended to solve. Don’t try to kill a mosquito with a bazooka!

Which brings me back to hexagonal architecture. Like all architecture styles, its value is not in simply doing it, but knowing when and why you should. The old adage applies once more: "It depends." Its relevance is largely dependent on the kind of problem you are attempting to solve, the complexity of your domain, and the kind of boundaries your application needs.

Since I’ve already tried applying hexagonal architecture in a variety of projects, across different domains and in multiple programming languages, you could say I’ve willingly signed myself up for a fair bit of architectural suffering. And that’s exactly what I want to write about. The good, the bad, and the “what-was-I-thinking” moments that came with it.

# Some basics

The closest analogy that I commonly see in other articles are the use of power outlets and plugs. As long as you have the same prongs, you are guaranteed electricity to charge your phone, if the grid is live. You travel to another country, buy an international adapter. All the concepts remain the same in this architecture - as long as you invoke the same inbound port, be it a REST call or a SOAP call or an undocumented untested protocol defined by a random IBM mainframe, the logic remains untouched and consistent and produces the same outcome. This predictability is a big deal!

The term *hexagon* has no real meaning in the architecture. The author simply found boxes and circles too dull for diagrams and opted for a six-sided shape instead. There’s no deeper symbolic meaning , that I know of, behind the hexagon itself. I have tried to draw this out without a hexagon and it looks great! You are free to try it out, pen and paper anyone?

Here is a representation of hexagonal architecture without any hexagons in it. The arrows represent information flow or call stack.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1744942651761/e8884bc9-9ceb-4877-b48d-03510742af60.png align="center")

What *does* matter, though, is the goal of this architecture: to protect the core of your system the application logic, the business rules from the noisy, ever-changing world outside. This means isolating the core from UI frameworks, REST controllers, messaging systems, databases, and all other external concerns.

In hexagonal architecture, the core interacts with the outside world through clearly defined boundaries: a single entry point and a single exit point, known as the **inbound port** and **outbound port**, respectively.

*   If something outside the application wants to invoke or use a capability within the core, it goes through the **inbound port**.
    
*   If the application needs to communicate with or depend on something external like a database or message queue it does so via the **outbound port**.
    

**Adapters** are the components that live on the edge. They’re responsible for translating between the messy, real-world technologies and the clean, intention-driven interfaces defined by the core. Think of them as interpreters: HTTP controllers, File / Disk operations, database connectors, messaging clients - they all act as adapters.

# The big WHY?

Separation of concerns is an important aspect in software architecture. Mortal humans likes order, boundaries and control, however far fetched that might seem in the world we are in. Hexagonal architecture provides just that, separation of concerns through clear boundaries, an order in the flow of information and layered control of behaviours.

*   **Improved Unit Tests:** By isolating the core logic, you can test it independently of the complexities and potential instability of external dependencies like databases or UI frameworks. Unit tests become more focused and reliable.
    
*   **Improved Integration Testing**: Having clear boundaries between different layers helps to apply narrow integration tests, improves contracts between layers, promotes stubs. This makes it easier to test the interactions between different parts of the system without relying on expensive e2e tests.
    
*   **Increased Maintainability:** Changes in the external world (e.g., upgrading a database, switching UI frameworks) are less likely to impact the core application logic, as long as the adapters are updated accordingly. This reduces the risk of introducing bugs and makes maintenance easier.
    
*   **Enhanced Flexibility:** The application becomes more adaptable to change. You can swap out external dependencies without rewriting the core logic. For example, you could switch from one database to another by simply implementing a new adapter.
    
*   **Better Team Collaboration:** Clear boundaries make it easier for different teams to work on different parts of the application with less risk of interference, mostly at the same time. As long as the boundaries and contracts remain the same, there should not be major conflicts.
    

In the next article, I will cover a Spring Boot application and its project structure that would help us enable this architecture.
