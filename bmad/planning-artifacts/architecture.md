---
stepsCompleted: [1, 2, 3, 4, 5]
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

## Core Architectural Decisions

### Decision Priority Analysis

**Critical Decisions (Block Implementation):**

1. **Database Abstraction:** Prisma 7.2.0 with PostgreSQL
   - Type-safe queries, migration system, excellent developer experience
   - PostgreSQL chosen for reliability and feature set

2. **MCP Client Integration:** @modelcontextprotocol/sdk v1.17.5 with Streamable HTTP transport
   - Official TypeScript SDK for MCP protocol
   - Streamable HTTP transport for production-ready remote communication
   - No stdio transport - HTTP from the beginning

3. **Agent Orchestration:** Event-driven architecture with message bus and centralized session manager
   - Message bus for broadcasting (all agents subscribe)
   - Session manager for turn orchestration and state management
   - Agents are independent services that emit/receive events
   - Decoupled, scalable, clear separation of concerns

4. **Message Schema:** JSON format with TypeScript interfaces
   - Fields: `id`, `timestamp`, `agentId`, `agentType` (GM/PC), `agentName`, `agentMood`, `content`, `type` (narration/dialogue/system/dice-roll)
   - Simple JSON serialization for debugging and compatibility

5. **Real-time Communication:** Server-Sent Events (SSE) for Growth phase
   - One-way server-to-client streaming
   - Built-in reconnection, HTTP-based, simpler than WebSocket
   - Suitable for observation-only UI

6. **Chronicle Storage:** PostgreSQL tables with Prisma
   - Structured relational approach with `Session`, `Message`, `DiceRoll`, `Event` tables
   - Supports complex queries, relationships, and history retrieval
   - Simpler than full event sourcing for MVP

7. **Authentication & Authorization:** JWT-based with role claims for Growth phase
   - Stateless, works well with MCP Server architecture
   - Role claims (GM/PC) and agentId for per-PC character sheet access
   - MCP client includes JWT in Streamable HTTP requests

8. **Error Handling & Logging:** Hybrid approach
   - MVP: Formatted console logs with clear formatting
   - Growth: Structured logging with Pino (JSON format, correlation IDs, log levels)
   - Graceful error handling with try-catch, retry logic for MCP connections

9. **LLM Integration:** LangChain.js v0.3.33 with `@langchain/mcp-adapters`
   - Provider-agnostic framework for LLM interactions
   - MCP integration via `MultiServerMCPClient` for tool calling
   - Supports agent orchestration patterns
   - Note: v1.0 expected October 2025, plan migration accordingly

**Important Decisions (Shape Architecture):**

- **Naming Strategy:** 
  - TypeScript/Prisma: PascalCase for models/classes, camelCase for properties
  - Database: UPPER_SNAKE_CASE for tables, lower_snake_case for columns
  - Implementation: Use `@@map` and `@map` in Prisma schema to enforce mapping
  - Single point of enforcement through Prisma schema conventions

**Deferred Decisions (Post-MVP):**

- Specific LLM provider choice (OpenAI, Anthropic, etc.) - implementation decision, LangChain.js abstracts this
- Web UI component library - Growth phase decision
- Advanced monitoring/observability tools - Growth phase decision
- Character sheet storage structure details - Growth phase decision

### Data Architecture

**Database:** PostgreSQL with Prisma 7.2.0
- Type-safe queries and migrations
- Naming strategy: PascalCase/camelCase in code, UPPER_SNAKE_CASE/lower_snake_case in database
- Chronicle storage: Relational tables (`SESSIONS`, `MESSAGES`, `DICE_ROLLS`, `EVENTS`)

### API & Communication Patterns

**MCP Client:** @modelcontextprotocol/sdk v1.17.5
- Streamable HTTP transport (no stdio)
- Official TypeScript SDK for MCP protocol
- Integrated via NestJS service/module

**Real-time:** Server-Sent Events (SSE) for Growth phase
- One-way streaming from backend to frontend
- Built-in reconnection handling

**Message Format:** JSON with TypeScript interfaces
- Schema includes `agentName` and `agentMood` for future extensions

### Agent Orchestration

**Architecture:** Event-driven with message bus and session manager
- Message bus for broadcasting to all agents
- Centralized session manager for turn orchestration and state
- Agents as independent services emitting/receiving events

### LLM Integration

**Framework:** LangChain.js v0.3.33 with `@langchain/mcp-adapters`
- Provider-agnostic LLM interactions
- MCP tool calling via `MultiServerMCPClient`
- Supports agent orchestration patterns
- Migration to v1.0 (October 2025) to be planned

