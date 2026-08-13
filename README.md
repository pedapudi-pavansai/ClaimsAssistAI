# ClaimsAssist AI — Member & Claims Intelligence

ClaimsAssist AI is a multi-agent healthcare claims intelligence platform designed to help members understand their claims, coverage, costs, and next steps through a unified AI-powered experience.

Built for the **Humana Hackathon**, ClaimsAssist uses a hierarchical multi-agent architecture consisting of **5 Gemini 1.5 Pro agents**: one central **Orchestrator Agent** and four specialized **Subagents** that can analyze different parts of a member's request simultaneously.

Rather than sending an entire healthcare question through a single LLM workflow, ClaimsAssist decomposes complex requests into specialized tasks, executes them in parallel, and synthesizes the results into one member-friendly response.

---

## Multi-Agent Architecture

The core of ClaimsAssist is a **5-agent hierarchical system**.

Each agent is implemented as a specialized **Gemini 1.5 Pro wrapper** with its own role and context.

The **Orchestrator Agent** sits at the top of the hierarchy. It receives the member's request, determines what information is needed, delegates work across four specialized agents, and combines their outputs into a final response.

```text
                              MEMBER
                                |
                                | Question
                                v
                    +-------------------------+
                    |    ORCHESTRATOR AGENT   |
                    |     Gemini 1.5 Pro      |
                    |                         |
                    |  Understands Request    |
                    |  Routes Tasks           |
                    |  Synthesizes Results    |
                    +------------+------------+
                                 |
                     Parallel Agent Execution
                                 |
          +----------------------+----------------------+
          |                      |                      |
          v                      v                      v
 +----------------+     +----------------+     +----------------+
 |   SUBAGENT 1   |     |   SUBAGENT 2   |     |   SUBAGENT 3   |
 | Gemini 1.5 Pro |     | Gemini 1.5 Pro |     | Gemini 1.5 Pro |
 |                |     |                |     |                |
 |  Specialized   |     |  Specialized   |     |  Specialized   |
 |    Analysis    |     |    Analysis    |     |    Analysis    |
 +-------+--------+     +-------+--------+     +-------+--------+
         |                      |                      |
         |              +-------+--------+             |
         |              |   SUBAGENT 4   |             |
         |              | Gemini 1.5 Pro |             |
         |              |                |             |
         |              |  Specialized   |             |
         |              |    Analysis    |             |
         |              +-------+--------+             |
         |                      |                      |
         +----------------------+----------------------+
                                |
                                | Agent Outputs
                                v
                    +-------------------------+
                    |    ORCHESTRATOR AGENT   |
                    |                         |
                    |   Response Synthesis    |
                    +------------+------------+
                                 |
                                 v
                    +-------------------------+
                    |    UNIFIED RESPONSE     |
                    +-------------------------+
```

---

## How the Agent Hierarchy Works

### 1. Member Request

A member submits a question through the ClaimsAssist interface.

The request may require information from several healthcare domains rather than having a simple one-step answer.

### 2. Orchestrator Analysis

The **Orchestrator Agent**, powered by Gemini 1.5 Pro, acts as the central coordination layer.

It analyzes the member's request and determines which specialized agents should participate in answering it.

### 3. Task Decomposition

Instead of asking one model to solve the entire problem, the Orchestrator breaks the request into smaller specialized tasks.

```text
Complex Member Question
          |
          v
   Orchestrator
          |
     Decomposition
          |
    +-----+-----+-----+-----+
    |     |     |     |     |
    v     v     v     v
   A1    A2    A3    A4
```

### 4. Parallel Agent Execution

The four Gemini-powered subagents can work **simultaneously** on their assigned tasks.

Each subagent operates independently with specialized instructions and context, allowing ClaimsAssist to gather multiple perspectives without requiring every task to execute sequentially.

### 5. Response Synthesis

The results from the specialized agents are returned to the Orchestrator.

The Orchestrator evaluates the outputs, combines the relevant information, and generates a single coherent response for the member.

