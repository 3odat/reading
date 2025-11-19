Awesome, you’re noticing exactly the right “mystery point” in the trace. Let’s answer your questions very directly, then walk one full turn like an interpreter.

Your questions:

Who calls tools?

How does the framework know which tool to call and with what args?

What does the tool return, and how does that become part of state / memory?

What’s the full workflow for one turn (state → prompt → tool call → state)?

I’ll answer in that order, then replay Turn 1 from your secure-session-1 trace.

1. Who actually “calls” the tools?

There are two levels:

a) The LLM decides what to call

This line:
