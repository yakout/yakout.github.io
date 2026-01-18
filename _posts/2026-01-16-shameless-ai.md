---
title: "Shameless AI"
excerpt: "<span style='background-color: #000; color: #fff; padding: 0.25em 0.5em;'>From BERT to Agentic loops: why I stopped worrying about the hype and started building at 10x speed.</span>"
last_modified_at: 2026-01-16T17:30:00-05:00
categories:
  - AI
tags:
  - Engineering
  - LLM
  - Productivity
header:
  overlay_image: /assets/images/shameless-ai-overlay.jpg
---

The last time I really went deep into the AI world was 2019. Back then, my AI/ML knowledge was built on the foundations of manually tuning hyperparameters, selecting the right activation functions (ReLU vs. Leaky ReLU), and worrying about whether my CNN would overfit. It was the era of YOLO v3, Siamese networks for one-shot learning, and the beginning of the Transformer revolution with BERT (even though the "Attention is All You Need" paper and GPT-1 preceded it around 2017–2018)—all of which I spent months studying (and summarized in my [CS 435 notes](/csed/cs-435/)).

It felt like "science" in the most traditional sense. You’d carefully design an architecture, manage gradient clipping to prevent those dreaded exploding loss functions, and wait hours for a training loop to finish, only to find out you had a "brain fart" in your reward function.

At that time, AI research was the peak academic hype; almost every graduation project involved some form of AI. But I didn’t care much for the hype itself. Instead, I was more interested in the "real" side of software engineering: understanding distributed systems at scale and building things that could handle the pressure of the real world.

Fast forward to today, and the landscape is unrecognizable. I’ve moved from training models to orchestrating agents (**"The New Hype!"**).

### The Shame of the Hype

Hype is a double-edged sword. On one hand, it drives innovation and brings resources to new frontiers; on the other, it creates a noise floor that makes it hard to distinguish between progress and vanity. Human nature is naturally suspicious of hype—we are hardwired to be afraid of being "sold" something that isn't real. It’s a defense mechanism that helps us avoid wasted effort.

The trick is knowing when to ignore the noise and when that noise is actually a signal that your entire career is being refactored. In 2019, I could afford to pass on diving deeper into AI research or the industry because my core interest—distributed systems—was still the primary way the world built software. But today, the shift toward Agentic AI isn't just another library or a "cool new thing." It is a fundamental change in the programmable layer of the world.

It’s easy to be cynical. Every day there's a new "game-changing" repo, another `.ai` domain, and a thousand LinkedIn posts claiming that if you don't use *this specific prompt*, you’re already obsolete. The **hype fatigue** is real, and the "shame" associated with jumping on the bandwagon is understandable.

But here’s the cold truth: <span style='background-color: #000; color: #fff; padding: 0.25em 0.5em;'>you don’t have an option. You need to keep up.</span>

My views aren't based on the hype; they’re based on heavy, daily AI usage throughout 2025. After seeing hundreds of real-world implementations and watching how the engineering barrier to entry has evaporated, I’ve realized that the "shame" of using AI is actually just the sound of a profession being refactored in real-time. It might just be the "fear" that leads people to become defensive sometimes—and again, this is totally understandable. You can't control people's feelings; it's built into our nature.

### AI Amplification: The 10x Reality

The most immediate benefit is that coding is no longer the bottleneck. Information retrieval is faster, more organized, and increasingly accurate. Most of the time, the syntax is solved. This lifts the entry-to-market barrier and "activates neurons" you didn't know you had. You're no longer just a "coder"; you're a system architect, a reviewer, and a product manager rolled into one.

Even the "boring" stuff is no longer a time-consuming task. It’s just execution.

But more importantly, **Agentic AI will force better engineering practices.** If you want an agent to work effectively, you need better project documentation, clearer specs, and rigorous testing. You can’t "vibe code" your way through a complex system with an agent; you have to be precise. You have to understand the rationale behind the new abstractions of our era—concepts like the **ReAct pattern** (Reason + Act), **Tool-calling loops**, **Stateful Tool-calling**, and **DAG-based workflows** (Directed Acyclic Graphs, as seen in frameworks like *LangGraph* or *CrewAI*). If the LLM is the "brain," then patterns like **ReAct** provide the internal reasoning (the inner dialogue), while **DAGs** act as the **prefrontal cortex**—the part responsible for high-level planning, orchestration, and structural decision-making.

### But... The Cognitive Cost

It isn't all free speed. The downsides are subtle but heavy:

