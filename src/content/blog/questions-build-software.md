---
title: "How Questions Build Software"
description: ''
pubDate: 'Jun 27 2025'
tags: ["questions", "developers", "architecture"]
---

# How Questions Build Software

Looking back, I never thought I would build a career in software. My degree is in Chemistry, a field of precise rules and predictable reactions. What could a mind buzzing with ionic bonds and benzene rings understand about software?

That self-inflicted doubt was so strong that in my first year on the job, I didn't write much code at all. I chose to spend most of my time testing software that others built. I called myself a professional observer.

It turns out, learning to test was the best way to learn how to build software. Sure it took me a few years, but it taught me how to think in the long run, not just how to code.

## The Tester's Lens: Thinking from the Outside-In

When you test a simple login screen, you don’t just check the happy path. You probe the edges. What about an incorrect password? A non-existent username? What if a field is left blank? What if someone enters 10,000 characters? What happens if the database is down? You aren’t just testing a feature; you are mapping the uncharted territories of a feature.

This is ‘outside-in’ thinking. It’s the opposite of the ‘inside-out’ approach, where you immediately jump into implementation details without grasping the landscape the code lives in. The context is everything.

Imagine you need to send notifications to customers. An inside-out approach might lead you to loop through a list. But an outside-in approach forces you to ask questions first. How many customers? How fast does this need to be? Can the process fail and be safely restarted?

Asking these questions shifts your focus - from writing a for loop to designing a resilient system. Suddenly, the solution might involve message queues, retries, background workers, or batching. The context doesn’t just broaden your perspective - it shapes what’s even viable. It becomes the constraint and the canvas for the system’s design.

## The Persistent Problem: Why Smart People Make Silly Mistakes

As I moved into development, I realized that seeing the big picture wasn't enough; you still have to build the individual components. But even when I tried to balance both mindsets, I still made silly mistakes. I could not blame timelines or distraction or personal emergencies all the time, so I was forced down a path of reflection and reading. My search for answers led me to Daniel Kahneman’s work on our two cognitive systems:

* **System 1** is fast, intuitive, and based on patterns. It’s the gut feeling that a 500-line method needs refactoring. It’s quick and automatic.
    
* **System 2** is slow, deliberate, and analytical. It’s what you use to reason through trade-offs, evaluate design alternatives, or think about how another developer will see your code in six months.
    

The challenge is that software development, due to various internal and external factors, creates perfect conditions for System 1 to take over. Factors such as biases, opinions, experiences, hunger, emotions, weekend plans - the list goes on! We reach for familiar patterns and make quick decisions, often missing the bigger picture. Overcoming millions of years of evolutionary wiring is hard.

One of my strongest memories around this comes from my years-long pledge to become a polyglot. I was mugging up as many programming languages as possible during the early years of my career - to become a true polyglot. But my Python, Scala, JavaScript and everything in between looked suspiciously similar to Java. I was fluent in many languages but a native speaker in none.

During one code review in a Javascript project, a teammate asked why I wrote a complex, Java-esque list filter using prototypes. "All you had to do was a filtered map," he said. He was right. My System 1 had jumped for a familiar pattern, a remnant of my Java 6 OOP experiences, even though it was the wrong one for the job.

These aren't failures of human intelligence. They are the natural consequences of how our minds work under a heavy cognitive load. The real question is, how do we, as developers, train System 2 to show up when we need it the most?

## Writing Tests to Think Better

Test-Driven Development (TDD) is a powerful tool for enforcing System 2 thinking. Our natural impulse is to jump into code as soon as we see an opportunity. TDD interrupts this. The ritual of writing a failing test, making the minimal changes to make it green, and then refactoring forces deliberate, methodical engagement of our analytical brain.

But I must admit, my first reaction to TDD was that it was a conspiracy to slow me down. The core question it asks is "What's the smallest possible piece of functionality I can test?". It was inspiring, but frankly, I wasn’t ready for the deep probing it required. Instead, I bought into TDD for a more practical reason: my tests became better documentation. Before, my tests were often an afterthought and didn’t really tell much other than increasing coverage. With TDD, tests showed intent and context - not just coverage. Over time, I realized that my components and interfaces were becoming easier to adapt. Fast forward to now, TDD isn’t just about writing tests for me; it's a discipline that forces analytical thinking and clarity of thought.

This might also explain the endless debate around TDD. Perhaps experienced developers who resist TDD have already found other ways to engage System 2. But if you haven’t consciously been aware of your own thought process, TDD may be worth another look.

Asking tough questions alone is one thing. Doing it while someone else is watching - that’s a whole different kind of pressure.

## When a Question Becomes a Threat

