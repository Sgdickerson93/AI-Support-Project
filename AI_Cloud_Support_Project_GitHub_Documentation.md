# AI-Powered Cloud Support & Incident Triage Platform

A hands-on cloud engineering project designed to simulate a real-world technical support and incident-triage workflow while building practical experience with Python, REST APIs, cloud tooling, AI, observability, containers, Kubernetes, infrastructure as code, and AWS.

> **Project status:** Early development — REST API and rule-based automated triage are implemented. AI/LLM, MCP, GraphQL, observability, Docker, Kubernetes, Terraform, and AWS integration are planned next.

---

## 1. Project Goal

The goal of this project is to build an **AI-powered cloud support platform** that can accept customer support cases, automatically triage incidents, connect to external tools, collect operational data, and eventually run as a containerized workload on AWS.

The project is intentionally being built incrementally.

The learning approach is:

> **Learn → Build → Break → Fix → Explain**

Rather than copying a large production application, each feature is being implemented in small pieces so the underlying Python, API, cloud, and troubleshooting concepts are understood.

---

# 2. What This Project Is Designed to Demonstrate

The long-term project is intended to demonstrate experience with:

- Python
- REST APIs
- FastAPI
- API validation
- HTTP status codes
- CLI tooling
- AWS CLI
- Cloud infrastructure
- Linux
- GraphQL
- MCP (Model Context Protocol)
- LLM/tool calling
- AI-assisted troubleshooting
- Multi-agent concepts
- Logging
- Metrics
- Prometheus
- Grafana
- Docker
- Kubernetes
- Terraform
- AWS
- Incident triage
- Customer support case lifecycle
- Troubleshooting and root-cause investigation
- Communicating technical problems clearly

---

# 3. Planned Architecture

The eventual system is designed around this flow:

```text
Customer
   |
   v
REST API
(FastAPI)
   |
   v
Support Case Management
   |
   v
AI / LLM Triage Agent
   |
   +--------------------+
   |                    |
   v                    v
MCP Tools           GraphQL
   |                    |
   +---------+----------+
             |
             v
      Cloud / Application
          Telemetry
             |
       +-----+-----+
       |           |
       v           v
     Logs       Metrics
       |           |
       +-----+-----+
             |
             v
      Prometheus / Grafana
             |
             v
      Docker / Kubernetes
             |
             v
           AWS
```

The architecture will evolve as additional components are implemented.

---

# 4. Current Technology Stack

## Implemented

- Python
- FastAPI
- Uvicorn
- Pydantic
- REST API
- OpenAPI / Swagger UI
- In-memory case storage
- Rule-based automated triage

## Planned

- LLM integration
- MCP
- GraphQL
- Prometheus
- Grafana
- Docker
- Kubernetes
- Terraform
- AWS
- AWS CLI
- Additional cloud/network troubleshooting tools

---

# 5. Current Project Structure

The project currently lives in:

```text
ai-support-project/
│
├── venv/
│
├── main.py
│
└── ...
```

The Python virtual environment is used to isolate project dependencies.

Currently installed core packages include:

```text
fastapi
uvicorn
```

---

# 6. Running the Application

Start the FastAPI development server with:

```bash
uvicorn main:app --reload
```

The API can then be tested through the automatically generated Swagger UI:

```text
/docs
```

FastAPI generates the API documentation from the application's routes and Pydantic models.

---

# 7. Current API

## Health Check

### Endpoint

```http
GET /health
```

### Purpose

Provides a simple health check for the application.

### Example response

```json
{
  "status": "healthy"
}
```

This will eventually become useful for container and Kubernetes health checks.

---

# 8. Support Cases

The application currently supports creating, viewing, listing, and updating support cases.

## Create a Case

```http
POST /cases
```

The request currently accepts:

```text
title
description
```

The project is moving toward having the application automatically determine:

```text
priority
category
```

instead of requiring the customer to determine them.

---

## List Cases

```http
GET /cases
```

Returns all currently stored support cases.

At this stage, cases are stored in memory using a Python list.

This means the data is temporary and will disappear when the application restarts.

