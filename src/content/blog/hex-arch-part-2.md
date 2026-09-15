---
title: 'Hexagonal Architecture - Structuring Java Applications'
description: "The decisions that are important for the architecture to survive"
pubDate: 'Apr 21 2025'
tags: ["hexagonal", "architecture"]
---


Understanding Hexagonal Architecture concepts is the first step. Applying it effectively to your codebase through a smart and practical structure is the next critical one. Getting this right means your team will navigate, evolve, and maintain the application with greater ease.

But what does a project structure look like for a Java Hexagonal application, and why does it matter beyond just organizing classes and files? This post dives into practical project structure options, emphasizing how you arrange your code is a fundamental architectural decision.

Let's start with the fundamental question: Why does a project structure matter? I've found two primary reasons, from my experience:

### Why Project Structure Matters

#### Navigation (Naming is Hard)

Naming is notoriously difficult. It's not just about finding the 'right' name initially; the chosen structure and naming conventions must make it easy for developers (including your future self) to quickly understand and navigate the application's key components and decisions.

#### Evolution

Software inherently evolves. Decisions made early on about structure, even seemingly minor ones, can become problematic over time. While you can prepare for evolution, anticipating every structural change is impossible. In my experience, the most significant struggles often arise from the inorganic growth of a project's structure. No IDE like Intellij or assistants like Copilot can save you from this one, or should I say not yet!?

### Choosing Your Structure: Self-Explanatory vs. Contextual

For a new application, I have categorized the structure into two groups: "**Self-explanatory**" and "**Contextual**" These are terms that I have come up with, so neither is superior or inferior, simpler or more complex. Their value lies in just how well they support the cohesion of different architectural concepts and the evolution within the context of your specific application and team.

#### TL;DR

Here's a quick summary to help guide your choice:

*   **Choose Self-Explanatory if:**
    
    *   Your app is small or short-lived
        
    *   Team is new to hexagonal architecture
        
    *   You want fast on-boarding
        
*   **Choose Contextual if:**
    
    *   You’re building a long-lived application
        
    *   You want strong architectural boundaries
        
    *   You’re comfortable with a learning curve upfront
        
    *   You have an experienced team who communicate, collaborate, and coordinate effectively and support them with mature workflows in CICD, Versioning, etc.
        

### The Self-Explanatory Structure

As the name suggests, this approach involves naming top-level packages or directories directly after the core concepts of hexagonal architecture: `ports`, `adapters`, and `core`.

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXc509xbXR1DDhMlXQ0ysXkVUn08biWoW7r2QUJoulH2ieue8PzfdU2bObrX0NdQiqjjKyxGUL8a0gye4APJyjK2uJZDXBo9zuXyfENKD1OBAowmHI7wjJeKlaJAbRMvxvdwYYb39g?key=rW1Nepa8mExZNmHN-b6xwzkP align="left")

I experimented with replacing the term `core` in a couple of projects because of the industry’s tendency to use `core` in so many other places - `core java`, `hibernate-core`, `spring-core` to name a few.

*   `domain` instead of `core`: If the average experience of the team is mostly around SQL, especially the big ones such as Oracle or MSSQL, they often interpret this name as a “database domain.” This confusion can be challenging for the first few sprints or months. I generally avoid using “domain” at the root level when working with a large team. Also, the `core` will most probably contain a domain model, and that would look like `domain` inside a `domain`.
    
*   `app` or `application` instead of `core`: I have had seasoned developers question this one. Their inquiries included: “Are we developing a mobile app?” and “Isn’t this whole thing the application? Why have an `application` package?”. These terms are not from the hexagonal architecture but from clean and onion architecture patterns and inherit some of the features from those patterns. Whenever I have used `application`, `ports` sounded more like use cases. Whenever I have used `core`, `ports` sounded more like domain abstractions or domain services - closer to the business model, and less about orchestration. For a new application, I suggest you pick this only if you like it and the name “core” is not an option.
    

#### Trade-offs of Self-Explanatory: Sprawl and Erosion

