# Unified Operations Workspace

## Product Vision & Project Roadmap

> **Working concept:** One workspace. Multiple systems.

## 1. The New Direction

The original AI Support Project is evolving into a **Unified Operations
Workspace**: a cloud-native integration, visibility, and analytics layer
for organizations that use multiple ticketing/work-management systems or
multiple instances of the same platform.

The initial goal is **not to replace ServiceNow, Jira, Zendesk, or other
systems of record**.

Instead:

``` text
ServiceNow Instance A ─┐
ServiceNow Instance B ─┤
ServiceNow Instance C ─┤
Jira ──────────────────┼──> Connector Layer
Zendesk ────────────────┘          |
                                   v
                            Normalization Layer
                                   |
                                   v
                            Unified Work Items
                                   |
                     +-------------+-------------+
                     |             |             |
                     v             v             v
                 Workspace      Analytics     Power BI
                     |
                     v
              Original Source Ticket
```

### North Star

> **One workspace. Multiple systems.**

A user should eventually be able to log into one application and see
authorized work from every connected system, search across those
systems, understand which platform/instance owns each ticket, and
navigate directly to the original ticket.

------------------------------------------------------------------------

## 2. Why This Pivot

The idea comes from a real support-work problem: teams may have to work
across several ticketing environments.

Example:

``` text
Corporate ServiceNow
Client ServiceNow
Infrastructure ServiceNow
Jira
```

This creates two major forms of fragmentation.

### Work fragmentation

An engineer may need to open multiple sites just to determine:

-   What is assigned to me?
-   Where is a particular incident?
-   Which system contains the ticket?
-   What high-priority work is open?
-   What is aging?

### Data fragmentation

Operational metrics may have to be manually pulled from multiple systems
and reconciled before they can be reported.

Examples:

-   Ticket volume
-   Open backlog
-   Tickets created vs. resolved
-   MTTA
-   MTTR
-   SLA compliance
-   Priority distribution
-   Category trends
-   Team workload
-   Aging tickets

### Product Hypothesis

> Organizations using multiple ticketing systems or instances need a
> simpler way to unify operational visibility, normalize their data, and
> expose it to analytics tools without replacing their existing systems.

This is a hypothesis to validate as the project grows.

------------------------------------------------------------------------

## 3. Example User Experience

``` text
MY OPERATIONS WORKSPACE

ALL | SNOW-CORP | SNOW-CLIENT | SNOW-INFRA | JIRA

Search all systems...

INC001284   P2   Database connection issue    SNOW-CORP
INC008921   P3   Login failure                SNOW-CLIENT
TASK-332    P2   Deployment issue             JIRA
INC003991   P1   API unavailable              SNOW-INFRA
```

Potential filters:

-   Source system
-   Source instance
-   Assignee
-   Team
-   Priority
-   Status
-   Category
-   Ticket type
-   Date

### Direct source navigation

Our platform does not initially need to own the ticket.

A normalized work item can retain:

``` text
source_system
source_instance
external_id
external_url
```

Clicking the work item can take the user directly to the correct
original system and ticket.

### Unified search

Instead of asking:

> Which ServiceNow instance was that incident in?

A user could search once.

Example:

``` text
Search: checkout 500 error

SNOW-CORP
INC003991 — Checkout API returning 500

SNOW-CLIENT
INC008292 — Customers receiving checkout errors

JIRA
BUG-882 — Checkout API regression
```

------------------------------------------------------------------------

## 4. Core Product Principles

### One Workspace, Multiple Systems

Users should not need to know where a ticket lives before they can find
it.

### Preserve the System of Record

Existing platforms remain authoritative initially. The workspace
aggregates, normalizes, searches, analyzes, and routes users back to the
source.

### Anything Captured Should Become Measurable

Normalized and supported custom fields should become usable for:

-   Search
-   Filtering
-   Reporting
-   Dashboards
-   Analytics APIs
-   Power BI
-   Future AI analysis

### Enterprise Power Without Unnecessary Complexity

Configuration should be approachable even when the underlying system is
sophisticated.

### AI Enhances the Platform

AI should not be the entire product.

Future AI capabilities may include:

-   Ticket summarization
-   Triage
-   Duplicate detection
-   Related-ticket detection
-   Field-mapping suggestions
-   Incident correlation
-   Natural-language analytics
-   Troubleshooting recommendations
-   Approved tool execution through MCP

------------------------------------------------------------------------

## 5. Unified Data Model

Different systems may represent equivalent concepts differently.

``` text
ServiceNow             Jira
-----------            ----
Incident               Bug
Number                 Issue Key
Assignment Group       Team
Opened                  Created
Resolved                Done
Priority 1              Highest
```

The normalization layer converts source records into a common model.

### Initial Unified Work Item

``` text
id

source_system
source_instance
external_id
external_url

title
description

status
priority
category

assignee
team

created_at
updated_at
resolved_at
```