That is intentional for the current learning stage.

---

## Get One Case

```http
GET /cases/{case_id}
```

Example:

```http
GET /cases/1
```

If the case exists, the API returns it.

If it does not exist, the API raises:

```python
HTTPException(status_code=404, detail="Case not found")
```

This demonstrates proper HTTP error handling.

---

## Update a Case

```http
PATCH /cases/{case_id}
```

The API currently supports these case statuses:

```text
new
triaged
investigating
resolved
```

Example request:

```json
{
  "status": "investigating"
}
```

The API validates the status before accepting it.

---

# 9. Pydantic Validation

The project uses Pydantic models through FastAPI.

For example:

```python
class CaseUpdate(BaseModel):
    status: Literal["new", "triaged", "investigating", "resolved"]
```

`Literal` restricts the value to a defined set of choices.

This prevents invalid values such as:

```json
{
  "status": "banana"
}
```

from being accepted.

FastAPI responds with a validation error:

```text
422 Unprocessable Entity
```

---

# 10. HTTP Status Codes Learned

Several important HTTP status codes have already been implemented and tested.

| Status | Meaning | Project Example |
|---|---|---|
| 200 | Successful request | GET case |
| 201 | Resource created | POST case |
| 404 | Resource not found | Case ID does not exist |
| 422 | Request validation failed | Invalid status value |

A key lesson:

Returning an error dictionary does not automatically create an HTTP error.

For example:

```python
return {"error": "Case not found"}
```

would still normally result in a successful HTTP response.

Instead:

```python
raise HTTPException(
    status_code=404,
    detail="Case not found"
)
```

actually tells the API client that the request resulted in a 404 error.

---

# 11. Current Support Case Data

A case currently contains information such as:

```json
{
  "id": 1,
  "title": "Application returning 500 errors",
  "description": "The application is returning 500 errors after deployment.",
  "status": "new",
  "priority": "high",
  "category": "application"
}
```

The exact data flow is being changed so that priority and category can eventually be determined automatically by the triage system.

---

# 12. Rule-Based Automated Triage

The first version of automated triage has been implemented.

Current function:

```python
def triage_case(description: str):
    description = description.lower()

    if "500" in description or "error" in description:
        return {
            "category": "application",
            "priority": "high"
        }

    return {
        "category": "application",
        "priority": "medium"
    }
```

This is intentionally simple.

It is **not AI yet**.

It is a rule-based classifier that demonstrates the fundamental data flow that the future AI system will replace or expand.

---

# 13. Why `.lower()` Is Used

The triage function normalizes the description:

```python
description = description.lower()
```

This prevents capitalization from affecting keyword matching.

Without normalization:

```text
error
Error
ERROR
```

would be treated as different strings.

After `.lower()`:

```text
error
error
error
```

all become comparable.

The important distinction learned here is:

```text
.lower()
    |
    v
Normalize the input

if
    |
    v
Make the decision
```

So `.lower()` does not perform the classification. It prepares the input so the classification rules work consistently.

---

# 14. Current Triage Logic

The current logic is:

```text
Support case description
        |
        v
Convert description to lowercase
        |
        v
Look for "500" or "error"
        |
        +---- YES ----> Application / High
        |
        +---- NO -----> Application / Medium
```

This is intentionally a small first milestone.

Future versions will be able to identify additional categories such as:

```text
application
deployment
network
authentication
database
```

and eventually use an LLM to reason over the case rather than relying only on keyword rules.

---

# 15. Important Debugging Lessons

This project has already produced several real debugging experiences.

## TabError

A Python indentation problem occurred:

```text
TabError: inconsistent use of tabs and spaces in indentation
```

The issue was fixed in VS Code by correcting the indentation.

### Lesson

Python uses indentation as part of its syntax.

Whitespace is therefore not merely cosmetic.

---

## Data Flow Bug

The Pydantic model was updated to include:

```text
priority
category
```

but the values did not appear in the returned case data.

The issue was that the model accepted the values, but the values were not being copied into the `new_case` dictionary.

The fix was to explicitly map them:

