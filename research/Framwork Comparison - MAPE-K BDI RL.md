MAPE-K

Explainer

MAPE-K is an autonomic (self-managing) system architecture popularised in autonomic computing. It decomposes adaptation into a control loop:
	•	Monitor: observe signals/events from the system and environment
	•	Analyze: interpret signals, detect patterns, diagnose state
	•	Plan: select an adaptation strategy
	•	Execute: enact changes through effectors
	•	Knowledge: shared model used across the loop (policies, history, system model)

It’s best understood as a feedback control loop for software systems, where the system continuously senses, reasons, and acts.

Key strengths:
	•	Clean separation of concerns (observe → reason → decide → act)
	•	Works well for operational autonomy (scaling, healing, optimisation)

Key limitations:
	•	Often underspecified about representations (what is “knowledge” exactly?)
	•	Can become hand-wavy in the Analyze/Plan boundary without formal semantics

Integration summary to GAAO / ILA-style agent

A clean mapping looks like this:
	•	Monitor → Event Fabric (E)
Events are the monitored stream. If your agent is event-sourced, monitoring is “append and subscribe.”
	•	Analyze → Condition evaluation / interpretation (C)
This is where raw events become state claims and condition profiles.
	•	Plan → Policy/plan selection (P)
Candidate actions are generated/selected under constraints.
	•	Execute → Dispatch / actuation (Δ)
Actions are committed and produce new events.
	•	Knowledge → Ontology + constraints + models (Ω, K, plus reference stores)
In a formal architecture, “K” is not a blob: it’s a set of typed structures (constraints, schemas, world models, policy definitions).

Practical design note: In GAAO terms, MAPE-K is a macro-loop that your internal tuple makes precise. MAPE-K gives you the control loop story; GAAO provides the data structures and semantics that make it computable and auditable.

⸻

BDI (Belief–Desire–Intention)

Explainer

BDI is a cognitive agent architecture model. It frames agency as:
	•	Beliefs: what the agent holds as true about the world (possibly incomplete/incorrect)
	•	Desires: goals or objectives the agent would like to achieve (can conflict)
	•	Intentions: chosen commitments the agent is currently pursuing (goals + partial plan)

Typical BDI loop:
	1.	Perceive world updates → revise beliefs
	2.	Deliberate about goals → choose or reprioritise desires
	3.	Commit to intentions → select plans
	4.	Execute steps
	5.	Reconsider when beliefs change or intention fails

Strengths:
	•	Great for explaining commitment, goal management, and plan-based action
	•	Human-legible (useful for reasoning about agent behaviour)

Limitations:
	•	Belief revision and intention reconsideration can be underspecified in practice
	•	Doesn’t inherently solve learning (how beliefs/policies improve over time)

Integration summary to GAAO / ILA-style agent

Mapping looks like:
	•	Beliefs → Condition/state claims derived from events (C) + knowledge structures (Ω/K)
In an event-sourced agent, beliefs are often reconstructed state plus derived inferences.
	•	Desires → Goals, utilities, preferences, or target conditions (P and/or Ω)
Desires can be represented as objective functions, goal sets, or constraint targets.
	•	Intentions → Active commitments in the policy/plan layer (P) + constraint binding (K)
Intentions become “the committed policy instance” + “the active constraints and priorities that bind it.”

The key upgrade you want in GAAO is: make beliefs auditable by tying them to event provenance, and make intentions formally bind to constraints over time (not just “the agent says it intends”).

⸻

Reinforcement Learning

Explainer

Reinforcement Learning (RL) is learning through interaction: an agent takes actions in an environment to maximise cumulative reward.

Core elements:
	•	State (s): representation of the situation
	•	Action (a): choice the agent can make
	•	Reward (r): scalar feedback signal
	•	Policy (π): mapping from state → action (or distribution over actions)
	•	Value function (V/Q): expected return from states/actions
	•	Environment dynamics: how actions change state

The RL loop:
	1.	Observe state
	2.	Choose action via policy
	3.	Receive reward + next state
	4.	Update policy/value estimates

Strengths:
	•	Powerful for improving behaviour from feedback (especially when rules are hard to hand-code)
	•	Works well when you can define reward clearly and run many trials

