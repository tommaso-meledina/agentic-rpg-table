---
stepsCompleted: ['step-01-init', 'step-02-discovery', 'step-03-success', 'step-04-journeys', 'step-05-domain', 'step-06-innovation', 'step-07-project-type', 'step-08-scoping', 'step-09-functional']
inputDocuments: ['BRIEF.md']
workflowType: 'prd'
briefCount: 1
researchCount: 0
brainstormingCount: 0
projectDocsCount: 0
classification:
  projectType: web_app
  domain: general
  complexity: medium-high
  projectContext: greenfield
---

# Product Requirements Document - agentic-rpg-table

**Author:** Tom
**Date:** 2026-01-21T11:48:24Z

## Success Criteria

### User Success

**MVP:**
- Developers can successfully observe agents exchanging messages and following RPG rules through console/logs
- Complete game sessions run end-to-end without critical failures

**Growth:**
- Users can watch complete, coherent game sessions through web UI
- Users understand agent interactions and character behaviors
- Users can observe multiple PCs with distinct personalities and character traits

### Business Success

- Personal project focused on technical exploration of agentic AI landscape
- Success = technical feasibility proven (system works end-to-end)
- Worth building if the system functions as designed

### Technical Success

**MVP:**
- Multi-agent coordination: GM and PC agents communicate reliably
- Rule adherence: Agents follow RPG rules correctly
- Session completion: Full game sessions run without critical failures
- MCP Server integration: Dedicated MCP Server hosting tools and resources (deterministic operations like dice rolls, static resources like rulebooks)

**Growth:**
- Session persistence: Chronicle system allows resuming sessions between system restarts
- Auth/authz system: Proper resource access control enforced (GM secrets, PC character sheets)
- Multi-agent orchestration: System handles multiple PC agents simultaneously
- Character system: Agents play according to their character sheets with distinct personalities

### Measurable Outcomes

**MVP:**
- Agents successfully exchange messages in a coherent game session
- RPG rules are followed correctly (dice rolls, rulebook references work)
- Console/logs show clear agent interactions
- MCP Server provides tools and resources as expected

**Growth:**
- Multiple PC agents operate independently without conflicts
- Character sheets influence agent behavior appropriately
- Auth/authz prevents unauthorized resource access
- Sessions can be resumed from chronicle
- Web UI displays real-time agent interactions clearly

## Product Scope

### MVP - Minimum Viable Product

- 1 GM agent + 1 PC agent
- Dedicated MCP Server hosting tools and resources:
  - Deterministic operations (dice rolling logic)
  - Static resources (RPG rulebook, sourcebooks with setting/lore)
- No authentication/authorization
- Console-based observation (tail logs or similar)
- Basic message exchange between agents
- Agents follow core RPG rules
- Complete game sessions run end-to-end

### Growth - Phase 1: Multi-Player Support

- Support for multiple PC agents (2+ players)
- Each PC agent operates independently
- GM coordinates with multiple players simultaneously
- Message broadcasting: All agents hear what's said at the table

### Growth - Phase 2: Personality & Character System

- Agent personalities: Each PC agent has distinct personality traits
- Character sheets: Each PC has detailed character information (stats, background, quirks)
- Character personalities: Characters have their own flavor/personality separate from agent personality
- Agents play according to their character sheets
- Different players have different personalities

### Growth - Phase 3: Authentication & Authorization

- Elegant auth/authz system mimicking TTRPG table dynamics:
  - Everyone knows the rules (public resources accessible to all)
  - Only GM knows story secrets (restricted GM resources - adventure plots, NPC stats)
  - Each PC knows only their own character sheet (individual PC resources)
- Resource access control enforced properly
- GM has access to restricted additional resources via MCP Server

### Growth - Phase 4: Web UI & Session Persistence

- Web UI for real-time observation of agent interactions
- Chronicle system: Session persistence allowing resumption between sessions
- Complete session history tracking
- Human can "watch the agents play" in real-time by reading messages they exchange

## User Journeys

### Journey 1: Developer - Early Access via Console (MVP)

