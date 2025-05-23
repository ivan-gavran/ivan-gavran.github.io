---
layout: post
title:  "Robotic Systems, Powered by Formal Methods"
date:   2023-08-25T00:00:00+00:00
tags: [formal methods, robotics, artificial intelligence]
published: true
mathjax: true
---


This is a summary of the introduction to my PhD thesis defense talk. The talk was based on my work at Max Planck Institute for Software Systems, where I was exploring how one can combine formal methods (FM) and autonomous systems. 

<figure style="width:90%">
  <img
  class="centered"
  src="{{ site.url }}/assets/posts/2023-08-25_robotic_systems_FM/robotInHospital.webp"
  alt="A hospital robot">
  <figcaption>Robots helping in a hospital</figcaption>
</figure>



When deciding whether to use formal methods for the development of a software system, there are three strong arguments in favor of doing so:
- FM force us (and even help us) to provide a precise specification of what the system is supposed to do
based on the specification, 
- FM can provide guarantees for software correctness (or find bugs, if they exist),
- for some parts of the system, we can even avoid writing code altogether: FM can synthesize provably correct code from the specification

This sounds like a great value proposition. And yet, when the decision time comes, most teams decide against using formal methods.

## Why is that so?

There are of course some strawman arguments.
 "_Programmers are terrified by math_" - says Leslie Lamport. "_FM as a discipline constantly overpromises but rarely delivers_", say others. There is also a question of semantics: as soon as a method starts being used in mainstream software development, it is no longer a part of formal methods - it is simply considered regular software engineering.

Realistically, though, it is always about what benefit formal methods are bringing to the table. The three selling points of FM are easily dismissed by:
- _having precise specification is useful, but my code is already precise enough and clear enough_
- _it is great to have correctness guarantees, but 90% of all bugs I can find by doing basic testing. As for the remaining 10%, it is not the end of the world if they remain in the production code_
- _synthesis only works for some simpler parts of the code, where the programming effort is not all that big_

And given that using FM does not come for free (there is a somewhat steep learning curve, immature tool support, and a necessity to keep the specification in sync with code changes), these arguments can be valid sometimes. For instance, when programming a mobile app: it can always be safely restarted after crashing.

<figure style="width:90%">
  <img
  class="centered"
  src="{{ site.url }}/assets/posts/2023-08-25_robotic_systems_FM/robotHumanDuel.webp">
  <figcaption style="font-size: 12px">Picture by Blutgruppe/Corbis</figcaption>
</figure>


But what if we are developing a (multi-)robot system? It will likely contain a couple of learned components, so inspecting a code and having a clear idea of what it does is illusionary. Furthermore, robots are big and strong machines operating near humans: we cannot be almost correct, guarantees are necessary. And finally, developing a robotic system requires knowledge from many diverse disciplines. Thus, synthesizing even parts of the overall system and letting programmers focus on the application logic is valuable.

## Synthesis for Multi-Robot Systems

Motivated by this reasoning, I decided to explore interactions between formal methods and robotic systems. Concretely, with help of many great collaborators, I first developed a programming model for multi-robot systems in which users give commands describing what needs to be done (in the language of linear temporal logic, LTL), and the system synthesizes all the robots' actions to accomplish the task. We implemented one system supporting such a model, both in simulation and on a group of Turtlebot robots.

<iframe width="560" height="315" src="https://www.youtube.com/embed/lJQv_0VwFxc?si=DLxIsOUNuMHRIiAN" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

The implemented system receives commands in linear temporal logic (LTL) and takes care of lower-level details (planning, coordination, task assignment, failure recovery,…) 

## Adaptable Formal Language for Instructing Robots

Following that, I set off to answer the question of how to help non-expert users write correct specifications. This line of work culminated in LTLTalk (read: little talk), a system that uses a natural language description of a task and a single demonstration to infer the formal specification of the task. Through interactions with users, LTLTalk augments its underlying formal language: its language will always remain formal (retaining all its benefits, such as interpretability of the system's decisions), but it will converge towards the fragment of the natural language spoken by its users.


<iframe width="560" height="315" src="https://www.youtube.com/embed/VlELl2sUGjQ?si=ryn9eQtrTI96TASq&amp;start=158" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

## Reinforcement Learning with Inferred Reward Machines

Finally, the last line of work was about helping reinforcement learning algorithms learn faster and become more interpretable by inferring automaton-like structures called reward machines (prior to our work, these machines were always assumed to be given).
For more details about each of these projects, please have a look at the <a href="https://kluedo.ub.rptu.de/frontdoor/index/index/docId/6863">thesis</a> :)


# Conclusion

Coming back to the initial question of when to insist on using formal methods, the conclusion is this: the more intricate the system is and the greater the price of failure, there is bigger need for using formal methods. The robotics domain is one such example, which furthermore offers some unique opportunities (objects and actions are concrete, thus interacting with users through examples becomes easier).

Of course, having high incentives is only a part of the story. The other part is building scalable, polished, user-friendly FM tools that every programmer will love to use, as well as providing learning material to ease the adoption of the technology.

Given how much of the world runs on software, increasing the level of trust in our software is a moral imperative. I thus hope that the work described in this post contributes towards making software (and autonomous systems in particular) more reliable.

---

<p style="font-size: 12px"> This post was first published on 6.6.2022, <a href="https://medium.com/@ivangavran/robotic-systems-powered-by-formal-methods-f1e5b91093f2">here</a>.</p>

