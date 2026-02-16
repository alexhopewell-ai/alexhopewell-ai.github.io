# I Built an Autonomous Software Factory in a Week. Agencies Should Be Worried.

## How one indie developer replaced an entire development team with pipeline orchestration, AI agents, and a kanban board

---

I shipped my first product the old-fashioned way. Well, the new old-fashioned way -- me and Claude Code, pair-programming through a productivity app from blank project to release. It took weeks. It was satisfying. And about halfway through, I realized I was building something more valuable than the product itself: a repeatable pattern.

That first project used Clean Architecture with domain-driven design. It had a specific folder structure, a dependency injection approach, a testing strategy, a design system. Every decision I made -- how to structure ViewModels, where to put repository protocols, how to handle notifications -- was a decision I would make the same way for the next product and the one after that. I was encoding my taste as a software developer into a living reference implementation.

The question became: what if I extracted those patterns into standards documents, pointed an AI agent at them, and told it to build the next product?

That question turned into what I call the Factory. And the answer turned out to be yes.

## The Stack

The factory is not a single tool. It is an orchestration layer built from several pieces that each do one thing well.

**Beads** is the issue tracker. Created by Steve Yegge as part of his Gastown multi-agent workspace system, Beads is git-backed -- every issue is a line in a JSONL file committed to your repository. No external database, no SaaS dependency, no authentication layer. Just structured data in your git history. I use the CLI (`bd create`, `bd close`, `bd update`) to track every piece of work. Every feature, every bug, every research task gets a bead. No exceptions.

**beads-fleet** is the dashboard I built on top of Beads. It is a Next.js application running locally that gives me visual access to everything: issue tables, kanban boards, dependency graphs, time travel diffs showing what changed between commits. But the feature that matters for the factory is the Fleet page -- a pipeline kanban board where each product idea is a card that moves through stages from left to right. Ideas, In Research, Research Complete, In Development, Prepare for Release, Released, Library Management, Completed. And off to the side, the graveyard: Bad Ideas.

**Claude Code agents** do the actual work. beads-fleet can launch headless Claude Code sessions as background processes -- no terminal window, no human watching. The agent gets a prompt, a repo path, a set of allowed tools, and a turn limit. It runs autonomously until it finishes or hits the limit. beads-fleet polls the process, detects when it exits, and automatically updates the pipeline labels to advance the card to the next stage.

**Shared libraries** I had extracted from previous projects contain the domain primitives that all my products share -- schedulable items, schedule types, notification infrastructure. Every product the factory produces depends on these reusable component libraries, which means shared logic is written once and maintained in one place.

**The reference implementation** is the gold standard. My first project, the one I built by hand. Every new product the factory builds follows its architecture, its DI patterns, its testing approach, its design sensibility. The factory standards documents were extracted directly from that codebase.

## How It Works in Practice

I have an idea for a product. Say, a habit tracker with smart reminders. I open a terminal, navigate to the factory repo, and create a factory epic:

```
bd create --type=epic "Smart habit tracker product" \
  -d "Track daily habits with configurable schedules. Notify when it's time
  to complete morning routines, weekly reviews, or monthly goals."
```

A card appears in the Ideas column on the Fleet page. When I am ready to investigate it, I click "Start Research." beads-fleet launches a Claude Code agent running the Opus model with web search enabled and up to 200 turns of autonomous work. The agent creates the product's repository, conducts market research -- real competitor analysis, keyword research, pricing strategy, category analysis -- and writes a full research report. When it finishes, the card automatically moves to Research Complete.

I read the report. If the idea has legs, I click "Send for Development." Another agent launches, this time in the product's own repository, with 500 turns and access to all my factory standards. It reads the research report as its specification. It creates granular beads for every feature. It scaffolds the project. It implements each feature following my architecture patterns, builds after every change, runs tests, closes each bead as it finishes. A progress bar on the fleet card shows me how many development beads are closed versus total.

