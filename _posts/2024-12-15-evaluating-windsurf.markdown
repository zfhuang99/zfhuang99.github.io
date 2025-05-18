---
layout: post
title:  Evaluating Windsurf - A Production System Experiment
categories: [Windsurf]
---

## Summary

2025 is projected to be a breakthrough year for AI agents, particularly
in software development. Agentic coding assistants like Cursor and
Windsurf (both forking from VS Code) are evolving rapidly, with
increasingly sophisticated capabilities. While social media abounds with
success stories of these tools democratizing coding --- including
accounts of children as young as eight building functional games ---
their effectiveness in handling complex, real-world codebases remains
largely unexplored.

This experiment examines how Windsurf can be leveraged to develop a new
feature for a sophisticated production component. We specifically
highlight how agentic coding differs from code completion as pioneered
by GitHub Copilot.

Our experiment subject is Microsoft\'s RSL [[1]] (Replicated State
Library), an implementation of the Paxos consensus algorithm that serves
as the core metadata engine for numerous large-scale distributed systems
within Azure. Similar components are used by other major cloud providers
in their production environments. RSL\'s open-source nature makes it an
ideal candidate for evaluating cutting-edge agentic coding assistants.

RSL enables consensus among a group of nodes when a quorum (majority)
remains operational. While this works effectively for single-group
scenarios, we\'ve encountered challenges in sharded systems with
multiple RSL groups. Specifically, we\'ve observed cross-talk --- nodes
from different RSL groups attempting to communicate with each other.
This interference can disrupt RSL\'s operation, potentially causing
system unavailability or compromising correctness. To address this, we
aimed to implement a group ID feature that restricts communication to
nodes sharing the same group identifier.

The implementation of such features is non-trivial. While we had a broad
understanding of RSL, navigating its complex codebase --- with the
single core file alone containing over 7,500 lines of code --- required
deep familiarity with implementation details at the functional level.
Under normal circumstances, just gaining sufficient understanding of the
codebase to implement this feature would have required focused effort
spanning several days. However, with Windsurf\'s agentic capabilities,
we were able to complete the implementation and resolve all build issues
in just **two hours** (not including the development of test cases).

## Agentic Flow Highlights

In this section, we present two examples that led to our eureka
moments in appreciating the power of agentic flow. In subsequent
sections, we cover the flow of our feature development and additional
interactions with the AI agent.

### Example 1

Upon completion of the group ID feature (as detailed in the next
section), we requested the AI agent to review a critical change. Our
objective was to ensure that the change had been applied across all
necessary code paths. With a single prompt, the AI agent meticulously
reviewed the entire file (comprising thousands of lines of code),
identified all relevant methods, determined where changes were
necessary, and generated the requisite modifications.

<p align="center">
  <img src="/assets/images/windsurf/image1.png" width="90%">
</p>

<p align="center">
  <img src="/assets/images/windsurf/image2.png" width="90%">
</p>

<p align="center">
  <img src="/assets/images/windsurf/image3.png" width="90%">
</p>

### Example 2

RSL employs a complex test setup, including a test engine with simulated
legislators, to facilitate testing. In order to augment the test cases
to cover our new feature, we wanted a comprehensive understanding of the
test case setup. Specifically, we sought to determine whether message
passing in the test cases was implemented through mocking or transferred
via TCP. Following a straightforward prompt, the AI agent traced through
both the core RSL library codebase and the test code implementation. It
successfully pieced together relevant information and provided a
detailed answer for our investigation.

<p align="center">
  <img src="/assets/images/windsurf/image4.png" width="90%">
</p>

<p align="center">
  <img src="/assets/images/windsurf/image5.png" width="90%">
</p>

<p align="center">
  <img src="/assets/images/windsurf/image6.png" width="90%">
</p>

## First prompt

Our initial interaction with the AI agent began by directly requesting
the feature implementation, without providing additional context. While
the agent couldn\'t immediately access the codebase and relevant files,
it demonstrated understanding of the task by proposing a viable
implementation strategy.

<p align="center">
  <img src="/assets/images/windsurf/image7.png" width="90%">
</p>

## Clarifying requirements

In its initial response, the agent proactively sought clarification
through targeted questions to better understand the requirements and
context.

<p align="center">
  <img src="/assets/images/windsurf/image8.png" width="90%">
</p>

## Asking for help

When unable to locate the necessary files, the agent explicitly
requested assistance rather than making assumptions or proceeding with
incomplete information.

<p align="center">
  <img src="/assets/images/windsurf/image9.png" width="90%">
</p>

## Proposing detailed implementations

Once the agent successfully located the necessary files, it proposed a
comprehensive implementation strategy.

<p align="center">
  <img src="/assets/images/windsurf/image10.png" width="90%">
</p>

## Updating multiple files

The agent successfully modified multiple files, some containing up to
more than 1,000 lines of code. However, when faced with legislator.cpp
--- the largest file at over 7,500 lines --- the agent encountered
limitations in directly implementing changes. After several unsuccessful
attempts, we adapted our approach by requesting the agent to specify the
necessary modifications, which we then applied manually.

<p align="center">
  <img src="/assets/images/windsurf/image11.png" width="90%">
</p>

## Identifying gaps

After implementing the core functionality, we initiated a new
conversation thread to review the central logic --- the validation and
rejection of messages based on group IDs. This decision to start fresh
helped avoid potential confusion from an oversized context window.
Although we needed to reorient the agent with the correct file paths, it
quickly engaged in meaningful code review.

The agent not only successfully analyzed the code but also identified
additional locations requiring modifications. Through this interactive
process, we (the pilot) discovered a simpler implementation approach.
When presented with this insight, the agent readily adapted and updated
the implementation to align with the simplified solution.

<p align="center">
  <img src="/assets/images/windsurf/image12.png" width="90%">
</p>

<p align="center">
  <img src="/assets/images/windsurf/image13.png" width="90%">
</p>

<p align="center">
  <img src="/assets/images/windsurf/image14.png" width="90%">
</p>

<p align="center">
  <img src="/assets/images/windsurf/image15.png" width="90%">
</p>

## Fixing build errors

As expected, build errors were easily resolved through prompting the
agent for fixes. While RSL\'s specialized build environment precluded
direct agent interaction with the build process, we\'ve observed more
autonomous capabilities in other contexts. In a separate Rust project,
the agent demonstrated full autonomy by successfully resolving all build
errors after four consecutive build-fix iterations.

<p align="center">
  <img src="/assets/images/windsurf/image16.png" width="90%">
</p>

<p align="center">
  <img src="/assets/images/windsurf/image17.png" width="90%">
</p>

### Autonomous iterations

In a separate project with a standard build environment, the agent
demonstrated full autonomy by successfully resolving all build errors
after four consecutive build-fix iterations.

<p align="center">
  <img src="/assets/images/windsurf/image18.png" width="90%">
</p>

<p align="center">
  <img src="/assets/images/windsurf/image19.png" width="90%">
</p>

<p align="center">
  <img src="/assets/images/windsurf/image20.png" width="90%">
</p>

[1]: <https://github.com/Azure/RSL>
