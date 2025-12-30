# Requirements and constraints

This document captures the initial set of requirements derived from the high-level business description of the agentic AI tabletop role-playing system. Requirements are categorized into **Functional Requirements (FR)**, **Non-Functional Requirements (NFR)**, and **Constraints (C)**.

The lists below are intentionally exhaustive and explicit. At this stage:
- Requirements are expressed in *what* terms, not *how*.
- No architectural or technological commitments are made unless they were explicitly stated by the business description.
- Some requirements may later be refined, merged, or split during subsequent steps.

---

## Functional Requirements (FR)

### FR-1 — Agent Roles and Responsibilities
- The system shall support multiple autonomous agents with distinct roles:
  - One **Game Master (GM)** agent.
  - One or more **Player** agents.
- Each Player agent shall impersonate a single **Player Character (PC)**.

### FR-2 — Rule Knowledge and Enforcement
- All agents shall have access to the complete ruleset of the selected tabletop RPG.
- Agents shall apply game rules consistently and accurately during gameplay.
- The GM agent shall be responsible for interpreting and enforcing rules where adjudication is required.

### FR-3 — Game System Agnosticism
- The system shall support multiple tabletop RPG systems.
- Switching from one RPG system to another shall be achievable by replacing or reconfiguring static resources (e.g. rulebooks, lore, character schemas), without modifying core system logic.

### FR-4 — Narrative Management
- The GM agent shall:
  - Follow a predefined adventure or storyline at a high level.
  - Dynamically improvise narrative details in response to player actions.
- The narrative flow shall not rely on a fixed or linear script.

### FR-5 — Player Autonomy and Personality
- Each Player agent shall operate independently from other Player agents.
- Each Player agent shall:
  - Exhibit a distinct personality.
  - Make decisions based on its personality and its PC’s attributes.
- Player agents shall not share internal reasoning or intentions unless expressed in-game.

### FR-6 — Character Sheet Awareness
- Each Player agent shall have access to and act according to its PC’s character sheet.
- The GM agent shall have access to all PC character sheets.
- Character sheets shall influence decision-making, success chances, and narrative outcomes.

### FR-7 — Turn-Based Interaction Model
- The system shall support conversational, turn-based interactions resembling a real tabletop RPG session.
- Interaction order shall be flexible and context-driven rather than strictly scripted.

### FR-8 — Dice Rolling and Random Resolution
- The system shall support dice rolls and other random mechanics required by the RPG system.
- Player agents shall request rolls when required by the rules.
- The GM agent shall:
  - Request rolls from Player agents when appropriate.
  - Perform rolls for Non-Playing Characters (NPCs).

### FR-9 — Deterministic Game Mechanics
- All deterministic mechanics (e.g. dice rolling, rule calculations) shall be executed by non-LLM, deterministic components.
- Agents shall consume the results of deterministic operations as authoritative inputs.

### FR-10 — Non-Playing Characters (NPCs)
- The GM agent shall manage NPCs, including:
  - NPC statistics.
  - NPC actions and dialogue.
- NPC behavior shall conform to game rules and narrative context.

### FR-11 — Knowledge Access Control
- Agents shall have access only to the resources appropriate to their role.
  - Shared resources: rules, public lore, setting information.
  - Restricted resources: adventure plots, NPC stats, hidden information (GM-only).
- The system shall enforce role-based access to knowledge.

### FR-12 — Session Chronicle Persistence
- The system shall persist a complete chronicle of each game session.
- The chronicle shall capture:
  - All relevant narrative events.
  - All agent interactions.
  - All rolls and outcomes.
- Persisted chronicles shall enable session replay and continuation.

### FR-13 — Session Continuity
- The system shall support stopping and restarting gameplay across multiple sessions.
- Agents shall be able to resume gameplay using persisted chronicles and state.

### FR-14 — Observer User Interface
- The system shall provide a user interface for human observers.
- Observers shall be able to view agent interactions in real time.
- The interface shall be read-only in the initial phase.

### FR-15 — Shared Table Communication Model
- All in-game communication shall be visible to all agents and observers.
- The system shall conceptually support private or restricted communication as a future extension (Phase 2).

#### FR-16 — Graceful Agent Failure Handling
- The system shall handle all errors encountered by agents in a graceful, non-disruptive manner.
- Errors shall not cause the entire system or session to fail by default.

#### FR-17 — Error Categorization
- The system shall classify errors into a limited, well-defined set of error categories (e.g. transient, recoverable, fatal).
- Unknown errors shall fall back to a generic error category.

