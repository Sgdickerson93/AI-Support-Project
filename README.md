# Unified Operations Workspace

> **One workspace. Multiple systems.**

## 👋 What is this?

The **Unified Operations Workspace** is a learning-first cloud engineering project exploring a problem I have encountered while working in technical support:

**What happens when your work is spread across multiple ticketing systems—or even multiple instances of the same system?**

Instead of opening several ServiceNow instances, Jira projects, or other support platforms to find and manage work, this project explores creating **one workspace that brings those systems together**.

The goal is not necessarily to replace the systems companies already use.

The goal is to make them easier to work across.

```text
ServiceNow A ─┐
ServiceNow B ─┤
ServiceNow C ─┼──► Unified Operations Workspace
Jira ─────────┤              │
Zendesk ──────┘              │
                             ├──► Unified ticket view
                             ├──► Search & filtering
                             ├──► Analytics
                             └──► Power BI
```

---

## 💡 The Idea

Imagine your organization uses three different ServiceNow instances.

Today, you may have to visit each one separately to find tickets, check assignments, or gather metrics.

The goal of this project is to eventually allow a user to log into **one application** and see authorized work across all connected systems.

A ticket could look something like:

```text
INC003991
Priority: P1
Status: Investigating
Source: Infrastructure ServiceNow

API unavailable in production
```

The workspace knows where that ticket came from.

Clicking it can take the user directly to the original ticket in the correct system.

The source platform remains the **system of record** while the Unified Operations Workspace becomes the layer that helps users find, understand, and analyze work across systems.

---

## 🔎 What Could This Eventually Do?

### Unified Work Queue

See tickets from multiple systems in one place.

```text
INC001284   P2   Database connection issue    SNOW-CORP
INC008921   P3   Login failure                SNOW-CLIENT
TASK-332    P2   Deployment issue             JIRA
INC003991   P1   API unavailable              SNOW-INFRA
```

### Cross-System Search

Search once instead of figuring out which system contains the ticket.

```text
Search: checkout 500 error
```

Results could include matching work from ServiceNow, Jira, and other connected platforms.

### Direct Source Navigation

Every normalized ticket retains information about its source:

```text
source_system
source_instance
external_id
external_url
```

This allows the workspace to route users back to the original ticket.

### Unified Analytics

Normalize data from multiple systems so operational metrics can be analyzed together.

Examples include:

- Ticket volume
- Open backlog
- MTTA
- MTTR
- SLA compliance
- Priority trends
- Aging tickets
- Category trends
- Team workload

### Power BI Integration

A future goal is to expose normalized operational data to Power BI so organizations can build dashboards without manually combining reports from multiple ticketing systems.

---

## 🧠 What About AI?

This project originally began as an **AI Support Project**, so AI is still part of the long-term vision.

However, AI is not intended to be the entire product.

The platform should be useful without it.

Future AI capabilities could include:

- Ticket triage
- Ticket summaries
- Related-ticket detection
- Duplicate detection
- Suggested field mappings
- Incident correlation
- Natural-language analytics
- Troubleshooting assistance
- MCP/tool integrations

The current project already contains a small Python rule-based triage engine.

That gives the intelligence layer a progression:

```text
Python Rules
     ↓
Improved Scoring
     ↓
AI-Assisted Classification
     ↓
LLM + MCP Tools
```

---

## 🛠️ What Has Been Built So Far?

The project currently has a FastAPI backend with basic support-case management.

### Current API

```text
GET    /health
POST   /cases
GET    /cases
GET    /cases/{case_id}
PATCH  /cases/{case_id}
```

Current functionality includes:

- FastAPI REST API
- Pydantic validation
- Case creation
- Case retrieval
- Status updates
- HTTP error handling
- Priority assignment
- Category assignment
- Python-based ticket triage

The project currently uses in-memory storage while the backend fundamentals are being developed.

---

## 🚧 What Am I Building Next?

The next milestone begins the transition from a single support application into the unified workspace concept.

### Step 1 — Multiple Mock Sources

Create two simulated ticket systems.

```text
Mock ServiceNow Corporate ─┐
                           ├──► Python
Mock ServiceNow Client ────┘
```

### Step 2 — Normalize Their Tickets

Different systems may represent the same information differently.

The application will transform them into a shared ticket model.

```text
Source A ─┐
          ├──► Normalization ──► Unified Ticket
Source B ─┘
```

### Step 3 — Unified API

Expose the normalized tickets through FastAPI.

Potential future endpoints:

```text
GET /tickets
GET /tickets/{id}
GET /tickets?source=servicenow
GET /tickets?priority=high
```

### Step 4 — Search & Filtering

Allow users to find work across connected sources.

### Step 5 — Persistence

Move from in-memory storage to a database such as PostgreSQL.

### Step 6 — First Real Integration

Eventually connect to a real external ticketing API and normalize authorized tickets.

---

## 🗺️ Long-Term Roadmap

Future areas of exploration include:

```text
Multiple ServiceNow Instances
        ↓
Unified Ticket Model
        ↓
Search & Filtering
        ↓
PostgreSQL
        ↓
Real API Connectors
        ↓
Authentication / RBAC
        ↓
Unified Dashboard
        ↓
Cross-System Analytics
        ↓
Power BI
        ↓
AI / MCP
        ↓
Docker
        ↓
CI/CD
        ↓
Kubernetes
        ↓
Terraform
        ↓
AWS
        ↓
Observability
```

This roadmap will evolve as the project and product hypotheses are tested.

---

## 🔐 Security Matters

Connecting enterprise systems introduces important security requirements.

Future versions will need to consider:

- OAuth
- Service accounts
- Least-privilege access
- Encrypted secrets
- Role-based access control
- Audit logging
- Tenant isolation
- Permission-aware search
- Secure API communication
- Credential rotation

A core principle is:

> **The unified workspace should never expose a ticket to someone who is not authorized to access that data.**

---

## 🧪 This Is Also a Learning Project

This project is intentionally being built incrementally.

I'm using it to deepen my understanding of:

- Python
- FastAPI
- REST APIs
- Linux
- Networking
- API integrations
- Databases
- Authentication
- AWS
- Docker
- Kubernetes
- Terraform
- CI/CD
- Observability
- Power BI
- AI/LLMs
- MCP and tool calling

Rather than adding technologies simply to make the stack larger, the goal is to introduce them as the architecture creates a real reason to use them.

---

## 🌱 Current Status

**Early Development / Learning Phase**

Current focus:

```text
Python Fundamentals
        ↓
for Loops
        ↓
Multiple Mock Ticket Sources
        ↓
Ticket Normalization
        ↓
Unified Ticket API
```

There is a much larger vision for this project, but I'm building it **one working layer at a time**.

---

## 🎯 Project Mission

> Build a secure, cloud-native operations workspace that unifies authorized work across multiple ticketing and work-management systems, preserves existing systems of record, simplifies navigation and analytics, and progressively uses automation and AI to reduce operational friction.

---

**One workspace. Multiple systems. 🚀**
