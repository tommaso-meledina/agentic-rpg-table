---
stepsCompleted: [1, 2, 3]
inputDocuments: ['BRIEF.md', 'bmad/planning-artifacts/prd.md']
workflowType: 'architecture'
project_name: 'agentic-rpg-table'
user_name: 'Tom'
date: '2026-01-22'
---

# Architecture Decision Document

_This document builds collaboratively through step-by-step discovery. Sections are appended as we work through each architectural decision together._

## Project Context Analysis

### Requirements Overview

**Functional Requirements:**

Fifty functional requirements organized into nine categories:

- **Agent Management (FR1-FR5):** Create and initialize GM and PC agents, configure from YAML/database, manage personalities (Growth), handle multiple PC agents simultaneously (Growth)
- **Game Session Management (FR6-FR10):** Start/stop sessions, initialize with specified agents, track state; Growth: resume from chronicle
- **Message Broadcasting & Communication (FR11-FR15):** GM and PC agents send messages to all agents, maintain order and chronology, format with identity and timestamps
- **Rule System & Dice Rolling (FR16-FR21):** All dice rolls and rulebook access via MCP Server, agents request rolls, GM rolls for NPCs, enforce rules, reference rulebook for decisions
- **Resource Management (MCP Server) (FR22-FR26):** Connect to MCP Server, access deterministic operations (dice), static resources (rulebooks, sourcebooks), restricted resources for GM (Growth), configure resource availability per session
- **Observation & Monitoring (FR27-FR35):** MVP: console/logs with formatting, agent identity, dice results, initialization status. Growth: web UI with chronological display, real-time streaming, distinguish GM/PC/system events
- **Character System (FR36-FR40, Growth):** Store/manage character sheets, PC agents access own sheets, sheets contain stats/background/personality, agents play according to sheets, differentiate agent vs character personality
- **Authentication & Authorization (FR41-FR45, Growth):** Enforce resource access control by role, all agents access public resources (rules), only GM accesses restricted resources (adventure plots, NPC stats), each PC accesses only own character sheet, prevent unauthorized access
- **Session Persistence (FR46-FR50, Growth):** Save/load session state to chronicle, track complete history, persist all relevant events, view history via web UI

**Non-Functional Requirements:**

- **Performance (NFR1-NFR4):** Fast message processing/display, minimal latency for real-time streaming (Growth), MCP dice operations without noticeable delay, performance optimization shouldn't warp architecture
- **Scalability (NFR5-NFR6):** Support scaling from 1 PC agent to 2-4 PC agents without architectural changes, stable multi-agent coordination as PC count increases
- **Accessibility (NFR7-NFR10, Growth):** WCAG AAA compliance for web UI, keyboard navigation, screen reader support
- **Reliability (NFR11-NFR14):** Graceful error handling, MCP Server connection failure recovery, Growth: resume from chronicle on system failure, reliable chronicle event persistence

**Scale & Complexity:**

- **Primary domain:** Backend / agent orchestration + MCP Server integration, with web UI as observational interface (Growth)
- **Complexity level:** Medium-high (multi-agent coordination, MCP integration, turn-taking mechanics, future auth/authz, chronicle persistence, real-time UI)
- **Estimated architectural components:** Orchestrator/session manager, GM agent, PC agent(s), MCP client, message bus/broadcast mechanism, observation layer (console + future HTTP/WebSocket), Growth: chronicle storage, auth layer, character storage, web SPA

### Technical Constraints & Dependencies

- **MCP Server dependency:** Required. Provides deterministic operations (dice rolling) and static/restricted resources (rulebooks, sourcebooks, adventure plots, NPC stats). Central to the architecture - defines how agents access rules and mechanics.
- **LLM-powered agents:** GM and PC agents are LLM-based, requiring tool-calling capabilities and resource access. Orchestration layer must manage agent lifecycle, turn-taking, and tool invocation.
- **MVP scope constraints:** 1 GM + 1 PC agent, MCP Server, console observation only. No auth, chronicle, web UI, multi-PC, or character sheets in MVP. Architecture must allow adding these without rewrite.
- **Backend-focused architecture:** PRD specifies "backend-focused architecture with web UI as observational interface" - core value is agent orchestration and MCP integration, UI is secondary for Growth phase.