#### FR-18 — Agent-Facing Error Abstraction
- Agents shall receive only **abstracted, high-level information** about errors encountered.
- Agents shall not be exposed to raw stack traces, internal exception details, or infrastructure-level diagnostics.

#### FR-19 — In-Game Error Expression
- When an error affects an agent’s ability to act, the agent shall:
  - Either express the issue in natural language as part of the game conversation (e.g. “I am currently unable to act because …”), or
  - Remain silent, as if skipping a turn.
- Error expression shall not break immersion or introduce out-of-world technical language.

#### FR-20 — Session Continuity Under Partial Failure
- The system shall allow gameplay to continue when:
  - A single Player agent fails.
  - The GM agent temporarily degrades but can recover.
- Recovery behavior shall favor narrative continuity over strict correctness.

#### FR-21 — Chronicle-Based Session Rehydration
- At the start of a new session, the GM agent shall:
  - Retrieve relevant past session chronicles.
  - Use them to reconstruct the narrative state of the game.
- Player agents shall receive sufficient narrative context to resume coherent play.

#### FR-22 — Narrative Summarization
- The system shall support summarization of past chronicles to:
  - Fit within LLM context constraints.
  - Preserve narrative, mechanical, and character-relevant continuity.
- Summaries shall be treated as authoritative session memory.

---

## Non-Functional Requirements (NFR)

### NFR-1 — Modularity and Extensibility
- The system shall be modular to support future extensions, including:
  - Additional RPG systems.
  - Private communication channels.
  - Human player participation.
  - Alternative user interfaces.

### NFR-2 — Maintainability
- The system shall be structured to allow:
  - Independent evolution of agents, rules, and UI.
  - Replacement or upgrade of LLMs without redesigning the system.

### NFR-3 — Consistency and Coherence
- The system shall ensure narrative and mechanical consistency across long-running sessions.
- Agents shall maintain coherent behavior over time.

### NFR-4 — Observability
- The system shall provide sufficient logging and tracing to:
  - Reconstruct session events.
  - Diagnose incorrect rule applications or agent behavior.

### NFR-5 — Determinism and Auditability
- Outcomes of deterministic mechanics shall be reproducible and auditable.
- Random events shall be traceable (e.g. logged dice rolls and seeds).

### NFR-6 — Performance
- The system shall support near-real-time interaction suitable for human observation.
- Latency shall not materially disrupt turn-based gameplay.

### NFR-7 — Security
- The system shall prevent unauthorized access to restricted knowledge.
- Agents shall not be able to infer or access information beyond their assigned permissions.

### NFR-8 — Portability
- The solution shall be deployable in different environments (e.g. local, cloud) without redesign.

#### NFR-9 — Fault Tolerance
- The system shall tolerate partial failures of agents or subsystems without cascading failures.
- Faults shall be isolated to the minimum possible scope.

#### NFR-10 — Context Efficiency
- The system shall optimize the amount of historical information injected into agent contexts to balance:
  - Narrative fidelity
  - Cost
  - Performance
  - LLM context window limitations

---

## Constraints (C)

### C-1 — Agentic Architecture
- The solution shall be based on an agentic AI paradigm with multiple autonomous agents.

### C-2 — LLM-Driven Agents
- Each agent shall be powered by a Large Language Model for reasoning and dialogue generation.

### C-3 — Deterministic Tooling for Mechanics
- Deterministic operations (e.g. dice rolls) shall not be delegated to LLMs.
- Such operations shall be executed via deterministic tools (e.g. MCP tools).

### C-4 — No Human Interaction (Phase 1)
- Human users shall not actively participate in gameplay in the initial phase.
- Humans are limited to observer roles.

### C-5 — No Private Communication (Phase 1)
- Private or partially restricted communication between agents shall not be implemented in the initial phase.
- The architecture must not preclude its future introduction.

### C-6 — Content-Driven Game Switching
- Game-specific behavior shall not be hardcoded.
- RPG systems shall be selectable via content and configuration.

### C-7 — Implementation Language Preference
- All agent implementations shall be coded in **Python**.
- Deviations from Python are permitted only if a strong, well-documented technical justification exists (e.g. lack of required libraries, prohibitive performance limitations, or critical ecosystem gaps).

---

## Open Points / To Be Validated Later
- Degree of strictness in rule enforcement vs. narrative flexibility.
- Granularity and structure of the persisted chronicle.
- Definition of “real-time” from a performance standpoint.
- Scope of observer UI (text only vs. enriched visualization).

These open points will be revisited during subsequent steps and may result in additional requirements or constraints.
