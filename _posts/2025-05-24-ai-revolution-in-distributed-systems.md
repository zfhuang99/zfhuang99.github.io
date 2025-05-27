---
layout: post
title:  AI Discovers Hidden Distributed System Bugs
categories: [GitHub Copilot, Formal verification, TLA+]
---

Formal verification has long been the gold standard for uncovering subtle bugs in distributed system design [[1]]. While AI has already proven its ability to accelerate verification processes [[2]], recent breakthroughs suggest a far more transformative potential: AI can now autonomously generate accurate formal specifications directly from production codebases. This capability marks a pivotal moment in software engineering, pointing toward a future where AI-driven correctness verification becomes not just standard practice, but potentially superior to human efforts [^disclaimer].

## AI Discovers Hidden Bugs

Our recent work with GitHub Copilot demonstrates this potential in action. The AI autonomously produced precise TLA+ specifications from Azure Storage's production source code, uncovering a subtle race condition that had evaded traditional code reviews and extensive testing. This achievement illustrates how AI can transform our approach to ensuring correctness in complex distributed systems.

### Strategic Planning: AI's Methodical Approach

We began by tasking GitHub Copilot (with o3 model) to formulate a comprehensive plan for analyzing a specific feature and generating a corresponding TLA+ specification. The AI responded with an 8-step methodology that demonstrated deep understanding of formal verification principles:

* Systematic cataloging of code paths, data structures, and Paxos/server commands
* Extraction of behavioral patterns and invariants from source code
* Construction of a minimal yet complete TLA+ model
* Integration of failure modes and safety/liveness properties

### From Plan to Execution: AI in Action

**1. Autonomous Code Analysis and Initial Specification**

  - After reviewing the AI-generated plan, we instructed GitHub Copilot Agent (with Claude 3.7 Sonnet model) to execute each step, marking completion as it progressed. Notably, we only provided the feature name and the relevant component directory to narrow the search scope. The AI autonomously identified pertinent files and extracted essential information without further guidance.
  
  - Subsequently, the AI produced comprehensive architecture and behavior documentation, along with an initial TLA+ specification containing over 10 invariants. Impressively, the primary safety invariant matched precisely what we anticipated.

**2. Iterative Refinement of the Specification**

  - Upon reviewing the initial specification, we noticed the absence of an etag-based optimistic concurrency control mechanism. We prompted the AI to investigate how etags were utilized to prevent race conditions. In response, the AI revisited the source code, updated the documentation accordingly, and refined the TLA+ specification to incorporate this critical mechanism.

  - We observed that the AI refined the specification iteratively, updating one atomic action at a time. This incremental approach likely helped the AI maintain precision and accuracy throughout the refinement process.

**3. Validation through Model Checking**

  - With the refined specification ready, we proceeded to validate it using the TLA+ model checker. Initially, the tool reported a language error, which the AI swiftly corrected. Subsequent model checking uncovered a violation of the primary safety invariant. After providing the violation trace to the AI, it quickly identified the problematic sequence — a concurrent deletion and reference addition scenario.

**4. Enhancing the Specification via Git History Analysis**

  - To ensure comprehensive coverage, we asked the AI to analyze the git commit history for the feature using git MCP. This analysis revealed an additional critical safety mechanism involving pessimistic locking, previously overlooked. The AI promptly updated both the documentation and the TLA+ specification to reflect this important discovery.

### The Critical Discovery

Within hours of iterative refinement, the AI had surfaced a critical race condition: an old Paxos primary could perform a deletion while a new primary simultaneously added a reference. This subtle bug had escaped detection through traditional methods, yet given Azure Storage's scale, it would likely have manifested in production eventually.

## Looking Ahead: The Autonomous Future

After a decade of manually crafting TLA+ specifications, I must acknowledge that this AI-generated specification rivals human work. This achievement represents more than incremental progress — it demonstrates that the fundamental components for fully automating correctness verification are now available.

Based on current capabilities, I envision the following evolution:

* **Autonomous System Analysis**: AI will independently dissect large-scale distributed systems, mapping critical components and their interactions.
* **Intelligent Invariant Discovery**: AI will identify and formalize key system invariants without human guidance.
* **Self-Validating Specifications**: AI-generated TLA+ models will undergo automatic validation, with AI analyzing any violations
  * Violations will trigger AI-crafted targeted tests
  * Confirmed bugs will be automatically corrected
  * Model-implementation discrepancies will drive continuous specification refinement
* **Reinforcement Learning Feedback Loop**: Each iteration will generate valuable data, enabling AI to continuously improve its verification capabilities.
* **Mastery Through Clear Objectives**: Like other domains with verifiable outcomes, distributed system correctness will become an area where AI consistently outperforms human practitioners.

While we haven't reached this destination yet, the path forward is clear. Even if AI development paused today, the foundation for an AI-driven correctness revolution has already been laid.

[1]: https://lamport.azurewebsites.net/tla/industrial-use.html

[2]: https://zfhuang99.github.io/tla+/pluscal/chatgpt/2023/09/24/TLA-made-simple-with-chatgpt.html

[^disclaimer]: Opinions are personal.