### Cross-Cutting Concerns Identified

- **Agent-MCP contract:** How agents invoke MCP tools (dice, rulebook access) and access resources. In Growth, how resource access is scoped (public vs GM-only vs per-PC).
- **Message model and ordering:** Shared message schema for GM/PC/system messages, maintaining chronological order for logs and future real-time UI and chronicle.
- **Session lifecycle:** Start, run, stop sessions. In Growth, persist and resume from chronicle.
- **Observation pipeline:** Single pipeline for MVP (console/logs) that can be extended to streaming endpoint for Growth web UI.

## Starter Template Evaluation

### Primary Technology Domain

Full-stack TypeScript application:
- **Backend:** NestJS for agent orchestration, MCP client integration, and session management
- **Frontend:** Next.js (React) for web UI observation interface (Growth phase)
- **MCP Servers:** Python (FastMCP 3.0) with `uv` package manager - separate Python projects, not part of main starter templates

### Starter Options Considered

**Backend - NestJS:**
- Official NestJS CLI starter (`nest new`) provides TypeScript with strict mode, modular architecture, ESLint/Prettier, and Jest testing setup
- Well-maintained, follows NestJS best practices, production-ready defaults

**Frontend - Next.js:**
- Official Next.js CLI starter (`create-next-app`) provides TypeScript, App Router, Tailwind CSS, ESLint, and modern build tooling (Turbopack)
- Current best practices, excellent developer experience, optimized for production

**MCP Servers:**
- Python projects using FastMCP 3.0 framework with `uv` package manager - separate from main application starters
- Will be initialized as standalone Python projects with FastMCP 3.0 dependencies managed via `uv`

### Selected Starter: Official NestJS CLI + Official Next.js CLI

**Rationale for Selection:**

- **NestJS:** Official starter aligns with framework conventions, provides strict TypeScript, modular structure suitable for agent orchestration, and integrates well with MCP clients
- **Next.js:** Official starter uses App Router, TypeScript, Tailwind CSS, and modern tooling suitable for real-time observation UI
- Both starters are actively maintained, follow current best practices, and provide solid foundations without unnecessary complexity
- Experienced team can extend these starters with additional features (Prisma for database abstraction, WebSocket support, etc.) as needed
- **MCP Servers:** Python/FastMCP 3.0 with `uv` provides fast, modern Python dependency management and aligns with FastMCP 3.0 best practices

**Initialization Commands:**

**Backend:**
```bash
nest new agentic-rpg-backend --strict --package-manager pnpm
```

**Frontend (Growth phase):**
```bash
npx create-next-app@latest agentic-rpg-frontend --typescript --app --src-dir --tailwind --eslint
```

**MCP Servers (Python):**
```bash
# Separate Python project initialization using uv
# Will use FastMCP 3.0 framework for MCP server implementation
# uv will be used for Python package management and project setup
```

**Architectural Decisions Provided by Starter:**

**Language & Runtime:**
- TypeScript with strict mode (NestJS backend)
- TypeScript (Next.js frontend)
- Node.js runtime for both
- Python runtime for MCP servers (separate projects) with `uv` for package management

**Styling Solution:**
- Tailwind CSS (Next.js frontend)
- Backend: API-focused, no styling needed

**Build Tooling:**
- NestJS: TypeScript compiler, NestJS build pipeline
- Next.js: Turbopack (development), Webpack (production), optimized builds
- MCP Servers: `uv` for Python dependency management and project setup

**Testing Framework:**
- NestJS: Jest configured with test files
- Next.js: Testing setup to be added separately (Vitest/Jest recommended)

**Code Organization:**
- NestJS: Modular architecture (`src/`, `test/`), dependency injection, controllers/services/DTOs pattern
- Next.js: App Router with `src/app/` routing structure, component-based organization
- MCP Servers: Python project structure following FastMCP 3.0 conventions

**Development Experience:**
- NestJS: Hot reload, TypeScript compilation, ESLint/Prettier, development server
- Next.js: Fast refresh, dev server, TypeScript checking, Turbopack for fast builds
- MCP Servers: `uv` provides fast dependency resolution and virtual environment management

**Note:** Project initialization using these commands should be the first implementation story.