**Persona:** Alex, a developer fascinated by agentic AI and LLM capabilities. They've been following the evolution of multi-agent systems and want to see how this system actually works in practice.

**Opening Scene:**
Alex hears about an open-source project where AI agents play tabletop RPGs. They're skeptical but curious - can agents really coordinate well enough to play a coherent game? They decide to try it themselves.

**Rising Action:**
- Alex clones the repository from GitHub
- Follows setup instructions to install dependencies
- Configures the system (YAML/database) to set up a game session
- Starts the agentic system
- Opens their terminal and runs `tail -f` on the system logs
- Watches as the system initializes: GM agent loads, PC agent loads, MCP Server connects

**Climax - The "Wow" Moment:**
- The GM agent describes an opening scene: "You stand at the entrance of a dark cave..."
- The PC agent responds in character: "I cautiously approach, checking for traps..."
- Alex sees a dice roll happen via MCP Server: "Rolling Perception... 15"
- The GM responds appropriately: "You notice something glinting in the shadows..."
- The PC makes a character-driven decision: "I'll investigate, but keep my weapon ready..."
- Alex realizes: "Holy shit, they're actually playing D&D. The agents are following rules, staying in character, and making coherent decisions. This isn't just random text generation - this is a real game session."

**Resolution:**
- Alex watches the entire session unfold, impressed by the technical execution
- They appreciate the subtle details: proper rule adherence, character consistency, GM improvisation
- They share their findings with developer friends: "You have to see this - the future is now"
- They look forward to the web UI but are satisfied with console access for now
- Maybe they contribute improvements or experiment with different RPG systems

**Emotional Arc:** Skepticism → Curiosity → Technical appreciation → Genuine entertainment → Excitement about the possibilities

---

### Journey 2: General User - Watching via Web UI (Growth)

**Persona:** Jordan, someone curious about AI capabilities who isn't necessarily technical. They've seen AI demos before but want to see something more substantial - actual AI agents having a coherent, entertaining interaction.

**Opening Scene:**
Jordan sees a link shared on social media: "Watch AI agents play D&D in real-time." They're intrigued - what does that even mean? They click the link, expecting maybe a pre-recorded video or a simple demo.

**Rising Action:**
- Jordan opens the web UI in their browser
- They see a clean interface showing agent messages in real-time
- A session is already in progress, or they start a new one
- They see multiple PC agents with distinct names and personalities
- Messages appear in real-time as agents "speak" to each other
- They notice the GM describing scenes, PCs responding, dice rolls happening

**Climax - The Entertainment Moment:**
- Jordan watches as the GM sets up an encounter: "Three goblins emerge from the shadows, weapons drawn..."
- PC Agent 1 (a bold fighter): "I charge the nearest goblin!"
- PC Agent 2 (a cautious rogue): "Wait, let me check for traps first..."
- PC Agent 3 (a wise wizard): "Perhaps we should try diplomacy?"
- The agents debate strategy in character, each staying true to their personality
- The GM adapts the scene based on their choices
- Jordan realizes: "This is actually entertaining. I'm watching a real game session unfold, and the AI agents are playing it like real players would. Each character has a distinct voice and approach. This is... fun."

**Resolution:**
- Jordan watches the full session, genuinely entertained
- They return later to watch more sessions
- They share the experience: "You have to check this out - AI agents playing D&D and it's actually good"
- They appreciate both the entertainment value and the "wow factor" of the technology
- They might explore different game systems or character configurations

**Emotional Arc:** Curiosity → Confusion → Understanding → Entertainment → Delight → Return engagement

---

### Journey Requirements Summary

**Capabilities Revealed by Developer Journey (MVP):**
- Console/log output system for real-time message observation
- Clear message formatting that shows agent identity and message content
- System initialization feedback (agents loading, MCP Server connecting)
- Dice roll visibility in logs
- Session start/stop controls
- Error handling and logging for debugging

**Capabilities Revealed by General User Journey (Growth):**
- Web UI for real-time message display
- Clean, readable interface showing agent messages chronologically
- Agent identity indicators (GM, PC names, character names)
- Real-time message streaming/updates
- Session management (start new, view existing, resume)
- Message formatting that distinguishes between GM narration, PC dialogue, and system events (dice rolls)
- Responsive design for different screen sizes
- Session history/chronicle viewing