Limitations:
	•	Reward specification is brittle (“reward hacking”)
	•	Sample inefficiency and safety issues in real-world systems
	•	Often requires careful state representation (which can itself be the hard part)

Integration summary to GAAO / ILA-style agent

In a GAAO-style architecture, RL plugs in as one possible mechanism for policy improvement:
	•	State → Reconstructed/derived state from event history (E → C → state)
	•	Action → Executions/dispatches (Δ) that emit events
	•	Reward → Evaluations computed from outcomes over time (R or evaluation ledger)
	•	Policy → P (policy store + selector + executor)
	•	Learning updates → X (adaptation/update operator) writing back into P (and possibly Ω)

The best fit is:
	•	Event log gives you training data (trajectories)
	•	Condition evaluation gives you state features
	•	Policy layer is the “thing being learned”
	•	Constraint fabric is your safety/guardrail wrapper around learned policies

You get a formal story like:
RL updates are constrained updates (not freeform), evaluated against K, and provenance-tracked via E.

⸻

ACT-R

Explainer

ACT-R (Adaptive Control of Thought—Rational) is a cognitive architecture for modelling human cognition. It’s built around:
	•	Declarative memory: “facts” stored as chunks
	•	Procedural memory: production rules (“if condition then action”)
	•	Modules: specialised systems (perception, motor, goal, memory)
	•	Buffers: limited-capacity working areas for module outputs
	•	Production selection: competing rules fire based on utility/activation

It aims to explain:
	•	How humans retrieve knowledge
	•	How habits and skills form
	•	Timing/latency of cognition under constraints

Strengths:
	•	Strong for modelling bounded cognition and memory dynamics
	•	Useful conceptual language for “what is in working memory vs long-term memory”

Limitations:
	•	Heavyweight if you’re not actually modelling human cognition
	•	Less directly aligned to modern ML policy learning unless used abstractly

Integration summary to GAAO / ILA-style agent

ACT-R is best used as an inspiration for memory and decision mechanics, not as a full implementation target.

Mappings:
	•	Declarative memory → knowledge stores (Ω) + retrieval mechanisms
	•	Procedural memory → rule/policy library (P), especially if you support symbolic policies
	•	Buffers / working memory → active context window: current condition profile + active constraints + current intention
	•	Production selection → policy arbitration / action selection under constraints (P + K)

If you want ACT-R’s value without the baggage, the actionable takeaway is:
	•	Make “what’s active right now” explicit (buffers/context window)
	•	Make retrieval costs/latency and attention limits modelable (even if crudely)
	•	Treat some behaviour as rule-based productions and others as learned policies

In ILA terms, ACT-R can inform how you design attention, working set, and retrieval ergonomics in your system.

⸻

Event-sourced architectures (state reconstruction from event streams)

Explainer

Event sourcing stores the system of record as an append-only log of events, not as mutable current state.
	•	Event: an immutable fact (“OrderPlaced”, “PaymentCaptured”, “ConstraintActivated”)
	•	Stream: ordered events for an entity/aggregate
	•	State reconstruction: current state is computed by replaying events
	•	Projections/read models: derived views optimised for queries
	•	Time travel: you can reconstruct state “as of” any past point
	•	Auditability: you can show why the system believes what it believes (provenance)

Strengths:
	•	Perfect audit trail
	•	Great for debugging and deterministic replay
	•	Enables multiple read models and retrospective analysis

Limitations:
	•	Requires careful event schema design and versioning
	•	Replays can be expensive (often need snapshots)
	•	“Current truth” is derived; consistency must be managed deliberately

Integration summary to GAAO / ILA-style agent

Event sourcing is almost a natural substrate for GAAO-like agents:
	•	Event Fabric (E) is literally the event log (the canonical record)
	•	Condition evaluation (C) is a projection layer: events → state/conditions
	•	Constraint Fabric (K) becomes auditable: constraint changes are events; active constraints can be reconstructed for any time slice
	•	Policy evolution (P/X) becomes traceable: policy updates are events; you can replay and compare behaviour across policy versions
	•	You can support:
	•	“Explain why you did X” (replay + decision trace)
	•	“What would you have done under policy v2?” (counterfactual replay with alternative policy binding)

This also gives you a clean “monitor” foundation: monitoring is subscription to the event stream; the agent’s world model is a projection.