* **Review Load:** You spend less time writing and significantly more time reviewing. If you're not careful, the cognitive load of validating what an agent produced can be higher than just writing it yourself. Senior team members are now more prone to burnout because they are spending more time reviewing AI-generated code than actually building.
* **The Side Project Trap:** AI makes it so easy to build that you might find yourself overwhelmed by a dozen half-finished projects. AI has changed the scale of production, which means you need a new level of discipline to actually *finish* one thing before the agent's context-switch cost eats you alive.

### The New Hierarchy of Languages

We are witnessing a fundamental shift in the stack. I grew up as an "artisanal software engineer," manually writing complex C code and feeling that rush when it compiled without errors. I’m glad I had that experience—much like learning Assembly to understand the hardware. It gave me a mental model of how things work under the hood that the next generation might never have to develop at the same level of detail.

But we are moving one layer up. C, C++, Java, and other coding languages are becoming the new "low-level" languages. Human language is becoming the standard "High-Level Language." And who knows? Maybe in the future, we won’t even need human language in the loop at all, and we might be able to ship things at the speed of inference + thoughts.

### How to Start (Sleeves Up)

Don't be ashamed of using AI heavily. Don't listen to the people saying "AI is dumb" just to feel superior, or people who will make you feel incompetent for just using AI. "Artisanal" software engineering is almost dead.

1. **Start with small, isolated tasks.** Don't go "YOLO mode" and let an agent delete bunch of stuff without review. Understand *what* they are doing, *why*, and *how*. Approve every step manually until you understand their failure modes.
2. **Focus:** Even if you have the power to build five things at once, don't. Focus on one project at a time. The context switch for a human-agent team is incredibly costly.
3. **Security Awareness:** As your agentic workflow matures, consider self-hosting your models. This gives you a development environment that is secure, private, and independent of external APIs. It is crucial to stay aware of the risks of uncontrolled autonomous systems, especially those that can take input from external sources. This is a massive topic—my next post will dive deep into this: "Security from .exe to .md." Stay tuned.
4. **A tool is just a tool:** Ignore the posts claiming "this tool/model is the best" or "that model/tool is dead." Seriously, any frontier model and popular agentic tool nowadays can achieve most of your needs. Focus on the target you want to achieve and the engineering aspect of it. Focus on "Spec'ing," Context Engineering, and Task Management for your agents. This is what really matters. Your time is more valuable than arguing about which model is the best. It's subjective and a matter of personal experience.
5. **Reading skills:** Believe it or not, this is going to be a much-needed skill now. Reading at high speed and having strong reading comprehension is crucial. You are going to spend most of your time reading (or maybe listening) than anything else.

### Some Orchestration Hacks

Treat AI like that brilliant colleague who knows every shortcut and obscure API. Don't just accept the output blindly; reverse-engineer the "hacks" it uses. My role as a senior engineer has fundamentally shifted from writing syntax to managing agent psychology, project state, and learning from these "hacks." And remember that AI is a fantastic partner, but a terrible boss:


* **Avoid Agent Laziness:** I’ve started explicitly instructing my agents to "unblock themselves." If a dependency is missing, install it. If a test fails, fix it and move on. Don't stop and ask me for permission for every trivial task.
* **Context Engineering:** Stop stuffing your entire GitHub repo into the context window. That’s how you end up in "MCP hell." Instead, turn your `DESIGN.md` into [RFC 2119-like](https://www.ietf.org/rfc/rfc2119.txt) requirements in a `REQUIREMENTS.md` file. Review them rigorously. As Addy Osmani points out, [a good spec](https://addyosmani.com/blog/good-spec/) is the difference between a working product and a hallucinated mess.
* **The Context War:** You have to balance **Stale Data vs. Token Count**. Managing project `.md` files is the new way of managing memory. If your documentation is stale, your agent is hallucinating. If it's too long, you're burning tokens for nothing.
* **Reset, Don't Loop:** When an agent gets stuck in a loop or starts "brain farting," stop. Don't keep trying to fix the same context. **Reset.** Start fresh. Agents need a "break" to clear their state, exactly like humans do. Simplicity is key—avoid over-customization that leads to brittle workflows.

### Final thoughts

While AI is a "shameless" collaborator that can get you half way there, the remaining work—the architectural soul and the edge-case correctness—is where human expertise is more critical than ever. AI might be a "super search engine" that removes friction, but it lacks the "wisdom" to understand intent or the catastrophic cost of a hallucinated CLI command in a production environment.

**Note:** The "good hype" I am referring to here is the integration of Agentic AI into our daily workflows. In my personal opinion, there are other distracting side hypes that have emerged as a result of this that you should make sure to filter out. Whatever your job field is, use AI in your work—don't let AI use you.

---