**Common Requirements Across Both Journeys:**
- Real-time message broadcasting (all agents hear everything)
- Clear agent identity in messages
- Dice roll visibility and results
- Session state management
- Message persistence (chronicle system)
- System reliability (sessions don't crash mid-game)

## Web App Specific Requirements

### Project-Type Overview

- Single Page Application (SPA) for real-time observation of agent interactions
- Minimal UI focused on displaying agent messages chronologically
- Backend-focused architecture with web UI as observational interface

### Technical Architecture Considerations

- **Architecture Pattern:** SPA with real-time message streaming
- **Real-time Communication:** WebSocket or Server-Sent Events for live message updates
- **State Management:** Client-side state for message history and session management
- **Backend API:** RESTful or GraphQL API for session management and chronicle access

### Browser Support

- **Target:** Modern browsers only (Chrome, Firefox, Safari, Edge - latest 2 versions)
- **No Support:** Legacy browsers, niche browsers, or older versions
- **Rationale:** Focus on modern web standards for real-time features

### Real-time Requirements

- **MVP:** Not applicable (console-based observation)
- **Growth:** Real-time message streaming from backend to web UI
- **Update Frequency:** Messages appear as agents generate them (near-instant)
- **Connection Management:** Handle reconnection on network issues
- **Message Ordering:** Ensure chronological message display

### Accessibility

- **MVP:** Not applicable (console-based)
- **Growth:** WCAG compliance (level to be determined)
- **Key Areas:** Screen reader support, keyboard navigation, message readability

### Performance Targets

- **Message Rendering:** Smooth scrolling as new messages arrive
- **Session History:** Efficient loading of chronicle/session history
- **Responsive Design:** Works on desktop and tablet (mobile optional)

### Implementation Considerations

- **Framework:** Modern SPA framework (React, Vue, or similar)
- **Real-time Library:** WebSocket client library or SSE implementation
- **Message Format:** Structured message format (JSON) with agent identity, timestamps, content
- **Session Management:** Ability to start new sessions, view existing sessions, resume from chronicle

## Project Scoping & Phased Development

### MVP Strategy & Philosophy

**MVP Approach:** Experience MVP - Focus on delivering the core entertainment value of watching AI agents play TTRPGs coherently. The MVP must demonstrate that the system works end-to-end and provides the "wow factor" of seeing agents follow rules and interact meaningfully.

**Core Value Proposition:** "Wow, the AI is playing D&D realistically, the future is now!"

**MVP Success Criteria:** 
- Developers can observe agents playing a coherent game session
- Agents follow RPG rules correctly
- System demonstrates technical feasibility of multi-agent coordination
- Entertainment value is evident even through console observation

### MVP Feature Set (Phase 1)

**Core User Journeys Supported:**
- Developer Journey: Console-based observation of agent interactions

**Must-Have Capabilities:**
- 1 GM agent + 1 PC agent
- Dedicated MCP Server hosting tools and resources:
  - Deterministic operations (dice rolling logic)
  - Static resources (RPG rulebook, sourcebooks with setting/lore)
- Console-based observation (tail logs or equivalent)
- Basic message exchange between agents
- Agents follow core RPG rules
- Complete game sessions run end-to-end
- Clear message formatting showing agent identity and content
- System initialization feedback (agents loading, MCP Server connecting)
- Dice roll visibility in logs

**Out of Scope for MVP:**
- Authentication/authorization
- Session persistence (chronicle system)
- Web UI
- Multiple PC agents
- Character sheets and personality systems
- Advanced features

### Post-MVP Features

**Phase 2 (Growth - Multi-Player Support):**
- Support for multiple PC agents (2+ players)
- Each PC agent operates independently
- GM coordinates with multiple players simultaneously
- Message broadcasting: All agents hear what's said at the table

**Phase 3 (Growth - Personality & Character System):**
- Agent personalities: Each PC agent has distinct personality traits
- Character sheets: Each PC has detailed character information (stats, background, quirks)
- Character personalities: Characters have their own flavor/personality separate from agent personality
- Agents play according to their character sheets
- Different players have different personalities

**Phase 4 (Growth - Authentication & Authorization):**
- Elegant auth/authz system mimicking TTRPG table dynamics:
  - Everyone knows the rules (public resources accessible to all)
  - Only GM knows story secrets (restricted GM resources - adventure plots, NPC stats)
  - Each PC knows only their own character sheet (individual PC resources)
- Resource access control enforced properly
- GM has access to restricted additional resources via MCP Server

**Phase 5 (Growth - Web UI & Session Persistence):**
- Web UI for real-time observation of agent interactions
- Chronicle system: Session persistence allowing resumption between sessions
- Complete session history tracking
- Human can "watch the agents play" in real-time by reading messages they exchange
- Real-time message streaming (WebSocket/SSE)
- Session management (start new, view existing, resume from chronicle)
- WCAG accessibility compliance

## Functional Requirements

### Agent Management

- FR1: System can create and initialize a GM agent
- FR2: System can create and initialize PC agents
- FR3: System can configure agent personalities (Growth)
- FR4: System can load agent configurations from YAML/database
- FR5: System can manage multiple PC agents simultaneously (Growth)

### Game Session Management

- FR6: System can start a new game session
- FR7: System can stop a game session
- FR8: System can initialize a session with specified agents (GM and PC(s))
- FR9: System can resume a game session from chronicle (Growth)
- FR10: System can track session state and progress

### Message Broadcasting & Communication

- FR11: GM agent can send messages that all agents receive
- FR12: PC agents can send messages that all agents receive
- FR13: System can broadcast messages to all agents at the table
- FR14: System can maintain message order and chronology
- FR15: System can format messages with agent identity and timestamps

### Rule System & Dice Rolling

- FR16: System can perform dice rolls via MCP Server
- FR17: System can access RPG rulebook content via MCP Server
- FR18: Agents can request dice rolls and receive results
- FR19: GM agent can request dice rolls for NPCs
- FR20: System can enforce RPG rules during gameplay
- FR21: Agents can reference rulebook content when making decisions

### Resource Management (MCP Server)

- FR22: System can connect to MCP Server hosting tools and resources
- FR23: System can access deterministic operations (dice rolling) from MCP Server
- FR24: System can access static resources (rulebooks, sourcebooks) from MCP Server
- FR25: GM agent can access restricted resources (adventure plots, NPC stats) from MCP Server (Growth)
- FR26: System can configure which MCP Server resources are available for a game session

### Observation & Monitoring

- FR27: Developers can observe agent messages via console/logs (MVP)
- FR28: System can output messages to console/logs with clear formatting
- FR29: System can display agent identity in console/log output
- FR30: System can display dice roll results in console/log output
- FR31: System can show system initialization status (agents loading, MCP Server connecting)
- FR32: Users can observe agent messages via web UI in real-time (Growth)
- FR33: Web UI can display messages chronologically as they occur (Growth)
- FR34: Web UI can distinguish between GM narration, PC dialogue, and system events (Growth)
- FR35: Web UI can stream messages in real-time as agents generate them (Growth)

### Character System (Growth)

- FR36: System can store and manage character sheets for PC agents
- FR37: PC agents can access their own character sheet information
- FR38: Character sheets can contain stats, background, and personality traits
- FR39: Agents can play according to their character sheet information
- FR40: System can differentiate between agent personality and character personality

### Authentication & Authorization (Growth)

- FR41: System can enforce resource access control based on agent role
- FR42: All agents can access public resources (RPG rules)
- FR43: Only GM agent can access restricted resources (adventure plots, NPC stats)
- FR44: Each PC agent can access only their own character sheet
- FR45: System can prevent unauthorized resource access

### Session Persistence (Growth)

- FR46: System can save session state to chronicle
- FR47: System can load session state from chronicle
- FR48: System can track complete session history
- FR49: System can persist all relevant events from a session
- FR50: Users can view session history via web UI (Growth)
