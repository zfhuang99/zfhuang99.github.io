---
layout: post
title:  The Coming AI Revolution in Distributed Systems
categories: [GitHub Copilot, Formal verification, TLA+]
---

Formal verification is widely recognized for uncovering subtle, elusive bugs early in distributed system design [[1]]. AI significantly accelerates this verification process [[2]], but recent advancements suggest an even more transformative potential: AI can now autonomously generate accurate formal specifications directly from large-scale production codebases. This breakthrough points toward a future where AI-driven correctness verification becomes standard practice, potentially surpassing human capabilities [^disclaimer].

## Recap: AI-Driven Bug Discovery

Our recent experience with GitHub Copilot Agent demonstrates AI’s ability to autonomously produce precise TLA+ specifications from Azure Storage's production source code. Remarkably, this AI-generated specification surfaced a subtle race condition previously missed by traditional code inspections and rigorous testing, highlighting AI's transformative potential in ensuring correctness in distributed systems.

### Crafting a Plan

We began by instructing AI to formulate a detailed, step-by-step plan for analyzing a specific feature and generating a corresponding TLA+ specification. AI outlined an 8-step strategy, including:

* Cataloging relevant code paths, data structures, and Paxos/server commands.
* Extracting behaviors and invariants from source code.
* Drafting a minimal yet complete TLA+ model.
* Incorporating failure modes and safety/liveness invariants.

### Executing the Plan

**1. Autonomous Source Code Analysis and Initial Specification**

  - After reviewing the AI-generated plan, we instructed AI to execute each step, marking completion as it progressed. Notably, we only provided the feature name and the relevant component directory to narrow the search scope. AI autonomously identified pertinent files and extracted essential information without further guidance.
  
  - Subsequently, AI produced comprehensive architecture and behavior documentation, along with an initial TLA+ specification containing over 10 invariants. Impressively, the primary safety invariant matched precisely what we anticipated.

**2. Iterative Refinement of the Specification**

  - Upon reviewing the initial specification, we noticed the absence of an etag-based optimistic concurrency control mechanism. We prompted AI to investigate how etags were utilized to prevent race conditions. In response, AI revisited the source code, updated the documentation accordingly, and refined the TLA+ specification to incorporate this critical mechanism.

  - We observed that AI refined the specification iteratively, updating one atomic action at a time. This incremental approach likely helped AI maintain precision and accuracy throughout the refinement process.

**3. Validation through Model Checking**

  - With the refined specification ready, we proceeded to validate it using the TLA+ model checker. Initially, the tool reported a language error, which AI swiftly corrected. Subsequent model checking uncovered a violation of the primary safety invariant. After providing the violation trace to AI, it quickly identified the problematic sequence — a concurrent deletion and reference addition scenario.

**4. Enhancing the Specification via Git History Analysis**

  - To ensure comprehensive coverage, we asked AI to analyze the git commit history for the feature using git MCP. This analysis revealed an additional critical safety mechanism involving pessimistic locking, previously overlooked. AI promptly updated both the documentation and the TLA+ specification to reflect this important discovery.

### Identifying the Race Condition

Within just a few hours of iterative refinement, AI surfaced a subtle yet critical race condition: an old Paxos primary could perform a deletion concurrently with a new primary adding a reference. This issue had previously eluded thorough design reviews, code inspections, and extensive testing. Given the scale and complexity of Azure Storage, however, it is highly likely this race condition would have eventually manifested in a production environment.

## A Bold Prediction

Considering the sophistication of the AI-generated TLA+ specification — and drawing from over a decade of experience manually crafting such specifications — I must acknowledge that the AI-produced version rivals human-crafted work. This observation highlights a significant milestone: the essential building blocks for fully automating correctness verification in distributed systems are now within reach.

I have a bold prediction for what’s about to unfold:

* AI will autonomously dissect large-scale distributed systems, pinpointing critical components and interactions.
* AI will independently analyze interactions, identifying key invariants.
* AI-generated TLA+ specifications will be automatically validated, with AI analyzing invariant violations.
  * Violations will prompt AI to autonomously craft and execute targeted tests.
  * Genuine bugs will be automatically corrected by AI.
  * Discrepancies between models and implementations will drive continuous AI-driven specification refinement.
* Iterative AI-driven loops will generate extensive datasets, ideal for reinforcement learning, continuously enhancing AI capabilities.
* As with any domain featuring clear, verifiable outcomes, correctness in distributed systems will inevitably be mastered by AI, significantly reducing the frequency and impact of correctness issues.

We’re not yet at the finish line, but even if AI stopped advancing today, the roadmap toward an AI-driven correctness revolution is already clearly visible.

[1]: https://lamport.azurewebsites.net/tla/industrial-use.html

[2]: https://zfhuang99.github.io/tla+/pluscal/chatgpt/2023/09/24/TLA-made-simple-with-chatgpt.html

[^disclaimer]: Opinions are personal.