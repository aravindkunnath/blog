---
title: 'Adaptive Architecture'
description: 'Your Application Needs Different Zones for Different Levels of Understanding'
pubDate: 'Jan 09 2026'
tags: ["adaptive", "architecture", "understanding", "zones"]
---

Two related problems that I have consistently seen in teams are

1. Architectural decisions made before understanding the domain become increasingly difficult to change. Technically, organizationally and psychologically difficult. Teams carry these decisions like a weight.
    
2. This weight manifests as regret. Not the mild 'we could have done this better' kind. The kind that slows teams down, creates friction, and makes every subsequent decision harder.
    

I've also seen the inverse - teams that prioritize explicit learning over uncertainty or premature certainty are able to deliver smoothly. With such teams, the software and its evolution follows the path of the team's understanding. Much like Peter Naur's theory building, the shared mental model produces the system's attributes. Software in that sense is an emergent artifact.

Following is one of many real-world project experiences that I have, where I actively learned about this.

This was a project of 4 pairs of developers, and we had a somewhat rigid deadline of 3-4 months. Before I joined, the team and the architect decided to implement 3 services for a system - one for accepting messages, another one for sending out reports and the third one was a CRUD service for managing some configuration and metadata. Because of budget constraints and organizational policies around having a single writer for a single database instance, it was decided to use one database. Since the data consistency requirements were pretty strict, it was decided that the third service will be hosting an HTTP API and the other two services would use a synchronous API call to fetch/persist data. I inherited these decisions and watched the team struggle with them.

It took us 3 months to complete all the initial requirements and go to production. Once it had gone into production, we started seeing some challenges with this architecture. In hindsight, the signals were there even before we went to production, but we were too busy getting things done. The challenges were

1. HTTP semantics was leaked into the model
    
2. API calls took more than 300ms most of the times and we did not have any control over the overall API gateway setup
    
3. When minor new requirements to add a few flags into the database came in, we had to start updating the models in all three services. We were already in a coupled deployments mode, so this was not going to fix much.
    
4. The team was distributed and required increasingly more communication to synchronize changes.
    

We were learning that the architectural decisions of the past were biting us. All of this had taken around 3 months to build. And we were learning from our mistakes. But we had done something right, we had a pretty good handle on our testing. Our tests were largely behavioural. Though there were some meet-the-coverage tests, we had built a good suite of tests that we trusted on. A short internal discussion with the team screamed frustration around the way changes had to be done. And then I reframed it - 'What if those three months were learning? What can we do with that knowledge?'. To all of us, the obvious option was "remove the service that does not work for us". We did a back of the napkin estimation of what we would need to do to change the architecture, and rewrite/refactor the whole thing. I had by then introduced the concepts of feature flags from evolutionary architecture to the team, so we devised a case, a plan and estimated a timeline of 3 weeks to change everything.

We had to convince leadership to approve a re-architecture of 3-month-old production code. Most organizations default to "make it work" rather than "let's redo it". We presented the facts as follows

* Every change takes 2 days instead of 2 hours due to coordination overhead
    
* APM shows that there is a consistent mean latency of more than 300ms . We have to address this before we scale up.
    
* We are here due to the policy, and we know a better way to address that policy
    
* We understand the problem and domain better now to solve this
    
* Feature flags is a guard rail and let us run both in parallel and roll back instantly"
    

And in 3 weeks, we

1. removed the CRUD service entirely
    
2. addressed the performance and scalability concerns
    
3. unified the data models across the rest of the two services and had clear idea of which service is responsibility for what
    
4. feature flagged the transition
    

3 weeks is all it took to change the approach to deploy it into production and in the fourth week we had switched the flag. Surprisingly, the team just knew what needed to be done. Since the organizational mandate to have one writer service had to be complied with, we designated the second service as the sole writer. The first service simply published messages to a topic rather than blocking on a synchronous HTTP call.

Was the initial architecture wrong? Yea, it was. Could we have known it beforehand? I am not sure. Almost everything about the technology, except some of the org policies, the team knew and were highly skilled at. But the team had not learned about the problem. Once they learned the problem, when there was a shared understanding, the development velocity changed unprecedentedly. The first three months were actually a learning phase that changed the way the team understood about the feature. Once learnt, their commitment and understanding of how to build the software had drastically changed. If we had called this phase as a learning phase explicitly, the organizational friction would have been much lower. The team psychology would have been different. We would have made different technical choices (simpler, easier to change). We may not have taken that whole 3 months to achieve the understanding and the architecture would have emerged without the frustration.

This also aligns with the 'Monolith First' principle - teams need to learn boundaries before committing. Rigid adherence to any architectural pattern or decision without explicit learning prevents the co-evolution of teams and systems that produce satisfactory software.