The schema will evolve as integrations are researched and implemented.

------------------------------------------------------------------------

## 6. Field Mapping

Equivalent information may have different names across platforms.

``` text
ServiceNow.Affected Application ─┐
Jira.Service ────────────────────┼──> Affected Service
Zendesk.Product ─────────────────┘
```

A future admin interface could make these mappings easy to configure.

Eventually AI could suggest mappings:

> These fields appear semantically equivalent. Map them to **Affected
> Service**?

An administrator would approve or reject the suggestion.

This connects to a broader product principle:

> **Anything normalized should automatically become measurable.**

------------------------------------------------------------------------

## 7. Analytics & Power BI

Analytics is a major part of the new direction.

Instead of manually pulling metrics from several systems:

``` text
SNOW A ─┐
SNOW B ─┤
SNOW C ─┼──> Unified Data Layer ──> Analytics
Jira ───┘                              |
                                       v
                                    Power BI
```

Possible cross-system metrics:

-   Total work volume
-   Open backlog
-   Created vs. resolved
-   MTTA
-   MTTR
-   SLA compliance
-   Priority distribution
-   Category trends
-   Workload by team
-   Volume by source
-   Aging work

### Metrics Should Lead to Investigation

Dashboards should not be dead ends.

A user should eventually be able to drill from:

``` text
Deployment incidents increased 18%
```

into the tickets, systems, teams, services, and time periods that
produced the metric.

------------------------------------------------------------------------

## 8. What We Have Already Built

**We are not restarting the project.**

The original AI Support Project provides the first backend foundation.

### Current FastAPI Endpoints

``` text
GET    /health
POST   /cases
GET    /cases
GET    /cases/{case_id}
PATCH  /cases/{case_id}
```

### Existing Concepts

-   FastAPI
-   Pydantic models
-   API validation
-   HTTP exceptions
-   In-memory case storage
-   Status management
-   Priority
-   Category
-   Python rule-based triage

### Existing Triage Engine

`triage_case()` currently evaluates title/description text and scores
categories such as:

``` text
application
deployment
network
authentication
database
```

The current classifier can evolve rather than disappear.

``` text
V1 — Python rules
V2 — Better scoring and structured reasoning
V3 — AI-assisted classification
V4 — LLM + approved MCP tools
```

Eventually the intelligence layer could analyze normalized tickets
arriving from external systems.

------------------------------------------------------------------------

## 9. Pivot Architecture

### Current

``` text
User
 |
 v
POST /cases
 |
 v
FastAPI
 |
 v
triage_case()
 |
 v
cases = []
```

### First Pivot

``` text
Mock Source A ─┐
               ├──> Python --> Normalize --> Unified Tickets
Mock Source B ─┘
```

### Long-Term Direction

``` text
ServiceNow A ─┐
ServiceNow B ─┤
ServiceNow C ─┼──> Connectors --> Normalization --> Unified Platform
Jira ─────────┤
Zendesk ──────┘
```

------------------------------------------------------------------------

## 10. First Prototype

The first prototype does **not** require access to real enterprise
systems.

### Goal

> Combine tickets from two mock sources into one normalized API.

``` text
Mock SNOW Corporate ─┐
                     ├──> Normalize --> GET /tickets
Mock SNOW Client ────┘
```

Each normalized ticket should eventually retain:

``` text
source_system
source_instance
external_id
external_url
```

This proves the basic product concept:

> **One workspace. Multiple systems.**

------------------------------------------------------------------------

## 11. First Real Integration Milestone

After the mock architecture works:

> Connect one real external ticketing API, retrieve authorized tickets,
> normalize them, and expose them through the unified application.

Then add another instance/source.

That proves the key technical thesis: multiple systems can be
represented through one workspace.

------------------------------------------------------------------------

## 12. Security Requirements

Security must be designed into the integration architecture.

Future requirements include:

-   OAuth where supported
-   Appropriate service accounts
-   Encrypted secrets
-   Least-privilege permissions
-   RBAC
-   Audit logging
-   Secure API communication
-   Tenant isolation
-   Credential rotation
-   Permission-aware search
-   No plaintext credentials

### Security Principle

> The unified workspace must not expose a work item to a user who is not
> authorized to access that data.

The exact authorization model will require careful design as real
integrations are introduced.

------------------------------------------------------------------------

## 13. Learning-First Development

This remains a learning project even if it later becomes a commercial
product.

The goal is to become capable of:

-   Building the code
-   Reading the code
-   Explaining the architecture
-   Troubleshooting failures
-   Making design decisions
-   Using AI coding assistance critically instead of depending on it

The product will therefore grow alongside the engineering curriculum.

------------------------------------------------------------------------

## 14. ADHD-Friendly Roadmap

We use three buckets so the full vision does not become today's
responsibility.

### NOW

**Python automation fundamentals**

Current concepts:

