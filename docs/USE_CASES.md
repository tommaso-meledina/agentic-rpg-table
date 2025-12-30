# Use-case model

## Actors

### Human Actors

- **Observer**
  A human user who watches the game unfold in real time.

- **Operator**
  A human responsible for configuring, starting, stopping, and supervising game sessions.

---

### System / Agent Actors

- **Game Master Agent (GM Agent)**
  Autonomous agent acting as the game master.

- **Player Agent**
  Autonomous agent acting as a player character.

- **Session Orchestrator**
  A system-level actor responsible for coordinating agent lifecycles, enforcing policies, handling errors, and managing session state.
  *This actor represents the “system” in a use-case sense, without implying monolithic implementation.*

- **Deterministic Mechanics Engine**
  Executes deterministic game logic such as dice rolls and rule-based calculations.

- **Chronicle Store**
  Persistent storage of session chronicles and summaries.

- **Knowledge Repository**
  Storage and access layer for rules, lore, adventures, and other static resources.

---

## Use Case List (Actor-Validated)

---

## 1. Game System Configuration (Prepare & Configure)

#### UC-4 — Select RPG System
- **Primary Actor:** Operator
- **Supporting Actors:** Knowledge Repository
- **Goal:** Choose which tabletop RPG system the session will use.

---

#### UC-5 — Load Game Content
- **Primary Actor:** Operator
- **Supporting Actors:** Knowledge Repository
- **Goal:** Load rules, lore, adventures, and character schemas for the selected RPG.

---

## 2. Agent Setup and Initialization

#### UC-6 — Initialize GM Agent
- **Primary Actor:** Session Orchestrator
- **Supporting Actors:** Knowledge Repository
- **Goal:** Instantiate the GM agent with appropriate role, permissions, and knowledge access.

---

#### UC-7 — Initialize Player Agent
- **Primary Actor:** Session Orchestrator
- **Supporting Actors:** Knowledge Repository
- **Goal:** Instantiate a Player agent with a specific personality and PC character sheet.

---

#### UC-8 — Enforce Knowledge Access Control
- **Primary Actor:** Session Orchestrator
- **Supporting Actors:** Knowledge Repository
- **Goal:** Ensure each agent accesses only authorized knowledge.

---

## 3. Session Lifecycle Management (Underlying Mechanics)

#### UC-1 — Initialize Game Session
- **Primary Actor:** Operator
- **Supporting Actors:** Session Orchestrator, GM Agent, Player Agent
- **Goal:** Start a new game session with selected RPG system, adventure, and agents.

---

#### UC-2 — Resume Game Session
- **Primary Actor:** Operator
- **Supporting Actors:** Session Orchestrator, GM Agent, Player Agent, Chronicle Store
- **Goal:** Resume a previously saved game session using persisted chronicles.

---

#### UC-3 — Terminate Game Session
- **Primary Actor:** Operator
- **Supporting Actors:** Session Orchestrator, GM Agent, Player Agent, Chronicle Store
- **Goal:** Gracefully stop a running game session and persist final state.

---

## 4. Gameplay Interaction (Actual Play Time)

#### UC-9 — Conduct Narrative Turn
- **Primary Actor:** GM Agent
- **Supporting Actors:** Player Agent
- **Goal:** Describe the current situation and advance the narrative.

---

#### UC-10 — Perform Player Action
- **Primary Actor:** Player Agent
- **Supporting Actors:** GM Agent
- **Goal:** Declare and narrate a PC action in response to the game state.

---

#### UC-11 — Adjudicate Player Action
- **Primary Actor:** GM Agent
- **Supporting Actors:** Deterministic Mechanics Engine
- **Goal:** Determine the outcome of a Player action according to game rules.

---

#### UC-12 — Perform Dice Roll
- **Primary Actor:** GM Agent or Player Agent
- **Supporting Actors:** Deterministic Mechanics Engine
- **Goal:** Execute a rule-mandated random roll and obtain an authoritative result.

---

#### UC-13 — Control NPC Behavior
- **Primary Actor:** GM Agent
- **Supporting Actors:** Deterministic Mechanics Engine
- **Goal:** Decide and resolve NPC actions.

---

## 5. Error Handling and Recovery

#### UC-14 — Handle Agent Error
- **Primary Actor:** Session Orchestrator
- **Supporting Actors:** GM Agent, Player Agent
- **Goal:** Detect, categorize, and gracefully manage agent errors without disrupting the session.

---

#### UC-15 — Recover from Partial Agent Failure
- **Primary Actor:** Session Orchestrator
- **Supporting Actors:** GM Agent, Player Agent
- **Goal:** Allow the session to continue or recover when one or more agents fail.

---

## 6. Persistence, Memory, and Observation

#### UC-16 — Persist Session Chronicle
- **Primary Actor:** Session Orchestrator
- **Supporting Actors:** Chronicle Store
- **Goal:** Persist all relevant session events and outcomes.

---

#### UC-17 — Summarize Past Sessions
- **Primary Actor:** Session Orchestrator
- **Supporting Actors:** Chronicle Store
- **Goal:** Generate summaries of past sessions for context rehydration.

---

#### UC-18 — Stream Game Events to Observers
- **Primary Actor:** Session Orchestrator
- **Supporting Actors:** Observer
- **Goal:** Present real-time game interactions to human observers.

---

#### UC-19 — Monitor Session Health
- **Primary Actor:** Operator
- **Supporting Actors:** Session Orchestrator
- **Goal:** Observe the operational status of the session and agents.

---

## Rationale for the “Session Orchestrator” Actor

- Replaces the ambiguous “System” actor.
- Represents **coordination, policy enforcement, and lifecycle management**, not business gameplay logic.
- Maps cleanly to:
  - Process supervisors
  - Agent runtimes
  - Control planes
- Avoids prematurely committing to:
  - Monolith vs microservices
  - Centralized vs distributed execution

This actor will become pivotal in:
- The **System Context Diagram**
- Cross-cutting concerns (security, observability, fault tolerance)
- Several early Architectural Decisions

---

If you agree with this actor normalization, the next natural step is to **derive a Use Case Model** (actors + associations), which will transition cleanly into the **System Context Diagram**.
