---
title: 'The Last Unassisted Generation'
description: ''
pubDate: 'Jan 14 2026'
tags: ["adaptive", "architecture", "understanding", "zones"]
---

# The Last Unassisted Generation

The introduction of AI-assisted and Agentic coding makes me think about the history of agriculture, specifically the shift toward monocultures.

By increasing harvesting efficiency to feed billions and being successful at it, we removed the genetic diversity that crops achieved through thousands of years of adaptation. By optimizing for yield, profit, and predictability, we traded off adaptability, resilience, and diversity. When the Irish Potato Famine struck, a million people died because a single cultivar couldn't resist a pathogen.

We optimized for what we could measure—yield and velocity—and lost what we couldn't: the distributed knowledge of millions of farmers making small, careful selections over generations.

We are doing it again. This time, with how we think.

## The Flattening of Understanding

In a previous post, I wrote about how [your application needs different zones for different levels of understanding](https://akdev.blog/your-application-needs-different-zones-for-different-levels-of-understanding). I argued that we must protect the "Core" of our systems—the parts where we require absolute clarity—while allowing the "Edge" to be experimental and volatile.

AI is effectively **flattening these zones.** Because AI can generate code for the "Core" as easily as it can for a "Feature Toggle," it creates an illusion that we understand both equally. It allows us to build the foundation of our systems using the same low-friction, low-understanding methods we usually reserve for prototypes.

## The Paradox of Velocity

AI is extraordinary at accelerating what you know *and* what you don't know. It generates code faster than we can build the mental models to evaluate it. This creates a dangerous inversion: **As the time to generate code shortens, the feedback loop between mistake and consequence actually lengthens.**

In the past, the speed of writing code was throttled by the speed of understanding it. If you didn't understand the logic, you couldn't write the syntax. You felt the friction immediately.

Today, we get instant gratification—code compiles, tests pass, CI is green. But we get delayed comprehension. The bug surfaces in production, weeks later, in ways that obscure causation. Short-term success breeds long-term fragility.

I saw this play out recently. A developer followed TDD, wrote tests, and got green lights. The AI-generated code was syntactically perfect and worked flawlessly in local development.

But the AI had made a subtle error: it stored temporary state in a local variable. In an ephemeral, containerized environment, that state vanishes the moment the pod scales or restarts. Because the generation was so fast, and the code looked so "right" (even to human reviewers scanning for syntax rather than architecture), more PRs were merged on top of it.

This is a textbook example of **Doyle's Catch**.

Named after engineer John Doyle, the concept warns that as our tools for simulation and modeling become more powerful, the gap between a successful "demonstration" (local dev) and the real world (production) actually widens and becomes harder to see.

AI makes the "demonstration" phase trivial. We can generate code that passes tests and looks correct in seconds. But because the generation was so effortless, we assume the bridge to the real world is equally solid. It isn't. The AI optimized for the idealized environment of the prompt, not the hostile reality of the real world systems.

## The Ironies of Automation

In systems engineering, the **Ironies of Automation** teach us that the more reliable an automated system becomes, the more crucial the human operator becomes for the rare moments it fails.

When AI fails, it doesn't fail like a human. It doesn't get "tired"; it hallucinates with confidence. It creates solutions that violate unstated architectural constraints—the "invisible foundations" that live only in the mental models of the engineers.

If nobody in the room has the manual "tracing" skills to diagnose these failures without the tool that created them, the failure will cascade. We are effectively creating a "Cognitive Lock-in," where we are dependent on the vendor not just for the tools, but for the thought process itself.

## The Responsibility of the Bridge

I am part of the last generation of engineers who developed entirely without AI. We learned through friction. We broke things and read cryptic error messages. It wasn't that our code was inherently "better"—we wrote plenty of spaghetti—but the high cost of iteration forced us to build "Zones of Understanding" in our heads. We *had* to simulate the system mentally because we couldn't just regenerate it.

In the new reality, these zones of understanding are fading. I am afraid that our solution to the pressure of AI is to "rewrite code" every single time because we no longer understand the existing codebase enough to refactor it. That is not a sustainable model; we will never catch up to the pace at which negative feedback loops occur.

Once the pre-AI generation retires, the instinct for "architectural smell"—the ability to look at a piece of code and know it will cause a crisis in six months—doesn't automatically transfer. It only survives if we create intentional "Seed Vaults" of knowledge.

## What This Looks Like Practically

We can't withstand the economic pressure to fully slow down. But we can design systems that restore essential feedback loops without abandoning AI's benefits.

**1\. Staged AI Access (Calibrating for Risk)**

This needs the lens of risk management, and not gatekeeping or privileges.

* **Entry Level (Spec-First):** You cannot generate the solution until you can articulate the problem. Write the spec and decompose the data flow manually *before* engaging the AI.
    
* **Senior Level (Resilience Ownership):** Unrestricted AI use, but you carry the burden of the team's comprehension. If you ship complex, AI-generated patterns that your team cannot debug or make sense of, that velocity was borrowed, not earned.
    

**2\. Comprehension Gates**

Before merging AI-generated code, the author must be able to:

* Explain the flow of information, data and decisions in their own words.
    
* Identify the most likely failure mode.
    
* Describe the recovery plan if it fails in production. If you can't do this, you don't understand the code well enough to ship it.
    

**3\. The "Manual Override" Drill**

At least once a quarter, run an entire sprint (or a specific critical feature) without AI assistance. Managers often push back on this due to velocity concerns. The counter-argument is simple: disaster recovery for augmented capabilities. If Github Copilot goes down, or we change vendors, or the model starts degrading, does the team grind to a halt? If yes, you are a security risk.

**4\. Failure Retrospectives with Context**

When AI-generated code fails:

* Document the prompt, the output, why it seemed right, and why it failed.
    
* Turn failures into teaching moments about *architecture and practices*, not just syntax.
    

Ideally, future models with persistent memory and project-level context will handle this "context repair" automatically. But until the tools can effectively articulate their own reasoning back to the team, our social systems must be stronger than our technical ones.

## Be the Seed Vault

We may be the only ones who understand both worlds: the friction of the manual struggle and the speed of the augmented future. Our role is not to guard the past, but to secure the future.

We must act as the bridge. We must ensure that as we move toward a world of "instant code," we don't lose the "earned understanding" that keeps those systems standing. We must become the Seed Vaults—preserving the genetic diversity of thought required to survive the next famine.

*Note: Parts of this post were refined using Gemini and Claude to sharpen the articulation.*