-   Dictionaries
-   Conditionals
-   `if` / `else`
-   Comparisons
-   `and` / `or`
-   Compound conditions

Immediate next lesson:

``` text
for loops
   |
   v
iterate through multiple tickets
   |
   v
multiple mock ticket sources
```

### NEXT

1.  Learn Python `for` loops.
2.  Create two mock ticket sources.
3.  Iterate through tickets from both.
4.  Normalize different source formats.
5.  Build a unified ticket list.
6.  Expose unified tickets through FastAPI.
7.  Add filtering/search.
8.  Add persistent storage.

### LATER

-   PostgreSQL
-   Real ServiceNow API connector
-   Multiple ServiceNow instances
-   Authentication
-   RBAC
-   Secure secrets
-   Jira connector
-   Zendesk connector
-   Power BI integration
-   Cross-system analytics
-   Custom field mapping
-   Admin configuration UI
-   Frontend workspace
-   AI triage
-   AI field mapping
-   Natural-language analytics
-   MCP/tool calling
-   GraphQL
-   Observability
-   Prometheus
-   Grafana
-   Docker
-   CI/CD
-   Kubernetes
-   Terraform
-   AWS

**Nothing in LATER is today's responsibility.**

------------------------------------------------------------------------

## 15. Commercial Validation Questions

The project may have commercial potential, but market validation should
happen alongside development.

Questions to investigate:

-   How common is multi-instance ticket fragmentation?
-   How often do support teams work across several systems?
-   How do teams currently create cross-system dashboards?
-   How much reporting is manual?
-   How are equivalent fields reconciled?
-   What tools are companies already paying for?
-   Which roles experience the most pain?
-   Would organizations prefer an integration layer over replacing
    existing systems?
-   Which connector would create the most value first?
-   What security/compliance requirements would affect adoption?

Potential users to learn from:

-   Support engineers
-   Support managers
-   IT operations teams
-   Service desk teams
-   SRE teams
-   Platform teams
-   ITSM administrators
-   Operations leadership

------------------------------------------------------------------------

## 16. Product Hypotheses

### H1 --- Unified Visibility

Teams operating across multiple ticketing systems or instances
experience meaningful friction because work is fragmented across
interfaces.

**Status:** To validate

### H2 --- Cross-System Analytics

Operational reporting becomes difficult when metrics must be manually
collected and reconciled across systems.

**Status:** To validate

### H3 --- Integration Before Replacement

Organizations may prefer a visibility/integration layer that works with
their existing systems rather than immediately replacing established
systems of record.

**Status:** To validate

### H4 --- Unified Search

Users benefit from searching all authorized systems without first
knowing which system contains the work item.

**Status:** To validate

### H5 --- Easier Field Normalization

Organizations need a simpler way to map equivalent operational fields
across heterogeneous systems.

**Status:** To validate

------------------------------------------------------------------------

## 17. Parking Lot

Ideas live here until they become an active milestone.

-   Personal unified work queue
-   Team unified queue
-   Global cross-system search
-   Saved searches
-   Custom dashboards
-   Executive dashboards
-   Power BI datasets
-   Cross-system SLA analytics
-   AI ticket summaries
-   Duplicate-ticket detection
-   Related-ticket detection
-   Incident/deployment correlation
-   Service-health correlation
-   AI-assisted field mapping
-   Natural-language queries
-   Suggested troubleshooting
-   Alerting
-   Webhooks
-   Slack/Teams notifications
-   GraphQL analytics layer
-   MCP integrations
-   Connector health monitoring
-   Drag-and-drop field mapping
-   Visual connector setup
-   Custom field analytics
-   Gradual native ticket/workflow capabilities

------------------------------------------------------------------------

## 18. Technology Direction

The project is expected to progressively expose the developer to:

``` text
Python
FastAPI
REST APIs
JSON
External API integrations
Authentication / OAuth
PostgreSQL
Data normalization
Linux
Networking
Power BI
AWS
Docker
CI/CD
Kubernetes
Terraform
Observability
Prometheus / Grafana
AI / LLM integration
MCP / tool calling
```

Technologies should be introduced because the architecture requires
them---not simply to add technologies to the project.

------------------------------------------------------------------------

## 19. Next Coding Session

We continue exactly where the Python curriculum stopped.

### Next Concept: `for` Loops

Instead of arbitrary loop exercises, the lesson begins the product
pivot.

``` text
Mock Source A tickets
        +
Mock Source B tickets
        |
        v
Python iterates through them
        |
        v
First step toward a unified queue
```

The long-term vision changed.

**The next coding lesson did not.**

------------------------------------------------------------------------

## 20. Working Mission Statement

> Build a secure, cloud-native operations workspace that unifies
> authorized work across multiple ticketing and work-management systems,
> preserves existing systems of record, simplifies navigation and
> analytics, and progressively uses automation and AI to reduce
> operational friction.

### Working tagline

> **One workspace. Multiple systems.**