```python
"priority": case.priority,
"category": case.category
```

### Lesson

Changing the input model does not automatically change the application's internal data structure.

Data has to flow through each stage:

```text
Request
   ↓
Pydantic model
   ↓
Python logic
   ↓
Dictionary / object
   ↓
Response
```

This was an important debugging milestone because the problem was identified by following the data flow rather than simply changing random code.

---

# 16. Python Concepts Learned So Far

The project is also being used as a practical Python learning environment.

Concepts covered include:

## Functions

A function is a reusable block of logic.

Example:

```python
def triage_case(description: str):
```

Conceptually:

```text
Input
  ↓
Function
  ↓
Processing
  ↓
Output
```

The function accepts a description and returns structured triage information.

---

## Type Hints

Example:

```python
description: str
```

This communicates that `description` is expected to be a string.

---

## Lists

Cases are currently stored in an in-memory Python list:

```python
cases = []
```

New cases are added with:

```python
cases.append(new_case)
```

---

## Dictionaries

Individual cases are represented using dictionaries:

```python
{
    "id": 1,
    "title": "...",
    "description": "...",
    "status": "new"
}
```

Dictionary keys provide named access to the data.

---

## Conditional Logic

The triage function uses:

```python
if
```

to make a decision based on the input.

Example:

```python
if "500" in description or "error" in description:
```

---

## Exceptions

The API uses:

```python
raise
```

to signal an error condition.

Example:

```python
raise HTTPException(...)
```

A useful distinction learned:

```text
return
    = give the caller a result

raise
    = stop normal execution because something went wrong
```

---

# 17. Swagger / OpenAPI

FastAPI automatically generates API documentation.

Swagger UI is available at:

```text
/docs
```

This allows endpoints to be tested interactively.

OpenAPI is the API specification describing things such as:

- available endpoints
- HTTP methods
- request bodies
- response structures
- validation rules

Swagger UI provides an interactive interface for working with that specification.

---

# 18. Testing So Far

The API has been tested through Swagger UI.

Testing has included:

- Health endpoint
- Creating support cases
- Retrieving individual cases
- Listing cases
- Updating case status
- Testing invalid status values
- Testing missing case IDs
- Testing priority
- Testing category
- Testing rule-based triage

The project is being tested feature-by-feature rather than waiting until the entire application is complete.

---

# 19. Why the Current Triage Is Important

The current rule-based triage may look simple, but it establishes an important architecture pattern.

Today:

```text
Customer description
        ↓
Python rules
        ↓
Structured triage result
```

Eventually:

```text
Customer description
        ↓
LLM
        ↓
Structured triage result
```

The API does not need to fundamentally change just because the decision-making engine becomes more sophisticated.

This is one of the reasons the project is being built incrementally.

---

# 20. Planned AI / LLM Layer

The next major evolution is to introduce an LLM.

The future system will allow the model to analyze a support case and produce structured information such as:

```text
Category
Priority
Reason
Suggested next action
Potential affected system
Confidence
```

For example:

```json
{
  "category": "deployment",
  "priority": "high",
  "reason": "The application began returning errors immediately after a deployment.",
  "suggested_action": "Inspect deployment logs and application health."
}
```

The rule-based classifier provides a simple foundation for this future behavior.

---

# 21. MCP / Tool Calling Direction

A major future component is MCP (Model Context Protocol).

The goal is to allow the AI system to interact with tools instead of simply producing text.

Conceptually:

```text
User reports incident
        ↓
LLM analyzes issue
        ↓
LLM decides a tool is needed
        ↓
MCP tool
        ↓
External system / API / cloud resource
        ↓
Tool result
        ↓
LLM analyzes result
        ↓
Support response
```

Example future workflow:

```text
"Application is returning 500 errors."

        ↓

AI identifies likely application issue

        ↓

AI requests application health information

        ↓

MCP tool calls an external system

        ↓

Tool returns health/log information

        ↓

AI analyzes evidence

        ↓

AI provides troubleshooting result
```

This is where the project begins moving from a normal REST API into an AI-enabled support system.

---

# 22. GraphQL Direction

