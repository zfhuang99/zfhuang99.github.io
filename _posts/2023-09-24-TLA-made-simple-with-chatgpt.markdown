---
layout: post
title:  TLA+ Made Simple with ChatGPT
categories: [TLA+, PlusCal, ChatGPT]
---

## More than just a first impression

Back in 2013, I was working hard on my first implementation of the Paxos consensus protocol. When everything was going smoothly, it worked great. But when I tried some tricky test cases, things often went wrong. It was tough making sure my implementation was perfect, especially with so many possible tests to think of.

I thought to myself, "I can't be the only one facing this problem." So, I started looking for better ways to handle these challenges. That's when I talked to some folks at Microsoft Research and heard about the P programming language [[1]]. It felt like I was onto something.

Around the same time, Leslie Lamport was awarded the Turing Award. Not long after the annoucement, Leslie gave a lecture at MSR. The biggest room in building 99 was packed. Everyone wanted to hear from the newest Turing Award winner.

Leslie's talk had a profound impact on me. I can still remember the title like it was yesterday: "Who Builds a Skyscraper without a Blueprint?". He was talking about how some of us try to figure out complex distributed systems while we're writing code, like trying to design a skyscraper while you're already building it.

That was the first time I heard about TLA+ [[2]]. Over the next few years, I realized how important TLA+ was for making sure our distributed systems worked right. I became a big fan of TLA+ and even helped to host the first few multi-day TLA+ training sessions by Leslie himself for everyone at Microsoft.

## Challenges in TLA+ adoption

Despite TLA+ showing promise in real-world systems [[3]], its widespread adoption remains limited. What seems to be holding it back?

One primary challenge is its relatively steep learning curve. Among the available resources, Leslie's lectures [[4]] stand out as the most comprehensive guide. To illustrate, one developer from Azure was able to craft detailed specifications after diving into Leslie's lectures for an entire week. However, for many, this might be an optimistic timeline, with a more realistic learning period often spanning several weeks or even longer.

Another significant concern is the scarcity of resources when facing difficulties. During my time assisting other developers, I observed the struggles they faced in adopting abstract thinking. Without prompt and constructive feedback, refining such skills can be a prolonged journey.

Moreover, the lack of readily available support compounds the issue. When developers grapple with specific aspects of the TLA+ language, finding guidance can often be a challenge in itself.

## TLA+ in the age of LLM

The arrival of Large Language Models (LLM) promises a transformative shift, potentially making TLA+ more accessible to a broader range of developers.

Drawing from my recent experiences, I've recognized the immense value of using ChatGPT to draft TLA+ specifications and iron out language quirks. A particular moment that struck me was when I realized a flaw in my initial design of a distributed system protocol. After modeling the protocol in TLA+ and running the validation, I was presented with an invariant violation, highlighted by a comprehensive error trace. Out of curiosity, I fed this error trace to ChatGPT. Astonishingly, ChatGPT not only pinpointed the core of the mistake but also offered a list of options to refine the protocol. In this endeavor, ChatGPT emerged as a truly invaluable assistant.

Interestingly, each time I've shared this experience with colleagues, they've expressed genuine surprise. It seems that the potential synergy between TLA+ and ChatGPT remains largely undiscovered. This realization motivates this article, aiming to enlighten a broader audience.

## Simplifying TLA+ with ChatGPT

For illustrative purposes, I chose a simple toy consensus protocol to interact with ChatGPT. It's worth noting that even with more complex and real-world protocol designs, the insights remained consistent. Those curious can find the complete ChatGPT session detailed [[5]]. Here are some of the key takeaways.

### Drafting specification

We'll begin by outlining the distributed system challenge at hand. Then, we'll prompt ChatGPT to produce a TLA+ specification using PlusCal.

<p align="center">
  <img src="/assets/images/tla_draft_spec.jpeg" width="100%">
</p>

### Resolving language errors

After copying the specification into a .tla file, we employ the TLA+ toolbox for compilation. If the compilation encounters issues, we turn to ChatGPT for error resolution. Interestingly, ChatGPT tends to repeat certain minor errors. Recognizing these patterns allows us to preemptively address them in subsequent prompts by setting a few guiding rules.

<p align="center">
  <img src="/assets/images/tla_fix_compile_error.jpeg" width="100%">
</p>

### Reviewing specification

Upon reviewing the specification, I noticed that ChatGPT mistook CHOOSE for representing non-determinism. It's important to clarify this with the model. Moving forward, incorporating this clarification into our guidelines for future prompts will be beneficial.

<p align="center">
  <img src="/assets/images/tla_choose_vs_with.jpeg" width="100%">
</p>

### Defining invariants

Having acquired a full TLA+ specification and confirming its correctness through model checking, we can now proceed to establish invariants.

<p align="center">
  <img src="/assets/images/tla_define_invariant.jpeg" width="100%">
</p>

### Interpreting error trace

When I ran the model checking, it flagged an invariant violation. I turned to ChatGPT to help break down and understand this error trace. I was genuinely taken aback by ChatGPT's ability to not just delineate the error trace clearly, but also to shed light on the underlying cause of the error.

<p align="center">
  <img src="/assets/images/tla_analyze_error_trace_1.jpeg" width="100%">
  <img src="/assets/images/tla_analyze_error_trace_2.jpeg" width="100%">
</p>

### Can ChatGPT mend distributed protocols?

It might seem like a tall order, but I decided to challenge ChatGPT: Could it suggest ways to rectify the toy consensus protocol? What stunned me was how ChatGPT didn't just offer one, but a spectrum of potential solutions, weighing the advantages and drawbacks of each. In the end, I went with the most straightforward solution, and ChatGPT re-generated the specification. The revised spec sailed through the verification process seamlessly. Truly remarkable!

<p align="center">
  <img src="/assets/images/tla_mend_protocol.jpeg" width="100%">
</p>

## Conclusion

As AI delivers an unprecedented surge in programming productivity, ensuring the robustness and fail-proof nature of our cloud-scale distributed systems becomes even more paramount. Embracing formal methods like TLA+ and software verification is imperative [[6]]. Through this article, I've aimed to highlight how ChatGPT can be a game-changer in making TLA+ more approachable. Looking ahead, I envision a future where, with tools like ChatGPT, developers can effortlessly construct more resilient infrastructures at scale.

[1]: https://github.com/p-org/P

[2]: https://lamport.azurewebsites.net/tla/tla.html

[3]: https://lamport.azurewebsites.net/tla/industrial-use.html

[4]: https://lamport.azurewebsites.net/video/videos.html

[5]: https://chat.openai.com/share/52147e84-ff08-434a-94c8-769701f4d246

[6]: https://zfhuang99.github.io/rust/chatgpt/2023/03/14/implementing-Paxos-in-Rust-with-ChatGPT.html#future-of-ai-assisted-programming

