---
title: 'I need a reductive AI in the generative era'
description: ''
pubDate: 'Jun 08 2026'
tags: ["reductive", "ai"]
---

My perception about the large language models and its generative power has changed. I believe that the super power of Generative AI is Reduction, which is probably a paradox of a kind.

The general usage pattern that I see is all generation. Docs are written by LLM, and the reader uses LLM to summarize it. Team members are using LLM to fine tune their messages to other team members. Email responses come pretty quickly with a lot of text, obviously written by LLMs, as if the problem at hand was the response time from whom the email was addressed. For the sake of solving a problem or answering a question, employees are copy pasting auto generated responses. We are probably at a stage where the distance from reality is far enough that the writer and reader are not understanding what is going on between them.

A recent experience worth calling out here is when we were asked to generate an API contract, but the details - data model, validations, patterns, methods etc were not clear and alignment was missing altogether. So naturally, it was marked blocked. It took no time for the architect to ask an agent to build it and voila - an entire Swagger was available in the chat response. Without checking, it was shared to the team asking to continue the build. Only when we opened it we realized that 99% of the APIs just return 200 OK responses, there is no data returned. There were no models or definitions. The task is technically complete, which was to generate an API contract, with no value or alignment.

It is hard to see how LLM is the root cause of this. But previously, when this tool was not present, the unknowns could not be translated to hallucinations and could not be forced into completion. When LLM is present, especially the generated ones, it is easy to show that something was generated rather than nothing, when in fact nothing was better than something.

Reduction, the act of making something smaller in quantity, is what I am now mostly using LLMs for. It’s as if my entire usage pattern has changed in the last few months, and I see real benefit from it.

**1\. Chaos to Knowledge Graph**

My brain, and most of ours presumably, is naturally chaotic. There is never a straight line of thought in my mind, except when I am highly focused. And it fits into my small, localized, world. But the moment I want to transfer a part of my brain to someone else, I find it very hard to understand where to start from. For example - when working with slides for solutioning, proposal et al, I used to ask others to show me a template and I will fill it in. They would reject this idea, saying that I first have to write and the template is a later problem. I don’t think like that - I want to know the sections and order beforehand, otherwise they themselves will ask me, how do we order this. 

Using the knowledge graph, thanks to Karpathy, I have found myself in a very comfortable position nowadays. Several of my “chain of thoughts” are now externalized in a way that is rich, searchable and connectable - all of which would have taken more than my focus-time allowances from family.

I also have a tendency to apply Systems Thinking almost at every problem I see ( sorry, I cannot resist it ). I now use the “Interrogatory LLM” approach to connect the dots. This was interesting, because sometimes I need a question for me to tap into some of the areas of memory and an LLM is super efficient in doing that.

I also use voice memos in my phone, copy the transcript and then feed it into the LLM to create 3-bullet single line summaries.

**2\. Assessing quality of implementation**

Another area where reduction is super powerful is to gather the meaning of various sections, visualizing them and finding or spotting issues with it. The agents particularly can also review the test code, make it more behavioural so that the source to test coupling is removed one by one, and in the event I have found that the actual lines of code that we need to maintain goes down considerably. This is also partly due to the fact that the applications that I tried had test design issues.

One another way I use to apply reduction is to load two classes or modules of source code to compare the level of entropy in them. Areas such as domain leakage, duplicated code, leaky abstractions can all be compared and the report is quite useful for brownfield projects. We can also assess the quality of tests, see how they are managing their dependencies in the test, also if there are any composite functionalities between them and whether tests (integration scope) are covering it.

**3\. Decomposing Information**

Occasionally, I get overwhelmed with the amount of information that I need to break down. Applying systems thinking is useful, but will quickly get a reality check that it is too large to comprehend in a single go. Most information is vector in nature, with a certain velocity and a direction. I mean, the same information delivered by a peer or an executive has a different span or weight associated with it, all dependent on the context and problem statement. While the decision is largely mine, I now eagerly get the inter-connectedness established through the agents. Using BMad is one option, there are also downloadable skills available in Github and in Tessl. I have got a personal skill of my own too, that I have meticulously created based on the way I think and understand. All these with templates and workflows that I generated iteratively, will leave different levels of information categorized by tags, directories and read more links depending on how deep I want to go back and recollect.

LLMs generative capabilities come from the past, but blindly repeating the past to progress does not bring a new future. Creativity and ingenuity are both forward looking, and we should be trusting ourselves with it. This technology is here to stay. As for me, less of generation and more of reduction.
