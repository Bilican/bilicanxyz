+++
date = '2026-07-02T10:00:00+03:00'
draft = false
title = 'Loop Engineering: Worth It Yet?'
description = 'Notes on loop engineering: what can and cannot be offloaded to an agent, and why you should never let an agent grade itself.'
tags = ['loop-engineering', 'claude-code', 'ai-agents']
hero = '/images/loop-hero.jpg'
+++

{{< callout type="author" >}}
Even though I'm an agentic engineer this post was written solely and entirely by me for the purpose of being authentic and insightful.
{{< /callout >}}

I was busy with building in the previous weeks so haven't had the chance to dive deep into loops but for the last two days I have been diving deeper into loop engineering.

Normally when building with claude code I had familiarity with ralph loops, especially the form that matt pocock uses. My standard workflow went like the following:

{{< fig-pipeline >}}

Each step was battle tested and existed for a specific reason.

**Have a grilling session with the skill `/grill-with-docs`**

Need to align the concept in my mind with claude and build a product that fits my criterion and is aligned with my overall goal. Also figure out some key design decisions that need to be made I might have otherwise missed.

**Consult key decisions with the team during this process**

Sometimes key decisions need a second opinion which is incredibly important if you are building a production level product.

**`/to-issues` to split the task into vertical slices which can be implemented.**

If not split into chunks the task is too large and models enter the dumb zone when implementing it. (More bugs are introduced). Also if the slices are not vertical there can be no verification until the very end which is also bad.

**Delegate subagents to implement each slice with a fresh context.**

Fresh context for each issue means every implementation is done in the smart zone.

**There is an optional step here: sometimes I verified the implementation and found bugs in it using codex.**

Codex is terrible at coding but really good at catching bugs as of my experience and the domain I work in.

**Let the orchestrator agent test the implementation end to end.**

If there are small fixes that need to be made before I verify it myself then that is great.

So this seemed kind of optimal. Loops which tried to automatize more just seemed too scary and my prior experience would just tell me they would converge into slop. But I still decided to look into them.

## What is Loop Engineering

Loop engineering is the discipline of engineering the feedback loop that wraps the agent.

{{< callout type="principle" label="The goal" >}}
Be able to merge the code to the repo without human supervision needed.
{{< /callout >}}

{{< fig-loop >}}

So the delegation part and the verification part that was handled by me should be handled by an agent.

### Can I actually offload the task delegation part to an agent?

No, because I'm the one actually steering the product and deciding what the product is and what it is not. I should continue to be the judge of what gets added and what doesn't get added to the product.

If the tasks are bug fixes then theoretically it can be offloaded but we have to have strong verifiers for a capability to automatically detect a bug. This implies a feature has been tested by a human and a verifier is constructed via human guidance.

### Can I actually offload the task verification part to an agent?

Yes, well partly. I mostly work on building a desktop app with certain features which are hard to verify. There are a couple of reasons for this:

1. Some aspects of the app contain animations and visual queues.
2. There is a UI and the agent has to visually navigate through the app to drive different flows.
3. To determine if one of our agents has completed a task successfully the coding agent has to judge the work of our browser agent, and the browser agent can bias the coding agent into thinking it was successful.
4. Some features are flaky and fail not because the tool is bad but because they depend on external sources.

So are there solutions to these problems? Well partly, and there are some cool insights I gathered along the way that could even be helpful to the main workflow I'm using.

## Never let an agent grade itself

This is a key concept that people have arrived at independently.

{{< fig-evaluators >}}

There is a choice to make in terms of the evaluator you are selecting.

The first approach: you could use a weak verifier and fresh context to evaluate a certain feature, given that it can execute a script which tests the certain flow of the feature. This verified script should be the ground truth and the agent can judge its outcome quite easily. The intellectual load and the design of the verifier is in control of the loop engineer. To make sure to avoid cheating of the coding agent you should of course strictly prohibit it from changing the verifier script.

The second approach would be to have a strong evaluator with fresh context but you let the model act. You never let the model read the code. This is a general principle because if you let the model read the code, then you make it possible for your original coding agent, which coded the feature, to bias the evaluator model incredibly just by using the comments part or by naming conventions or even by how they implemented the feature in the repository to actually suffice another aspect but not the original feature you intended.

This ties back into one of the key points I mentioned, which was a problem but which could be solved in a semi-successful manner. You could integrate ways to let the agent execute flows through your user interface by adding tags, proper naming tags or proper shortcuts to your app depending on which framework you use. You should most likely not let your coding agent, especially Claude Code, use its computer use feature where it drives apps through screenshots in the pixel space. This could make both versions of verifiers better.

## Sources

The sources I read while writing this post:

- **The Kitchen Loop: User-Spec-Driven Development for a Self-Evolving Codebase**  
  [arxiv.org/abs/2603.25697](https://arxiv.org/abs/2603.25697)
- **Loop Engineering: Four Loops That Actually Works**, by zostaff  
  [x.com/zostaff/status/2070852153594290195](https://x.com/zostaff/status/2070852153594290195)
- **Loop Engineering: The Anthropic Playbook for Designing Systems That Prompt Your Agents**  
  [drive.google.com](https://drive.google.com/file/d/1qzKI4DKnyHRpXK1J3ATPqwaqLc0iNu-M/view)
- [x.com/0xCodez/status/2064374643729773029](https://x.com/0xCodez/status/2064374643729773029)
- [x.com/sairahul1/status/2064277888216555684](https://x.com/sairahul1/status/2064277888216555684)