When the agent finishes and all beads are closed and the build passes, the card moves to Prepare for Release. I run the product in a test environment. If I find issues, I write feedback in the epic notes and click "Send back to Development" -- the agent relaunches with my feedback as additional context. If the product is ready, I click "Approve Release" and a smaller agent generates store metadata, release notes, and a screenshot plan.

After I ship and the product goes live, one final agent analyzes the codebase for patterns that should be extracted back into the shared libraries, feeding improvements back into the reusable components for the next product. The card moves to Completed.

The entire pipeline -- from "I have an idea" to "the product is ready for shipping" -- can complete in days.

## The Part Where Agencies Get Nervous

I want to be careful here because this is where it is easy to sound like every breathless LinkedIn post about AI disrupting everything. So let me be specific about what I actually built and what it actually means.

I am one person. I have no employees. I have no contractors. I built a system that takes a product idea, conducts market research, plans features, implements a full software product following professional architecture patterns, runs tests, and prepares release materials. The human touchpoints are: defining the idea, reviewing the research, testing the built product, and clicking ship.

This is work that a software development agency charges twenty to fifty thousand dollars for. For a simple product. And they take three to six months.

I am not claiming the factory produces the same quality as a seasoned team of ten working for months on a complex enterprise application. It does not. But the honest truth is that most projects agencies build are not complex enterprise applications. They are relatively straightforward CRUD apps, scheduling tools, reminder systems, habit trackers, utility products, marketing websites, internal dashboards, custom business software. The kind of greenfield project where the architecture is well-understood, the design patterns are established, and the real value is in the product decisions -- what to build, for whom, and why.

The factory handles the "how to build it" part. That used to be the expensive part. It is not anymore.

Agencies survive today on two things: the complexity of translating business requirements into working software, and the client's inability to do it themselves. The first is being automated. The second is becoming less true every month. A product-minded founder with Claude Code and the right orchestration layer can now produce what used to require a team.

This is not theoretical. I built this in a week. The factory repo, the pipeline spec, the fleet board, the agent configurations, the standards documents -- all of it. And the first product through the pipeline took days, not months.

## What I Actually Learned

The surprising lesson was not that AI can write code. Everyone knows that by now. The surprising lesson was that the hard part is no longer implementation -- it is orchestration. Knowing which agent to run, in which repo, with which prompt, reading which standards, with what turn limit and tool access. Getting the pipeline stages right so that human decision points happen at the right moments. Making sure the research agent's output becomes the development agent's input cleanly. Ensuring each product gets its own repository with its own git history and its own issue tracking, while the factory repo maintains the orchestration layer.

This is systems thinking, not programming. And it is the kind of work that rewards experience and taste over raw coding ability. I could build the factory because I had built my first project by hand. I knew what good architecture looked like because I had spent years building software. The AI did not replace my expertise. It amplified it to a degree I did not expect.

My factory is not open source but you can build your own using beads-fleet API as the front end, it is MIT licensed. Beads is freely available. Claude Code is accessible to anyone with an Anthropic account. The patterns I described here are not proprietary. If you have domain expertise and product taste, you can build your own factory for your own domain -- whether that is custom business software, web applications or mobile apps.  The only thing stopping most people is not access to tools -- it is the willingness to think about software development as a pipeline to be orchestrated rather than code to be written.

That shift in thinking is the real disruption. Not the AI itself, but what happens when someone treats it as infrastructure rather than a novelty.

---

The dashboard and pipeline orchestration described in this article is built on [beads-fleet](https://github.com/alexhopewell-ai/beads-fleet) (MIT licensed). Special thanks to [Steve Yegge](https://github.com/steveyegge) for creating [Beads](https://github.com/steveyegge/beads), the git-backed issue tracker that makes all of this possible.

---

*Alex Hopewell (a pseudonym) is an indie software developer building autonomous delivery systems. She writes about the practical reality of AI-assisted development.*
