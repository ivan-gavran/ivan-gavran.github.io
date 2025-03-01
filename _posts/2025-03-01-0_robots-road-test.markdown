---
layout: post
title:  "AI drivers cannot just pass the road test; AI coders cannot just write a couple of tests"
date:   2025-02-27T00:00:00+00:00
tags: []
published: true
mathjax: true
---

*When will self-driving cars be ready enough for roads (without any restrictions)?*
There were many proclamations of accomplishing full autonomy, but they always turned out to be false.
The main challenge lies in handling very rare scenarios, those never seen in the training data.

And yet, how do we decide if a human can be given a permission to drive autonomously?
Well, we mostly give candidates some pen-and-paper exam, followed by an hour long drive around their area. 
And that's it, they're deemed fully autonomous.

The paradox is obvious: contemporary AI systems would pass those exams easily.
Why are we still full of doubt about them?
It is because our criteria for trusting a human candidate is not based only on the fact that they passed the exam.
We trust the candidate because they passed the exam **and** because they are human.

Humans are predictable enough that we can extrapolate their general driving skill from their behavior in that short exam. (And as soon as there are signs of unpredictability: e.g., a progressing short-sightedness, influence of alcohol, or similar, we limit the human driver's autonomy.)

## The case of AI programmers
Loosely analogous is the question of when to trust a program written by an AI agent.

For a human programmer, we are often happy enough if there are some tests witnessing that the program behaves correctly under basic scenarios and under predictable edge cases.
Should we expect the same from (statistically) generated programs? Definitely not.

Like in the case of driving, the tests are not the only reason we trust the program. 
They just support our idea of how humans typically think, and what mistakes they typically make.
This lets us anticipate errors and judge when a set of tests is likely sufficient.

An AI agent, on the other hand, could make much weirder and less predictable mistakes.

This unpredictability is why AI agents need to give us much stronger assurance of their programs' correctness.
For instance, by generating loads of tests. 
Better yet, by creating formal models (e.g., in TLA+, Alloy, or Quint) for the implementation, and connecting the two through [model-generated tests](https://mbt.informal.systems/). 

In short, AI coders---like AI drivers---must do far more than their human counterparts, because of our (I believe justified) scepticism of their failure modes.

---
Relevant papers:
 - [Clover: Closed-Loop Verifiable Code Generation](https://arxiv.org/pdf/2310.17807)
 - [Accessible Smart Contracts Verification: Synthesizing Formal Models with Tamed LLMs](https://arxiv.org/pdf/2501.12972)