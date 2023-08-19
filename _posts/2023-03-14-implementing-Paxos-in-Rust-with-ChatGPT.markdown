---
layout: post
title:  Implementing Paxos in Rust with ChatGPT
categories: [Rust, ChatGPT]
---

## Introduction

This article provides an overview of my experience implementing Paxos in Rust, aided by the advanced language model, ChatGPT. This journey, which involved a thorough ChatGPT session spanning multiple days, covered an extensive range of topics. These stretched from mastering fundamental language constructs to troubleshooting compiler errors and exploring advanced areas such as concurrency, asynchronous programming, and model checking.

The dynamism of this interaction led to a noticeable surge in my productivity, enabling me to complete a substantial amount of work in a rather condensed timeframe. Impressively, the entire project was wrapped up in less than a week, with roughly 2-3 hours invested on weekdays and an additional half-day over the weekend.

I would be remiss not to mention that this was my *inaugural* Rust project. Despite having a basic grasp of the language from prior resources like "Rust by Example" and the completion of "rustlings"—a compilation of 94 mini assignments designed to introduce Rust concepts—this project truly underscored the remarkable capabilities of ChatGPT in alleviating Rust's infamously steep learning curve.

The experience left me profoundly convinced of the transformative potential that the synergy of Rust and ChatGPT holds for the field of *infrastructure software development*. Rust's unique ability to deliver code devoid of memory leaks, crashes, and race conditions ensures a robust and efficient software infrastructure. Coupling this with the problem-solving prowess of ChatGPT can help us navigate the more intimidating facets of Rust, including its compiler errors and learning curve.

Moreover, this expedition offered an insight into the probable future of AI-assisted programming. With the support of AI, developers' productivity could reach new heights, enabling the generation of significantly larger volumes of code. However, it's essential to bear in mind that AI-generated code is not inherently bug-free. To ensure the robustness of our software infrastructure in the face of an influx of new code, a proactive approach is needed. Rust, with its prowess in minimizing low-level bugs, is a formidable tool for this task. To navigate the realm of high-level bugs, we must rely on formal methods like TLA+ and other formal verification techniques.

### Why Paxos

Paxos is one of the most fundamental protocols anchoring many distributed systems. A simple implementation of Paxos might involve two proposers each trying to commit their own value among three acceptors. This process involves concurrency, as the requests from the proposers may compete with each other at any of the acceptors in an arbitrary sequence. Furthermore, the process involves asynchrony. When a proposer sends requests to the acceptors, the responses might return in any order or not at all, in the event of network issues. Implementing Paxos provides an opportunity to thoroughly comprehend both the concurrency and asynchrony aspects of Rust, making it an appealing choice for my inaugural project.


## Highlights of ChatGPT

The comprehensive ChatGPT session, including numerous interactions around compiler errors, is accessible [[1]] for those interested. To offer a snapshot of the expansive spectrum of assistance provided by ChatGPT, I've compiled a few standout moments below.

### Getting started

Here is the initial prompt! Although ChatGPT's comprehensive knowledge of Paxos is impressive, what amazed me even more was its ability to infer that I intended to develop Paxos on a key-value store, deduced merely from the project name.

This also implies that, to maximize the potential of Large Language Models like ChatGPT, we should try to translate the problems we're tackling into concepts that are familiar and easily understood.

<!-- ![getting_started](/assets/images/rkvpaxos_getting_started_dark_v0.jpeg){:width="90%"} -->
![getting_started](/assets/images/rkvpaxos_getting_started_dark.jpg){:width="90%"}

### Define data structure

Here are a few examples of me asking ChatGPT to define some basic data structures and tailoring the definitions based on my need.

![define_data_structure](/assets/images/rkvpaxos_define_data_structure_dark.jpeg){:width="90%"}

### Program async

![write_async_method](/assets/images/rkvpaxos_write_async_method_dark.jpeg){:width="90%"}