GraphQL will eventually be incorporated to demonstrate working with another API paradigm in addition to REST.

The project will use REST for the primary support API and GraphQL as another interface for retrieving or interacting with structured operational data.

This provides practical experience troubleshooting:

```text
REST
GraphQL
MCP/tool calls
```

rather than treating APIs as only theoretical concepts.

---

# 23. Observability Direction

The project will eventually include:

- Logging
- Metrics
- Health checks
- Prometheus
- Grafana

The goal is to make the system observable.

Instead of only asking:

> "Is the application working?"

the system should eventually help answer:

> "What is happening inside the application?"

Examples of future metrics:

```text
Number of support cases
Cases by priority
Cases by category
API request count
API error count
Response latency
Triage failures
Tool-call failures
```

Prometheus will collect metrics and Grafana will provide dashboards for visualizing them.

---

# 24. Containerization Direction

After the application is working locally, it will be packaged into a Docker container.

Conceptually:

```text
Python application
       ↓
Docker image
       ↓
Container
```

This will introduce practical experience with:

- Dockerfiles
- Images
- Containers
- Ports
- Environment variables
- Container networking
- Application health checks

---

# 25. Kubernetes Direction

The Dockerized application will eventually be deployed to Kubernetes.

The Kubernetes stage will introduce concepts such as:

- Pods
- Deployments
- Services
- ConfigMaps
- Secrets
- Health probes
- Scaling
- Networking

The `/health` endpoint already provides a natural starting point for future Kubernetes health checks.

---

# 26. Terraform Direction

Terraform will eventually be used to define infrastructure as code.

The intended progression is:

```text
Manual infrastructure
        ↓
Terraform configuration
        ↓
Repeatable infrastructure
```

Terraform will eventually manage cloud resources required by the project.

---

# 27. AWS Direction

The final cloud-oriented stages will move the project toward AWS.

Potential AWS components include:

- Compute
- Networking
- Storage
- IAM
- Cloud monitoring
- Container infrastructure

The exact AWS architecture will be introduced incrementally rather than creating the entire environment at once.

The project is intentionally designed to demonstrate understanding of how an application moves from:

```text
Local development
        ↓
Container
        ↓
Kubernetes
        ↓
Infrastructure as Code
        ↓
AWS
```

---

# 28. Customer Support Lifecycle

The project is also designed around a realistic support lifecycle:

```text
Customer Inquiry
      ↓
Case Created
      ↓
Triage
      ↓
Investigation
      ↓
Bug / Issue Identification
      ↓
Resolution
      ↓
Customer Communication
```

Current implementation covers the early stages:

```text
Case Created
      ↓
Automated Triage
      ↓
Status Tracking
```

Future versions will expand the workflow toward investigation, bug reporting, resolution, and AI-assisted customer communication.

---

# 29. Why This Is a Cloud Engineering Project

Although the project begins as a Python API, the goal is much broader.

It demonstrates the lifecycle of a cloud-native application:

```text
Application development
        ↓
API design
        ↓
Validation
        ↓
Troubleshooting
        ↓
AI integration
        ↓
External tool integration
        ↓
Observability
        ↓
Containerization
        ↓
Orchestration
        ↓
Infrastructure as Code
        ↓
Cloud deployment
```

This gives the project a natural progression from application engineering into cloud engineering, DevOps, and eventually SRE concepts.

---

# 30. Development Philosophy

The project is deliberately not being built as a giant copy-and-paste application.

Each feature is introduced only after understanding the previous layer.

The working philosophy is:

### 1. Learn

Understand the concept.

### 2. Build

Implement a small feature.

### 3. Break

Test invalid inputs and unexpected situations.

### 4. Fix

Diagnose the problem and correct it.

### 5. Explain

Be able to explain what happened and why the solution works.

This approach is especially important for cloud engineering because troubleshooting and understanding system behavior are more valuable than simply being able to reproduce a tutorial.

---

# 31. Current Milestone

## Milestone 1 — REST Support API

### Completed

