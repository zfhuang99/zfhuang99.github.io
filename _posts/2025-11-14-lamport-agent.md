---
layout: post
title:  Lamport Agent - AI-assisted Formal Specification
categories: [GitHub Copilot, Formal verification, TLA+]
---

This blog post was prompted by an invitation to present to Nvidia engineers ([slides](/assets/attachments/lamport_agent.pdf)).

In the previous post [[1]], I discussed how large language models (LLMs) are now capable of producing precise formal specifications directly from large production codebases and identifying nuanced race conditions. This process can be largely automated, making it broadly applicable. Accordingly, we have developed Lamport Agent so that others can utilize with their own codebases. The following is an example demonstrating this agent using DeepSeek’s open-source distributed file system, 3FS [[2]]. The complete GitHub Copilot chat history and all the artifacts produced by the agent are available here [[3]].

## CRAQ in DeepSeek 3FS

3FS implements an optimized chain replication protocol, called CRAQ [[4]]. We
aim to model this system and check its consistency.

The charts compare standard chain replication with CRAQ. In standard
replication, only the tail node serves reads for consistency. CRAQ
allows any node to serve reads by using version numbers: writes
increment the object\'s version at the head and are marked dirty until
they commit at the tail. Once committed, the update propagates back,
converting all dirty writes to committed ones. Nodes can serve reads
directly unless they have dirty writes; in that case, they fetch the
latest version from the tail. The process grows more complex when
additional steps are needed to address node failures and repairs.

To ensure strong consistency, all reads must return the most recent
committed write. Our experiment evaluates whether 3FS' implementation
meets this requirement.

  --------------------------------------------------------------- ---------------------------------------------------------------
<p float="left">
<img src="/assets/images/lamport/image1.png" width="45%" />
<img src="/assets/images/lamport/image2.png" width="45%" />
</p>

  --------------------------------------------------------------- ---------------------------------------------------------------

## Lamport Agent in action

To begin, we create a custom agent in GitHub Copilot by following the
provided instructions [[3]].

<p align="center">
  <img src="/assets/images/lamport/image3.png" width="80%">
</p>

The procedure may be initiated through a straightforward initial prompt.

![](/assets/images/lamport/image4.png){:width="100%"}

The agent develops a 6-phase plan, starting with phase 1: studying the
implementation and documenting the architecture and the happy path
logic.

![](/assets/images/lamport/image5.png){:width="100%"}
![](/assets/images/lamport/image6.png){:width="100%"}

A snippet of the happy path markdown.

![](/assets/images/lamport/image7.png){:width="100%"}

![](/assets/images/lamport/image8.png){:width="100%"}

Phase 2 covers failure paths.

![](/assets/images/lamport/image9.png){:width="100%"}

![](/assets/images/lamport/image10.png){:width="100%"}

Phase 3 produces invariants -- 7 safety and 1 liveness in this example.

![](/assets/images/lamport/image11.png){:width="100%"}

![](/assets/images/lamport/image12.png){:width="100%"}

One may choose to stop at this stage and use the agent solely to enhance the understanding of the codebase, without proceeding to the TLA+ section.

![](/assets/images/lamport/image13.png){:width="100%"}

AI is able to identify and fix its own errors, whether they are related
to syntax or logic.

![](/assets/images/lamport/image14.png){:width="100%"}

AI engages in a back-and-forth argument with me, much like a human team
member. This appears to be an emergent behavior in GPT5.1!

![](/assets/images/lamport/image15.png){:width="100%"}

![](/assets/images/lamport/image16.png){:width="100%"}

![](/assets/images/lamport/image17.png){:width="100%"}

We can continue driving AI to improve the specification with minimal
additional effort.

![](/assets/images/lamport/image18.png){:width="100%"}

![](/assets/images/lamport/image19.png){:width="100%"}




[1]: https://zfhuang99.github.io/github%20copilot/formal%20verification/tla+/2025/05/24/ai-revolution-in-distributed-systems.html

[2]: https://github.com/deepseek-ai/3FS

[3]: https://github.com/zfhuang99/lamport-agent

[4]: https://www.usenix.org/legacy/event/usenix09/tech/full_papers/terrace/terrace.pdf