### Create unit tests

![create_unit_test_for_given_method](/assets/images/rkvpaxos_create_unit_test_for_given_method_dark.jpeg){:width="90%"}

### Rewrite as Rust native

![pattern_matching](/assets/images/rkvpaxos_pattern_matching_dark.jpeg){:width="90%"}

### Revamp implementation

My interaction with ChatGPT led me to realize that 'channels' might be a powerful primitive for implementing Paxos. Specifically, consider a scenario where a proposer sends a voting request to three acceptors. As soon as it receives responses from at least two of the three, it can conclude a voting round. This can be implemented elegantly using channels—the proposer simply listens on a channel, with each individual response from different acceptors delivered via the same channel. This way, the proposer can implement a straightforward loop and tally the responses, sidestepping the need for complex concurrency primitives. With this insight, I requested ChatGPT to revamp the implementation.

Given that most of my time was spent prompting and editing, this request for a significant rewrite didn't feel particularly taxing.

![entire_method_with_updated_design](/assets/images/rkvpaxos_entire_method_with_updated_design_dark.jpeg){:width="90%"}

### Refactor

By this stage, I had successfully implemented Paxos Phase 1 and Phase 2. Adhering to the principle of prioritizing functionality before optimization, there was some degree of redundancy between the two phase implementations. To my delight, I realized I could simply request ChatGPT to refactor the duplicated code on my behalf.

![refactor_duplicated_code](/assets/images/rkvpaxos_refactor_duplicated_code_dark.jpeg){:width="90%"}

### Model checking

When AWS unveiled their ShardStore paper [[2]] at SOSP, the authors open-sourced a model checking framework for Rust, named Shuttle. Having never explored Shuttle before and having little interest in poring over its documentation, I decided to task ChatGPT with explaining its workings. With just a few prompts, I quickly grasped the essence of the framework and understood how I could potentially incorporate it into my own testing regime—a task for another day.

![model_checking](/assets/images/rkvpaxos_model_checking_dark.jpeg){:width="90%"}

## Future of AI-assisted programming

This journey has deeply convinced me of the imminent rise of Rust as a dominating force in infrastructure software development, potentially surpassing C++.

As more developers begin to embrace AI assistance, a tremendous increase in productivity is predicted, facilitating the generation of much larger volumes of code. However, it's crucial to note that AI-generated code isn't exempt from bugs. A recent study [[3]] revealed that "participants who had access to an AI assistant wrote significantly less secure code" and "were more likely to believe they wrote secure code." While this study centered around security, it's reasonable to infer that its conclusions could be applicable to other areas such as availability and reliability.

In the wake of this anticipated surge in productivity, the need to maintain high reliability within our infrastructure software stack is of utmost importance. The escalating demand for stability is likely to elevate Rust as the most desirable choice. This is largely due to Rust's proficiency in mitigating low-level bugs such as memory leaks, crashes, and race conditions.

However, Rust alone can't guarantee high-level correctness, such as ensuring safety (preventing data loss in storage systems) or liveness (always enabling users to upload and read blobs). Until LLMs evolve to develop profound reasoning capabilities, these aspects will remain as significant areas of expertise for developers. To address high-level bugs, formal methods, such as TLA+ and formal verification, are increasingly being adopted as effective strategies.

As AI-assisted programming becomes the norm, every developer's AI co-pilot will become standardized, making them largely interchangeable. Therefore, the ability to tackle high-level correctness issues effectively will emerge as a key differentiator. This skill will be instrumental in maintaining the relevance of human developers in the rapidly evolving landscape of infrastructure software development.




[1]: https://chat.openai.com/share/263d8cb4-0001-46de-bbea-d2a07de60f9c

[2]: https://www.amazon.science/publications/using-lightweight-formal-methods-to-validate-a-key-value-storage-node-in-amazon-s3

[3]: https://arxiv.org/abs/2211.03622