However, the self-explanatory approach has trade-offs, primarily related to the friction it creates (or rather, lacks) for the architecture's evolution. I have listed two that I have found consistently.

##### Trade-off 1: Package Sprawl

Developers (myself included) prefer to focus on solving the core business problem rather than spending excessive time at seemingly trivial structural decisions. Naming a new package often falls into this category - trivial enough that spelling mistakes get into production. I myself have asked this quite a number of times in my career thinking; “What’s in a name?”. My answer nowadays - ask yourself in three months.

If you've ever looked at a project and wondered about the reason for numerous top-level packages, you've seen an outcome similar to this approach. The tendency to create a package or directory at the root for various concerns that don't fit neatly into `ports`, `adapters`, or `core` is very strong in this approach.

This leads to what I call "**package surfing**": and I define it as **the constant need to scroll up and down, checking package declarations to understand where a class lives and its relationship to others**. Just like a surfer riding waves, you find yourself going up and down in a “sea” of directories, which makes understanding the structure difficult, resulting in poor readability and slows you down.

For example, it's common to create `commons` or `utils` packages, and the root seems the easiest place for them to live, at that time! Where do Spring configuration files go? Not in `core`, `ports`, or `adapters`, so let's create a `config` package, for now! The list goes on: `constants`, `dto`, `mapper`, `exception`, to name a few more. In a fast-paced development environment, these decisions are made quickly, and maintaining a coherent structure becomes difficult. We might think refactoring is easy with modern tools like Intellij and copilots etc., but believe me – they are no match for deep structural issues.

##### Trade-off 2: Architecture Erosion

Another trade-off, one which is not quite obvious but I have seen occurring over longer periods of time, and one which I see as more human-centric than technical, is that this structure inadvertently makes it easier for developers to overlook or violate key architectural principles important for hexagonal architecture.

Following principles are a couple that can be bypassed easily as the project evolves, gradually eroding architectural integrity:

*   **Anti-Corruption Layer (ACL):** A pattern used to isolate a system from another system’s model, protecting the integrity of your domain.
    
*   **Postel's Law:** "Be conservative in what you send, liberal in what you accept." This is used as a guiding principle for interfaces/contracts between boundaries.
    

Both trade-offs have pretty much nothing to do with the team, architecture, or technology themselves but they are a cumulative side effect of external pressures such as tight deadlines or development friction (or lack of friction). Simply put, this structure lacks the cognitive load or friction that encourages developers to take careful consideration of where code should live in relation to architectural boundaries. And that lack of friction isn't enough to fight against the evolutionary pressure towards disorganization, what we generally call the entropy of a system. Now can we completely stop the teams from bypassing the aforementioned principles by following the contextual structure - it’s hard to say for sure, but we need all the support we can get!

#### When to Choose Self-Explanatory

If you like to keep it simple and see how your application evolves, you can choose this one - by all means. If you are developing a small application, go ahead without a doubt. If you are aiming for a short-lived micro-service, and by that, I mean a couple of years maybe, and you do not want to overload the team with all the context and evolutionary principles, please go ahead and adopt this structure. There is a beauty to simplicity, and it is certainly worth fighting for.

*Note:* Moving `ports` into `core` does bring a slight improvement to the structure, but I have seen the sprawl and erosion trade-offs popping up nonetheless.

### The Contextual Structure

This approach to structuring a project is primarily aimed at slowing down the team just enough so that the structure forces the team to think where a particular component in the application should go. And that is why it is named contextual - you have to apply context before you can start changing the application.

Let me start by saying this: as soon as you pick this structure - the team’s velocity will take a hit the first few sprints or months. But the structural integrity that this structure provides is a pretty strong one - this supports a controlled evolution, clear boundaries, and better tests. If you have a long-lasting project and you want to stick to hexagonal architecture as long as you can, I highly recommend picking this one.

There are two varieties that are listed below:

#### Contextual Structure: Single Module