Having someone watch me code used to feel like a nightmare. The fear of being judged was real. When I first tried pair programming, I approached it like TDD - focusing on the technical aspects. But unlike TDD, where I could force System 2 thinking through ritual and discipline, the questions during pairing sessions weren't working. The questions that made TDD so powerful - whether from me or my pair - felt threatening rather than showing curiosity.

It took me a while and a lot of reading to understand why. Our brain doesn't just optimize for familiar patterns; it also optimizes for safety. When we feel exposed or judged, we go into a defensive mode, rejecting questions that might reveal our uncertainty or ignorance. The very mechanism that makes pair programming powerful - having someone challenge your thinking - becomes counterproductive when trust and safety is absent.

This is where I learned that pair programming has a prerequisite that TDD doesn't: trust and safety. You need to feel secure enough to say "I don't know" or "I'm not sure about this approach". Without that foundation, the questions meant to engage System 2 thinking instead triggered System 1's defense.

So I started building relationships before I started building code. I'd spend time getting to know my pair - their communication style, their experiences, their preferred way of working through problems. This wasn't just being nice and jolly; it was creating the conditions where System 2 could actually function. When someone asks "Why did you do it that way?" it needs to feel like genuine curiosity, and not judgment.

The transformation was remarkable. Once that trust existed, pair programming became one of the most powerful ways to break out of autopilot mode. Your partner's questions force you to articulate your reasoning, expose your assumptions, and consider alternatives you might have missed. It's like having an external System 2 processor helping you think through problems.

This insight about trust and safety wasn't just practical. It pointed to something deeper about how teams function. Much later in my career, I learned there was a name for what I'd discovered: psychological safety, the condition where you can ask questions, admit uncertainty, and make mistakes without worrying about humiliation or punishment.

But this also revealed why pair programming can feel forced or ineffective. When it's mandated without attention to the relationship dynamics, you get compliance instead of collaboration. The social energy required to maintain both technical focus and interpersonal safety is immense - which is why effective pairing, like TDD, requires intentional practice and can't be sustained indefinitely.

## When Questioning Wears You Down

Asking better questions can transform how we build software. But questioning - especially the deliberate kind - has its own costs: cognitive, emotional, and social. When we adopt practices like TDD or pair programming, we’re not just writing better code - we're trying to slow down and force clarity. These practices succeed by injecting questions into a developer’s workflow.

Constantly asking hard questions - about purpose, design, or behavior - requires sustained attention. And attention is a limited resource. When we overuse reflective practices without acknowledging their limits, we risk burnout, fragility, or polite but unproductive collaboration.

Take TDD, for instance. It’s not a universal solution. You can't approach every layer - domain logic, database code, API contracts - with the same testing mindset. TDD is most powerful where behavior is crisp and intention matters. Asking the same questions in every layer blindly adds more ceremony than clarity. You have to keep asking: “What exactly am I validating here and how is it different from there?” And that takes mental effort.

Pair programming carries similar weight. It's built on questions - “Why did you choose this?”, “What if we did it another way?” - but those questions only help if there's trust. Without psychological safety, questioning can feel like interrogation. Dysfunctional pairing happens when one person dominates, the other withdraws, or neither feels safe being uncertain. You get silence or compliance, not true collaboration.

These aren’t reasons to abandon questioning. But they’re reminders that deep thought costs energy, and that our tools and practices only work when used with intention, not obligation. Build relationships. Take breaks. Recognize when reflection becomes overload.

## **The Questions That Matter**

Looking back at my journey from chemistry lab to the many software teams I worked with, I realize the common thread wasn't the transition from molecules to code - it was learning to ask better questions. In chemistry, precise questions led to predictable outcomes. In software, the right questions reveal the unpredictable complexities that make or break our systems.

The tester's mindset taught me to question assumptions: "What happens when this fails?"

TDD forced me to question purpose: "What's the smallest thing I can verify?"

Pair programming showed me how to question safely: "Can you walk me through your thinking?"

These aren't just development practices - they're frameworks to interrogate assumptions that pull us out of System 1's comfortable autopilot mode and into System 2's deliberate analysis. They transform us from developers who just write code, to designers who understand systems.

The art lies in knowing which questions to ask when. The power comes from creating environments - both technical and social - where those questions can be asked honestly. Whether you're debugging a failing test, architecting a notification system, or working through a complex problem with a teammate, the quality of your questions determines the quality of your solutions.

Your degree, your background, your fear of being judged - none of these disqualify you from building great software. What matters is cultivating the curiosity to question what you think you know, and the wisdom to create spaces where others can do the same.

We spend so much energy designing systems that check every dimension of technology. Our code handles edge cases and unexpected inputs because we know systems fail in unpredictable ways. What if we applied the same engineering rigor to our teams? What if we spent half that engineering effort on creating humane environments where questions are welcome, uncertainty is normal, and intellectual curiosity isn't punished but celebrated?
