---
title: "Who's Driving Your Architecture"
description: ''
pubDate: 'Jun 20 2025'
tags: ["developers", "architecture"]
---

# Who's Driving Your Architecture?

Ask any architect who drives their technical decisions, and you'll hear the usual suspects: "the product," "the business," or "the end-user."

I believe they're all missing the most critical factor.

End-users want working products. The business wants shipped features. Managers want predictable timelines. But there's one group that ultimately decides whether your architecture thrives or becomes technical debt: **the development team**.

Your developers aren't just stakeholders—they're your architecture's true customers. And most architects are building systems their customers can't actually use.

## The Vanilla Ice Cream Problem

No story captures this better than the classic industry tale of the vanilla ice cream car, for me.

A man wrote to an American car company complaining that his car wouldn't start whenever he bought vanilla ice cream. Chocolate, strawberry, any other flavour—the car started fine. Just vanilla caused this problem.

Most of the engineers in the company dismissed this as absurd. But one, an empathetic and curious engineer, decided to investigate. He accompanied the customer on his ice cream runs and discovered something subtle but critical: vanilla was the most popular flavour, so stores kept it at the front. Other flavours were stored in back and took longer to retrieve.

The extra time for non-vanilla flavours let the engine cool down. The quick vanilla trips meant the engine was still hot, causing vapour lock—a known issue that prevented restart.

The flavour was a red herring. The real problem was timing.

This is what software architects face daily: teams bring you "vanilla ice cream problems"—confusing errors, unexpected pain points, workflows that seem irrational, vague requirements from another team. The reflex is dismissal: "That's not how it's supposed to work" or "They just don't understand the design." or "Just create another API"

But here's what the car engineer understood: **assume they're not wrong; assume you're missing something.**

## The Hidden Cost of Developer Friction

The flaws that kill productivity don't show up in system diagrams or monitoring dashboards. They're invisible until you watch how developers actually work.

**Cognitive Overload**: When a simple change requires navigating six services, four layers of abstraction, and three different authentication schemes, developers burn mental energy just figuring out where to start. I've watched senior engineers spend entire mornings tracing through code to understand a basic data flow.

**Death by a Thousand Cuts**: Cryptic error messages. Inconsistent naming conventions. Setup processes that require archaic knowledge of the past decisions. APIs that work differently than they're documented. Each small friction point seems minor, but together they create a development experience like assembling IKEA furniture—blindfolded, without an Allen key.

**The Workaround Culture**: When your architecture is hard to use, and team’s complaints unaddressed, teams stop complaining about the vanilla ice-cream runs—they adapt. They build shadow APIs. They duplicate functionality. They create one-off solutions that bypass your carefully designed interfaces. Team stop ordering vanilla flavour. This creates entropy, a slow decay that hollows out your system from the inside. And it could be happening right now in your codebase.

You can measure this dysfunction. Track time-to-first-commit for new team members. Count "house-keeping" stories. Monitor backlog for tech debt. Monitor how often new services deviate from your patterns. Measure deployment times. Survey developers about their biggest daily frustrations. The invisible has visible footprints.

## Treating Architecture as a Product

The solution requires a fundamental mindset shift: **treat your architecture like a product, with developers as your users.**

This means usability matters as much as technical elegance. Here's how I would operationalize that:

**Embed with Your Users**: You can't ride shotgun with every team, but you can create touchpoints at scale. Run weekly office hours. You can't be in every sprint review, but you can create touchpoints. Occasionally pair-program on small features. Better yet, record yourself building something with your own architecture—narrate the steps, show the gotchas, invite collaboration. Let people see how it's actually supposed to work. Get in to the car with your customer.

**Conduct User Research**: Run developer experience surveys and interviews where you watch—don't explain, just observe—as developers use your systems. Where do they pause? What do they seek assistance for? What makes them swear under their breath? These moments reveal your architecture's true usability. Listen to your user's feedback.

**Use Your Own Product**: Build something real with your architecture regularly. Feel the pain directly. If you don't have bandwidth for this, rotate a "developer-in-residence" from one of your user teams into the architecture group for a few months. Let them bring their daily friction back to you with full context. You won't find the vanilla bug unless you taste it yourself.

**Cultivate Internal Champions**: Find engineers in your teams who deeply understand your architecture. These champions can relay context both ways—helping their teams use systems effectively while giving you ground-truth feedback about what's actually broken. They’re the ones who know when the vanilla flavour starts causing a mess in your system.

## When Developers and Architecture Conflict

This doesn't mean giving developers everything they ask for. Business constraints, security requirements, or scalability needs - they all require difficult trade-offs that create friction. The key is making those trade-offs transparent and providing sensible defaults for the common cases.

When Netflix moved to microservices, they didn’t order teams to break up monoliths. They built tools that made microservices easier to deploy, monitor, and debug. They made it easy to adopt and let the teams decide when they are ready.

When Stripe designed their API, they didn’t obsess over the purity of their REST-iness. They prioritized developer experience—making common operations simple, errors helpful, and the learning curve gentle. Their "unorthodox" API became a standard because it worked for real people building real applications.

## Redefining Success

We need to abandon the notion that the best architecture is the one with the cleanest diagram or fewest moving parts. Those might be good architectures, but they're not necessarily useful architectures.

A truly good architecture doesn't just work—it works *for people*. It empowers developers. It makes complex problems feel manageable. It turns architectural constraints into guardrails that help teams move faster, not walls that slow them down.

Success of an architecture isn't measured by theoretical elegance. It's measured by how confidently and quickly teams can use your systems to solve the problems in front of them.

## Stop Dismissing the Vanilla Ice Cream

The next time a developer brings you a problem that seems irrational or misguided, resist the urge to explain why they're wrong. Instead, get curious. Get in the car with them. Watch them order vanilla ice-cream!

Be curious and ask why the car won't start.

Because the answer is rarely about the ice cream flavour. It's about the system you've built—and whether it actually serves the people who have to use it every day.

Your architecture's success depends entirely on their success. Start designing for it.
