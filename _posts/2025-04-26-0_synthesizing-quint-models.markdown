---
layout: post
title:  "Synthesizing Formal Models with Tamed LLMs"
date:   2025-04-26T00:00:00+00:00
tags: [formal methods, artificial intelligence, program synthesis]
published: true
mathjax: true
---

When blockchain systems are said to be trustless, what this really means is that all the trust is put into software. 
Software vulnerabilities in blockchain world cost millions, and this makes for a strong incentive to ensure blockchain software is correct.
Any serious blockchain product will make sure to audit their code thoroughly, very often by multiple independent companies<sup>1</sup>.

## Formal Methods in Audits
A vanilla way to perform an audit is by getting experienced people inspect the code and reason about what could go wrong in the code logic under the weirdest possible edge cases.
A way to make sure that no edge case is missed and to scale the auditing effort is by using _formal methods_. 

Formal methods are a potent tool for ensuring software correctness.
They rely on automated logical reasoning to either uncover vulnerabilities or assert their absence. 

## ... and the Problem with Using Them
Unfortunately (and of course), it's not all smooth sailing: for large codebases and expressive languages, formal methods scale poorly. 
One common way to address the problem of scalability is to build a formal model of the system and reason about that model instead of the full system implementation. We call the techniques embracing this approach _model-based techniques_ (MBT)<sup>2</sup>.

A formal model is a mathematical abstraction of the software system.
We can reason about properties of the model, and establish the connection of the model to the actual implementation using model-based testing techniques. 
However, because of the initial cost of writing the model and the need for specialized knowledge of the modeling language, model-based techniques are used only exceptionally.

In our [recently published paper](https://arxiv.org/abs/2501.12972) we we show how to automatically generate models using LLMs **in a safe way**. We tame LLMs’ propensity for hallucination by setting a hard frame around it through mechanical model synthesis. 
After receiving initial generated model, we iteratively repair it, until the model conforms to the user’s (partial) specification.

## Method Overview
Our method allows the auditor-user to automate parts of the MBT workflow, or take over in the middle of the process. Importantly, at every step, the method provides a valuable artifact to the auditor. 

<figure style="width:100%">
  <img
  class="centered"
  src="{{ site.url }}/assets/posts/tamedLLMs/algoSchemeWhiteBackground.png"
  alt="Figure 1: The actor model scheme">
  <figcaption>Iterative repair of generated models</figcaption>
</figure>

As a first step, we transpile the smart contract(s) into a stub of the model: the stub captures interleavings of different actions, but does not capture the actions’ semantics. At this point, the auditor can use generated stubs to run fuzzing tests against the contract. 

In the second step, we ask the user to provide a natural language description of each action and a couple of input-output examples.  Our method then interacts with an LLM: beside the user's input, we give the LLM as context the generated stub and best practices of the modelling language together with some examples<sup>3</sup>. 
Every time the model is not aligned with user-provided examples, out tool generates the prompt pointing out the inconsistency and requesting new generation. 

The main insight on which we based the design of our tool is that the careful separation of concerns between mechanical synthesis (transpilation) and statistical synthesis (LLM-based) is crucial. Relying on an LLM to generate the whole model results in hallucinations (e.g., using non-existent helper functions) or convincingly looking yet wrong models. Conversely, transpiling the full contract into a model is hopeless: even if it worked, it would result in line-by-line re-implementation of the contract, instead of a good abstraction. 

Figure 1 provides a high-level overview of our method for transforming a smart contract ([CosmWasm](https://cosmwasm.com/) in our case) into a model ([Quint](https://quint-lang.org/) in our case).


## Conclusion
We tested the proposed method on problems from CosmWasm's [Capture the Flag repository](https://github.com/oak-security/cosmwasm-ctf), and it was able to create correct models for the problems (which then enabled finding bugs in smart contracts).
Furthermore, we used it to find a problem in one of Informal System's historical audits.

For more details about the method, please read [the paper](https://arxiv.org/abs/2501.12972)<sup>4</sup>, and get in touch if you'd like to discuss it.

---
Notes:



1: There are indeed many companies addressing the need for audits: [Informal Systems](https://informal.systems/), [Certora](https://www.certora.com/), [Immunefi](https://immunefi.com/), [Veridise](https://veridise.com/), to name but a few.

2: Have a look at https://mbt.informal.systems/

3: The examples are needed since most of the modelling languages are not part of the mainstream so LLMs may not have previous knowledge about them.

4: The paper was published together with dear colleagues Gabriela Moreira, Jan Corazza, and Daniel Neider at [ICST 2025](https://conf.researchr.org/home/icst-2025).