```text
Agent 1 ──┐
Agent 2 ──┤
Agent 3 ──┼──> Orchestrator ──> Unified Member Response
Agent 4 ──┘
```

---

## Why Multi-Agent?

Healthcare claims questions can span several areas at once.

A member may need to understand:

- What happened with a claim
- Why a particular decision occurred
- What their coverage means
- What costs they may be responsible for
- What actions they can take next
- Whether additional support is necessary

A traditional single-agent architecture requires one model to reason through all of these concerns.

ClaimsAssist instead uses **specialization + orchestration**.

```text
                SINGLE AGENT

Member ──> LLM ──> Entire Problem ──> Response


              CLAIMSASSIST AI

                       ┌──> Agent 1 ──┐
                       ├──> Agent 2 ──┤
Member ─> Orchestrator ├──> Agent 3 ──┼──> Orchestrator ─> Response
                       └──> Agent 4 ──┘
```

This architecture provides:

- **Parallel execution** across specialized tasks
- **Separation of responsibilities** between agents
- **Centralized orchestration** of complex requests
- **Modular agent design** for easier expansion
- **Independent agent prompting and context**
- **Unified response synthesis**
- **More explainable AI workflows**

---

## Agent Design

All five agents are powered by **Gemini 1.5 Pro**, but each wrapper serves a different purpose within the system.

```text
Gemini 1.5 Pro
      |
      +---- Orchestrator Agent
      |
      +---- Specialized Agent 1
      |
      +---- Specialized Agent 2
      |
      +---- Specialized Agent 3
      |
      +---- Specialized Agent 4
```

Using separate wrappers allows each agent to maintain its own:

- System instructions
- Responsibilities
- Context
- Input data
- Output structure

This allows the underlying model to remain consistent while giving each agent a distinct role within the overall workflow.

---

## Member Portal

ClaimsAssist includes a member-facing web application for accessing claims information and interacting with the AI system.

### Routes

| Route | Description |
|---|---|
| `/` | Member login |
| `/dashboard` | Member overview and claims intelligence |
| `/claims` | Claims information |
| `/assistant` | Multi-agent AI assistant |
| `/call` | Member support experience |

---

## Tech Stack

### Frontend

- React
- TypeScript
- Vite
- Tailwind CSS

### AI

- Gemini 1.5 Pro
- 5-agent hierarchical architecture
- 1 Orchestrator Agent
- 4 specialized Subagents
- Parallel agent execution
- Multi-agent response synthesis

### Data

- Local member CSV data
- Local claims data

---

## Running Locally

Clone the repository:

```bash
git clone <repository-url>
cd <repository-directory>
```

If Node.js is not installed globally, use the repository-local Node runtime:

```bash
export PATH="$PWD/.local/node/v24.17.0/node-v24.17.0-darwin-arm64/bin:$PATH"
```

Install dependencies:

```bash
npm install
```

Start the development server:

```bash
npm run dev
```

Vite will display the local development URL, typically:

```text
http://localhost:5173
```

---

## Demo Member

Use the following member information to explore the prototype:

```text
Member ID: MBR00036
First Name: Joshua
Last Name: Davis
Date of Birth: 1970-07-02
```

---

## Architecture Philosophy

ClaimsAssist is built around a simple principle:

> **Complex healthcare questions should be decomposed into specialized tasks rather than handled by one general-purpose agent.**

The four subagents provide specialized reasoning while the Orchestrator maintains control over the complete workflow.

This separation makes the system modular: individual agents can be modified, expanded, or replaced without redesigning the entire ClaimsAssist architecture.

---

## Future Development

The multi-agent architecture provides a foundation for future capabilities including:

- Live claims and member API integrations
- Persistent member context
- Retrieval-augmented generation over policy documents
- Additional specialized agents
- Dynamic agent routing
- Agent observability and tracing
- Human escalation workflows
- Real-time member support integrations
- Secure cloud deployment

---

## Disclaimer

ClaimsAssist AI is a prototype developed for hackathon and demonstration purposes. It is not intended to provide medical advice or replace professional healthcare guidance.