The root directories in this variety are `core` and `infrastructure`. While hexagonal architecture does not prescribe the term `infrastructure` explicitly, I started using `infrastructure` as a way to separate the concepts right from the beginning. And I took it from my experiences with onion and solid architecture patterns. It also gives us good direction to apply fitness functions - particularly in Java and Python projects. More on that in a later blog post.

##### Why Single Module Contextual is Better

One of the key wins here is that this structure allows the team to maintain the purity of the domain. This means that the `core` can be decoupled (with a few hacks/tricks, if needed) from all the external concerns, that are neatly contained in `infrastructure`. The `core` will contain just the `ports` and domain services written in pure language-specific features and nothing more. It’s even possible to have no loggers, `@Service`, `@Component`, etc., within `core`.

This clear boundary between `core` and `infrastructure` not only enforces separation of concerns but also allows for extreme testability and portability. In fact, with careful discipline, the `core` module can often be tested and evolved independently of any specific frameworks or technical choices.

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXegxrLDiF__dqHZ5Bvo17YUphXunlPbeFALtFJUqyr2dvsEP9zALzZuivFTgDFzGaAb3eecMr0AAWWNbFeuBB2YwJVl_iYZko9pBqfGtSDujFDLw1OGud01n0pVrj-fg5PYRHG0xA?key=rW1Nepa8mExZNmHN-b6xwzkP align="left")

##### **Naming Conventions: API and SPI vs Inbound and Outbound**

One of the trade-offs, however, is the ambiguity of the terms `inbound` and `outbound` for ports and adapters alike. The architecture prescribes terms `driven` and `driver` for the inbound and outbound ports respectively, but I never found them as nice package names. Since both `ports` and `adapters` can have `inbound` and `outbound` qualifiers, I have substituted the port qualifiers with `api` and `spi` in a few projects.

*   **API = Application Programming Interface:** This will contain all the `inbound ports` that will be used by various adapters. These are the entry points into the domain and serve as contracts to invoke a capability within the domain.
    
*   **SPI = Service Provider Interface:** A common term in core Java. It’s heavily used in mechanisms like `JDBC` and `JNDI` to dynamically load extensions at runtime. In this structure, the `spi` package contains all the `outbound ports` - interfaces defined by the domain that are implemented by `outbound adapters`. The dynamic wiring happens in the `infrastructure` layer but remains invisible to the domain, preserving its independence and purity.
    

