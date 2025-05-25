---
layout: post
title:  The Coming AI Revolution in Distributed Systems
categories: [GitHub Copilot, Formal verification, TLA+]
---

Formal verification is widely recognized for uncovering subtle, elusive bugs early in distributed system design [[1]]. AI significantly accelerates this verification process [[2]], but recent advancements suggest an even more transformative potential: AI can now autonomously generate accurate formal specifications directly from large-scale production codebases. This breakthrough points toward a future where AI-driven correctness verification becomes standard practice, potentially surpassing human capabilities [^disclaimer].

## Recap: AI-Driven Bug Discovery

Our recent experience with GitHub Copilot Agent demonstrates AI’s ability to autonomously produce precise TLA+ specifications from Azure Storage's production source code. Remarkably, this AI-generated specification uncovered a subtle race condition previously missed by traditional code inspections and rigorous testing, highlighting AI's transformative potential in ensuring correctness in distributed systems.

### Crafting a Plan

We began by instructing AI to formulate a detailed, step-by-step plan for analyzing a specific feature and generating a corresponding TLA+ specification. AI outlined an 8-step strategy, including:

* Cataloging relevant code paths, data structures, and Paxos/server commands.
* Extracting behaviors and invariants from source code.
* Drafting a minimal yet complete TLA+ model.
* Incorporating failure modes and safety/liveness invariants.

### Executing the Plan

**1. Autonomous Source Code Analysis and Initial Specification**

With minimal input — just the feature name and directory — AI autonomously identified relevant code, extracted necessary details, created architecture and behavior documentation, and generated an initial TLA+ specification with over 10 invariants. Writing TLA+ specifications involves careful abstraction and simplification; AI adeptly handled this, effectively balancing detail and abstraction.

**2. Refining the Specification**

When prompted to include a previously missed safety mechanism (optimistic concurrency control), AI re-analyzed the source code, updated documentation, and iteratively refined the TLA+ specification.

**3. Validation through Model Checking**

AI swiftly corrected an initial specification error identified by TLA+ model checking. Subsequent model checking revealed a critical violation of the primary safety invariant, with AI pinpointing the exact problematic sequence involving concurrent deletion and reference addition.

**4. Enhancing via Git History Analysis**

AI leveraged git MCP to analyze commit history, uncovering another missed critical safety mechanism (pessimistic locking). It promptly incorporated this knowledge into the specification and documentation.

### Identifying the Race Condition

Within just a few hours of interactive refinement, AI surfaced a rare but critical race condition occurring when an old Paxos primary performed a deletion concurrently with a new primary adding a reference.

## A Bold Prediction

Considering the sophistication of the AI-generated TLA+ specification — and speaking as someone with over a decade of experience manually crafting such specifications — I must acknowledge that the AI-produced version rivals human-crafted work. This realization underscores a profound insight: all essential components to fully automate correctness verification in distributed systems are now within reach.

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