### Authentication & Security

**Approach:** JWT-based with role claims (Growth phase)
- Stateless authentication
- Role claims (GM/PC) and agentId for authorization
- MCP Server validates JWT and enforces resource access

### Infrastructure & Deployment

**Logging:** Hybrid approach
- MVP: Formatted console logs
- Growth: Structured logging with Pino (JSON format)

**Error Handling:** Graceful degradation
- Try-catch in agent orchestration
- MCP client retry logic with exponential backoff
- Session continues on non-critical errors

### Decision Impact Analysis

**Implementation Sequence:**

1. Initialize NestJS backend with Prisma setup
2. Set up MCP client service with @modelcontextprotocol/sdk
3. Implement message bus and session manager
4. Create agent services with LangChain.js integration
5. Implement message schema and event system
6. Add console logging for MVP
7. Growth: Add SSE endpoint, chronicle storage, JWT auth, structured logging

**Cross-Component Dependencies:**

- MCP client depends on MCP servers being available (Python/FastMCP 3.0)
- Agent services depend on LangChain.js and MCP client
- Session manager coordinates message bus and agent services
- Chronicle storage (Growth) depends on message schema and Prisma setup
- SSE endpoint (Growth) depends on message bus for real-time updates

## Implementation Patterns & Consistency Rules

### Pattern Categories Defined

**Critical Conflict Points Identified:**
5 major areas where AI agents could make different choices: naming conventions, project structure, API formats, communication patterns, and process patterns.

### Naming Patterns

**Database Naming Conventions:**
- Tables: UPPER_SNAKE_CASE (e.g., `SESSIONS`, `MESSAGES`, `DICE_ROLLS`)
- Columns: lower_snake_case (e.g., `agent_name`, `created_at`, `is_active`)
- Foreign keys: `{table}_id` format (e.g., `session_id`, `agent_id`)
- Indexes: `idx_{table}_{column}` format (e.g., `idx_sessions_agent_id`)
- Enforced via Prisma `@@map` and `@map` attributes

**API Naming Conventions:**
- REST endpoints: Always nouns
  - Plural for collections: `GET /sessions`, `PUT /sessions/{id}`, `DELETE /sessions/{id}`
  - Singular for singletons: `POST /start`, `GET /status`, `POST /stop`
- Route parameters: `:id` format (e.g., `/sessions/:id`)
- Query parameters: camelCase (e.g., `?agentId=123&limit=10`)
- Headers: kebab-case with `X-` prefix for custom headers (e.g., `X-Agent-Id`)

**Code Naming Conventions:**
- Components: PascalCase (e.g., `SessionCard`, `MessageList`)
- Files: kebab-case matching component name (e.g., `session-card.tsx`, `message-list.tsx`)
- Functions: camelCase (e.g., `getSessionById`, `createMessage`)
- Variables: camelCase (e.g., `agentId`, `sessionState`)
- Constants: UPPER_SNAKE_CASE (e.g., `MAX_AGENTS`, `DEFAULT_TIMEOUT`)
- Boolean properties: `is` prefix (e.g., `isLoading`, `isActive`, `isComplete`)

### Structure Patterns

**Project Organization:**
- Tests: Co-located with source files (e.g., `session.service.spec.ts` next to `session.service.ts`)
- Components: Organized by feature in `src/features/` (e.g., `src/features/sessions/`, `src/features/messages/`)
- Shared utilities: `src/common/` or `src/shared/`
- Services: NestJS modules in `src/modules/` or feature-based organization

**File Structure:**
- Config: `src/config/` directory with environment-specific files
- Static assets: `public/` for Next.js, `assets/` for NestJS
- Documentation: `docs/` at project root
- Environment files: `.env.example` at root, `.env.local` for local development

### Format Patterns

**API Response Formats:**
- Success: Direct data (no wrapper) - `{ id: "123", name: "Session 1" }`
- Error: Structured error object - `{ error: { code: "SESSION_NOT_FOUND", message: "Session not found" } }`
- Base response classes: Write once, reuse across codebase using hierarchy/polymorphism
  - Base success/error response classes with common structure
  - Extend for specific response types
  - Leverage TypeScript interfaces/classes for type safety
- Date format: ISO 8601 strings (e.g., `"2026-01-22T12:00:00Z"`)
- Pagination: `{ data: [...], pagination: { page, limit, total } }`
- Boolean properties: Always use `is` prefix (e.g., `isActive`, `isLoading`, `isComplete`)