*Note:* I have seen short forms like `in` and `out` instead of `inbound` and `outbound`, and they work great in Java. But if you are using Kotlin, where `in` and `out` are keywords, this will cause the package declarations to contain a backtick (\`) and I just don’t think that’s a good idea!

#### Contextual Structure: Multi-Module

If you're using `Gradle` or `Maven`, then splitting your project into multiple modules is one of the most practical ways to enforce architectural boundaries in a hexagonal architecture. It’s also the point where things start to smell a lot like Clean or Onion architecture - not just in theory but in the actual structure of your codebase.

Instead of a single monolithic project where packages and internal directories suggest separation of concerns, a multi-module setup physically enforces it. Modules like `core`, `app`, and `adapters` can’t accidentally reach into each other — unless you explicitly let them. That means tighter boundaries, fewer accidental dependencies, and cleaner domain logic.

##### Why Multi-Module Contextual is Better

*   You can’t import `infrastructure` logic into your domain “just because it’s convenient.” You’ll have to think about where things belong.
    
*   Tests are cleaner. With `Spring`, monolith-style projects often rely heavily on `@MockBean` soup. But in multi-module setups, you often test real things with fewer mocked dependencies and more focused unit and integration tests.
    
*   It forces interface-driven design. Infra code implements interfaces defined in `core` or `app`, keeping domain logic pure and decoupled.
    

##### Common Multi-Module Structures

1.  **Two-tiered:**
    
    *   `core`: Your domain, use cases, ports. No `Spring` here.
        
    *   `infrastructure`: All adapters, implementations, framework glue. Good for small-to-medium projects or teams starting out. Keeps things separate but still manageable
        
        ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1745290443462/b27b4ab9-6fa7-4b80-9f88-507e02b00500.png align="left")
        
2.  **Multi-Tiered:**
    
    *   `core`: Pure domain logic, ports, domain models.
        
    *   `app`: Application layer (use cases, orchestration logic, sometimes `Spring` config).
        
    *   `adapters`: HTTP controllers, database access, message listeners — all implementing ports from `core`.
        
        ![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXfVZh2gFvxgD48ib9BDbhzaukXQ4OIWE-Jge5Xbslt6SKkJebc9OJTayZxBDToYEu-DTooMtUH8W0_A4LvdHZQdHeu5hjQ9rHtsxMrzSYBvYectDJUt9-LQvZ07TVQxpM9DVQD-?key=rW1Nepa8mExZNmHN-b6xwzkP align="left")
        

Both tiers offer excellent separation and can scale well with large codebases and teams. There are a few considerations give below, however these aren’t deal-breakers - just trade-offs worth knowing and considering. As mentioned earlier, this structure is where we begin stepping into Clean and Onion Architecture territory, which brings along many benefits - ones I won’t cover in detail in this post, but are certainly worth exploring further.

##### Considerations for Multi-Module

Here are some factors to consider before adopting a multi-module structure:

###### **Build & Tooling Overhead**

*   Multi-module gradle builds can be slow and flaky without good caching and incremental compilation.
    
*   Misconfigured dependencies can lead to circular references, versioning nightmares, and mysterious errors like `BeanNotFound` or `DuplicateBeanFound`.
    

###### **CI/CD Maturity**

*   Does your CI support partial builds, dependency caching, test splitting? If not, you will lose fast feedback loops from your “trusted” pipelines.
    
*   Versioning nightmares - imagine you have to hotfix a module, but your `core` is intact. Do you bump the whole app, or just the module or the module and `core` and `app`? Great, all of this documented, remind me where is this documentation /s?
    

###### **Team Coordination**

*   More modules = more boundaries = more coordination = less time to develop.
    
*   Changes across modules can slow you down unless teams are aligned and interfaces are stable.
    

###### **Test Complexity**

*   This slows down integration testing quite a bit, and in new projects where the interfaces are shaping up, this can result in a refactor loop.
    
*   Testing the whole application with the bean and dependency wiring using Spring or an equivalent is trickier. This friction will often result in higher CI costs.
    
*   Test architecture and reusable helper methods - object mothers, test doubles, quick and dirty mappers - all take a toll or multiple tolls.
    

#### When to Choose Multi-Module

In summary, a multi-module project structure is a good fit if:

*   The app is medium to large with complex domain logic.
    
*   The team values strict physical boundaries.
    
*   An existing CICD infrastructure with a high degree of maturity exists.
    
*   Communication, Collaboration, and Coordination are established capabilities within and between teams.
    

#### When Not to Choose Multi-Module

A multi-module structure is not a good fit for:

*   Apps that are small, in a prototyping phase, or targeting an MVP.
    
*   Single or small team targeting high iteration speed.
    
*   CICD processes are not established or are fragile and slow.
    
*   One team who does everything in the project - let’s save them!
    

### Conclusion

That wraps up this deep dive into structuring Java projects with hexagonal architecture. Yes, it was a long one, but hopefully, the emphasis on *why* structure matters made the length worthwhile. If you have questions, please leave a comment.

If you found this post helpful, please like and share. It really helps spread the word!

**A few references**

[https://beyondxscratch.com/2017/08/19/hexagonal-architecture-the-practical-guide-for-a-clean-architecture/](https://beyondxscratch.com/2017/08/19/hexagonal-architecture-the-practical-guide-for-a-clean-architecture/)

[https://www.amazon.ca/Clean-Architecture-Craftsmans-Software-Structure/dp/0134494164](https://www.amazon.ca/Clean-Architecture-Craftsmans-Software-Structure/dp/0134494164)

[https://blog.cleancoder.com/uncle-bob/2012/08/13/the-clean-architecture.html](https://blog.cleancoder.com/uncle-bob/2012/08/13/the-clean-architecture.html)
