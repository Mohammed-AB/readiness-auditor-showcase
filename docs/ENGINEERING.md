# Agent-Readiness Auditor: engineering notes

Documentation can read well and still leave an agent unable to complete a task. Agent-Readiness Auditor evaluates the task attempts, tool calls and missing information behind that gap.

## System path

~~~mermaid
flowchart LR
  Documentation --> AgentAttempt
  TaskDefinition --> AgentAttempt
  AgentAttempt --> BoundedToolbox
  BoundedToolbox --> ReceiptsAndFailures
  ReceiptsAndFailures --> Findings
  Findings --> BeforeAfterComparison
~~~

This diagram describes the demonstrated system boundary, not a production deployment topology.

## Decisions and tradeoffs

### Evaluate tasks instead of prose quality

The agent must perform concrete actions such as authenticated creation and paginated retrieval, leaving task-level receipts.

### Keep errors as useful evidence

A 422 response and its later correction remain visible in the trace rather than being hidden behind a final success label.

### Compare the documentation change

Before/after runs preserve task identity and findings so a documentation fix can be judged against the same goal.

## Review questions

- What evidence demonstrates **evaluate tasks instead of prose quality**, and what would fail without it?
- What evidence demonstrates **keep errors as useful evidence**, and what would fail without it?
- What evidence demonstrates **compare the documentation change**, and what would fail without it?

## Verification boundary

The recorded live run completed three tasks: authenticated creation, cursor pagination and a 422-to-201 correction. Bounded deadlines, budget reservation and sandbox boundaries are covered in the shared live-agent verification. Desktop and narrow-screen results inspected.

The toolbox and tasks are controlled examples. This does not claim comprehensive evaluation of an arbitrary production API or every model.