**What if we acknowledged that software exists in different states of team's understanding or learning - not like 'development' then 'production,' but states based on how well we grasp the problem we're solving?** Certainly, the entire application cannot be in learning state forever. But could we have different parts at different levels of certainty - some we're learning, some stable, some foundational?

I call this Adaptive Architecture (though the name matters much less than the concept right now). It has three distinct zones within your solution space:

1. **Learning Zone** - Where teams are discovering the problem
    
2. **Stable Zone** - Where understanding is proven and patterns are formalized
    
3. **Non-negotiable Zone** - Where aspects that are foundational and cannot be traded off lives
    

Each zone has different rules, different quality bars, and different expectations. The key is making these zones explicit and providing clear criteria for moving between them.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1767992915866/9160d4a7-81d6-4a7f-8498-8cd5f897a8e8.png align="center")

## Learning Zone

The Learning Zone is where teams discover what they will actually building. The priority is for the team to learn, and build the simplest thing that can teach you about the problem.  Product and engineering work closely, defining success criteria before writing code: What needs to be true for this to work? What signals will tell us if it's working?

Don't sacrifice quality, testability, or observability. Instead sacrifice architectural purity, code coverage targets, and fitting into existing patterns. Write behavioural tests that capture what the feature/capability does, not how it does it. These tests are important and become your safety net for refactoring or rewriting later. Use CUPID or light weight patterns. Don't try to abstract. Instead make your tests be simple, readable and overall - show that you are learning the expected behaviour.

There is only one goal in this phase - learn - for both product and engineering. This phase is over once the code is deployed into production and there is feedback about how it is working. Gather signals. If the implementation feels wrong, rewrite it. If the feature isn't valuable, delete it. You haven't committed yet.

## Promotion Gate

Once a feature proves its value in production, it goes through a promotion gate—a joint ritual between product and engineering. The questions are simple: Did we achieve the goals / expectations? Can we articulate the problem clearly? Can we name or rename concepts without ambiguity? Has the team built a shared mental model? Some of these are measurable, and some are not. And that is fine.

If the answer is yes, refactor or rewrite it into the Stable Zone. The behavioural tests from Learning become your safety net - if they still pass, you understand the behaviour. If they break, you missed something. Tests may need minor adjustments but a complete rewrite of tests means that you did not understand the behaviour.

Not everything gets promoted either. Failed experiments are removed and those do not touch the stable zone. Experiments that worked but need a different approach get rewritten—cheaply, because you haven't committed to the architecture yet. Only features that prove their value earn the investment in proper patterns.

## Stable Zone

The Stable Zone is where proven features live. Here, architectural patterns matter. SOLID principles make sense because you know what should be open/closed. DDD tactical patterns work because you've learned the actual entities. Bounded contexts can be formalized because you discovered where natural seams are.

Code here has proper structure, documentation, and fits the broader architecture. Changes are deliberate and well-tested. The quality bar is high because this code has earned the right to stay.

Features in the Learning Zone can depend on Stable Zone code, but never the reverse. Stable code cannot depend on Learning Zone experiments—that would make it unstable.

If a Stable Zone feature needs significant change, it returns to the Learning Zone first. You're learning again, so treat it as uncertain. Feature flags and evolutionary architecture patterns make this transition safe—the stable version keeps running while you experiment with the new approach.

## Non-negotiable Zone

The Non-negotiable Zone is where the platform lives. Infrastructure, observability, authentication, deployment pipelines, feature flags, monitoring - the foundational aspects both other zones depend on. This is also the set of foundational decisions that the team has to make and carry.

The quality bar here is also different: resilience, redundancy, careful change management, peer reviews. Changes are infrequent and heavily scrutinized. In Team Topologies terms, this is platform team territory - providing capabilities as a service to product teams.

This zone doesn't learn about the domain. It enables learning.

## Closing notes

This extends the principles from my earlier essay on [Empathetic Systems](https://akdev.blog/empathetic-systems-designing-systems-for-human-decision-making), where I argued for designing systems around human decision-making and psychological safety. Adaptive Architecture is how to operationalize those principles - giving teams explicit structures that acknowledge uncertainty, validate learning, and remove the burden of premature commitment.

Most of my regret for the software systems that I built accumulated from knowledge gained after deployment, when I'd already committed to decisions that weren't easy to revert. Adaptive Architecture is a framework that structures delayed decision-making for teams building under uncertainty. If your team is churning on decisions, the zones and language might help.

To the purist in you, this is not a new architecture. It's more of a meta-architecture or an architectural decision-making framework that codifies patterns I've observed across projects. When teams had explicit permission to learn before committing, delivery improved and regret decreased.

If you try this, even with just one feature, I'd welcome your feedback. What worked? What friction did you hit? I'm developing reference implementations and refining the framework based on real use.

In the next post, I hope to show you an example using Java and Spring Boot.