**Data Exchange Formats:**
- JSON field naming: camelCase in API responses (matches TypeScript)
- Booleans: `true`/`false` (not `1`/`0`), with `is` prefix for property names
- Null handling: Use `null` (not `undefined` or empty strings)
- Arrays: Always arrays, never single-item objects

### Communication Patterns

**Event System:**
- Event naming: kebab-case with domain prefix (e.g., `session:created`, `message:broadcast`, `dice:rolled`)
- Event payload: `{ type: "session:created", data: {...}, timestamp: "..." }`
- Event versioning: Include version in event type if needed (e.g., `session:created:v1`)

**State Management:**
- State updates: Immutable updates (create new objects, don't mutate)
- Action naming: `verbNoun` pattern (e.g., `createSession`, `updateMessage`)
- State organization: Feature-based slices/modules
- Boolean state properties: Use `is` prefix (e.g., `isLoading`, `isError`)

### Process Patterns

**Error Handling:**
- Global error handler: NestJS exception filters for HTTP errors
- User-facing errors: Friendly messages, log technical details
- Logging vs user errors: Distinguish between logged errors and user-visible errors
- Error codes: Use consistent error code format (e.g., `SESSION_NOT_FOUND`, `MCP_CONNECTION_FAILED`)
- Base error classes: Write once, reuse via inheritance/polymorphism

**Loading States:**
- Loading state naming: `isLoading` or `loading` (boolean with `is` prefix preferred)
- Global vs local: Prefer local loading states per feature/component
- Loading UI: Consistent loading indicators/spinners

### Enforcement Guidelines

**All AI Agents MUST:**

- Follow the naming conventions defined above (database, API, code)
- Use the established project structure patterns
- Adhere to API response format standards with base response classes (hierarchy/polymorphism)
- Use consistent event naming and payload structures
- Implement error handling according to defined patterns with base error classes
- Follow the database naming strategy via Prisma mappings
- Use `is` prefix for all boolean properties
- **Adhere to the GitFlow process outlined in BRIEF.md:**
  - Pick an issue (ensure no unsatisfied dependencies)
  - Claim assignment
  - Create a new branch from the default branch, following the conventional commit convention + issue number (e.g., `feat/11`, `fix/43`, etc.)
  - Work on the new branch, executing commit and push at every step (always following the Conventional Commits spec)
  - When all acceptance criteria are met, open a PR against the default branch, using the PR template that is available

**Pattern Enforcement:**
- ESLint rules for code naming conventions
- Prisma schema validation for database naming
- API contract tests for response formats
- Code review checklist for pattern compliance
- Document pattern violations in PR reviews
- Git hooks configured via `.githooks` directory (run `git config core.hooksPath .githooks`)

### Pattern Examples

**Good Examples:**

```typescript
// Component naming
export const SessionCard: React.FC<Props> = () => { ... }

// File naming: session-card.tsx

// API endpoint: GET /sessions/:id (plural for collection)
// API endpoint: POST /start (singular for singleton)

// Boolean property with is prefix
interface Session {
  id: string;
  isActive: boolean;  // CORRECT
  isLoading: boolean; // CORRECT
}

// Base response class (write once, reuse)
abstract class BaseResponse {
  success: boolean;
  timestamp: string;
}

class SuccessResponse<T> extends BaseResponse {
  success = true;
  data: T;
}

class ErrorResponse extends BaseResponse {
  success = false;
  error: { code: string; message: string };
}

// Event naming: session:created
eventBus.emit('session:created', { sessionId, timestamp });

// Database table: SESSIONS (via @@map)
model Session {
  id String @id @map("id")
  agentName String @map("agent_name")
  isActive Boolean @map("is_active")
  @@map("SESSIONS")
}
```

**Anti-Patterns:**

```typescript
// ❌ Inconsistent naming
export const session_card = () => { ... }  // snake_case
export const SessionCard = () => { ... }   // PascalCase - CORRECT

// ❌ Boolean without is prefix
interface Session {
  active: boolean;  // WRONG
  loading: boolean; // WRONG
  isActive: boolean;  // CORRECT
  isLoading: boolean; // CORRECT
}

// ❌ Wrapped success responses (unless using base class hierarchy)
{ data: { session: {...} } }  // Unnecessary wrapper if not using base classes
{ id: "...", name: "..." }    // Direct response - CORRECT

// ❌ Mutating state
state.messages.push(newMessage);  // Mutation
state = { ...state, messages: [...state.messages, newMessage] };  // Immutable - CORRECT

// ❌ Inconsistent API naming
GET /session/:id  // Singular for collection - WRONG
GET /sessions/:id // Plural for collection - CORRECT
POST /start       // Singular for singleton - CORRECT
```