- [x] Python project created
- [x] Virtual environment created
- [x] FastAPI installed
- [x] Uvicorn installed
- [x] FastAPI application created
- [x] `/health` endpoint
- [x] `POST /cases`
- [x] `GET /cases`
- [x] `GET /cases/{case_id}`
- [x] `PATCH /cases/{case_id}`
- [x] Case status validation
- [x] Priority field
- [x] Category field
- [x] 404 handling
- [x] 422 validation behavior
- [x] Swagger UI testing
- [x] Rule-based triage function
- [x] Basic automated triage logic

---

# 32. Next Milestone

## Milestone 2 — Automated Triage

The immediate next step is to connect:

```python
triage_case()
```

to:

```text
POST /cases
```

The intended flow is:

```text
POST /cases
      ↓
Create case
      ↓
Run triage_case()
      ↓
Determine priority/category
      ↓
Store triage result
      ↓
Return case
```

After that, the input model can be simplified so the customer does not have to manually provide priority and category.

---

# 33. Future Milestones

## Milestone 3 — Better Rule-Based Triage

Expand classification to recognize:

- Application issues
- Deployment issues
- Network issues
- Authentication issues
- Database issues

Add more meaningful priority logic.

Potentially add:

- Triage reason
- Suggested next step
- Triage source

---

## Milestone 4 — LLM Integration

Replace or augment keyword rules with an LLM.

Focus:

- Prompting
- Structured outputs
- Classification
- Reasoning over support cases
- AI-assisted troubleshooting

---

## Milestone 5 — Tool Calling / MCP

Allow the AI to interact with external tools.

Focus:

- MCP concepts
- Tool schemas
- Tool execution
- Tool results
- AI → tool → result → AI workflows

---

## Milestone 6 — GraphQL

Introduce GraphQL and practice troubleshooting both REST and GraphQL APIs.

---

## Milestone 7 — Observability

Add:

- Structured logging
- Metrics
- Prometheus
- Grafana
- Health checks

---

## Milestone 8 — Docker

Containerize the application.

---

## Milestone 9 — Kubernetes

Deploy the containerized application to Kubernetes.

---

## Milestone 10 — Terraform

Define infrastructure as code.

---

## Milestone 11 — AWS

Move the application toward AWS infrastructure and demonstrate cloud-native deployment.

---

# 34. Current Learning Outcomes

So far, this project has provided hands-on practice with:

- Building a REST API
- Defining API endpoints
- HTTP methods
- HTTP status codes
- Request validation
- Pydantic models
- Python functions
- Python dictionaries
- Python lists
- Conditional logic
- String normalization
- Exception handling
- Debugging Python indentation
- Debugging data flow
- Testing APIs through Swagger
- Thinking about API architecture
- Separating input validation from application logic
- Designing toward automated incident triage

---

# 35. Portfolio Value

The eventual project is intended to tell a clear engineering story:

> I built a cloud support platform that accepts customer incidents through a REST API, automatically triages them, uses AI and external tools to investigate issues, exposes operational telemetry, and can ultimately be containerized, orchestrated, provisioned with Terraform, and deployed on AWS.

The important part is that each layer will be backed by actual implementation and troubleshooting experience rather than simply listing technologies on a resume.

---

# 36. Current Project Status Summary

```text
REST API                 ██████████  Complete
Case Management          ██████████  Complete
Validation               ██████████  Complete
Rule-Based Triage        ████████░░  In progress
LLM Integration          ░░░░░░░░░░  Planned
MCP / Tool Calling       ░░░░░░░░░░  Planned
GraphQL                  ░░░░░░░░░░  Planned
Observability             ░░░░░░░░░░  Planned
Docker                   ░░░░░░░░░░  Planned
Kubernetes               ░░░░░░░░░░  Planned
Terraform                ░░░░░░░░░░  Planned
AWS                      ░░░░░░░░░░  Planned
```

---

# 37. Important Note

This project is intentionally being built incrementally.

The technologies listed in the roadmap are **planned components**, not claims that they have already been implemented.

The repository should be updated as each milestone is actually completed.

That makes the project a genuine record of hands-on learning, engineering decisions, debugging, and growth.
