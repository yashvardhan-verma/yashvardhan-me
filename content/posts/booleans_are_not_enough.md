+++
title = "When Booleans are not enough....STATE MACHINES ???"
date = "2025-06-11"

[taxonomies]
tags=["automata", "state-machines"]

[extra]
repo_view = true
comment = true
+++

<!-- This blog post is inspired from a talk delivered at SFPython by Joseph Harrington. -->

## Problem with multiple booleans

Booleans are essential to build any business logic, but the use of multiple boolean can lead to a messy codebase which is hard to understand and maintain. Messy codebase makes it difficult to find things and discourages anyone from touching it, using multiple booleans leads to code that becomes messy, unmanageable, and undesirable to work with.

Here's a detailed breakdown of the problems identified with using multiple booleans:

- **Insufficient Information** : A single boolean lacks the sufficient information, and is unable to provide useful information about the system's other state.

- **Proliferation of Booleans and Logic Complexity** : To solve the problem of insufficient information devs tends to add more booleans which quickly leads to a proliferation of flags. 

- **Fragility and Breaking Changes** : The logic built around these multiple booleans is highly prone to breakage. The moment a new state is introduced, any existing conditions or business rules defined using these booleans are likely to break.

To get a better understanding, consider the lifecycle of an order placed on an online store. To manage the orders status we have to define the multiple booleans like `is_pending`, `is_processing`, `is_shipped`, `is_delivered`, `is_canceled`.
At a moment a order might be in `is_pending` state, and to ensure this all other states must be set to `false`.
And when we manage the business logic, we need to consider these business rules which adds the weights of managing other boolean states.

- An order can only be shipped if it is currently processing.
- An order can only be delivered if it has been shipped.
- An order can be canceled if it is pending or processing, but not if it's already shipped or delivered.
- An order cannot be processed if it's already canceled or delivered.

and at this point is we try to add a new state like `AWAITING_PAYMENT` or `RETURNED` the overall implementation needs to be changed, which may break the application.

## SOLUTION : State Machine.
A state machine is defined as a "mathematical model of computation with a finite number of states, transitions between states, and a machine that can only be at a [single] state in a given time". Despite its complex-sounding definition, it can be visualized simply as a "directed graph where each of the nodes represents a state in the machine and the connections between the nodes are representing the transitions". A "pointer" indicates the current state at any given time.

**TO Design a State Machine**

- Define a finite number of states : For our order, these would be `is_pending`, `is_processing`, `is_shipped`, `is_delivered`, `is_canceled`. 

- Lay down the transitions between states: This means translating "the business rules into edges in the graph"

<!-- ## Why, how and when to use state machines.

When we use multiple booleans to represent a business logic it gets messy and hard to maintain and even to understand.

```python
class Video:
    def __init__(self, source):
        self.source = source
        self.is_playing = False

    def pause(self):
        self.is_playing = False

    def play(self):
        self.is_playing = True
    
    def stop(self):
        self.is_playing = False
``` -->
