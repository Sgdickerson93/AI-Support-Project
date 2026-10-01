# ☁️ AI-Powered Cloud Support & Incident Triage Platform

## Project Progress Tracker

This document tracks the development of my AI-powered cloud support platform.

The goal of this project is not only to build a working application, but to strengthen my hands-on skills in:

**Python • FastAPI • REST APIs • Linux • AWS • Networking • Docker • Kubernetes • Terraform • CI/CD • Observability • AI/LLMs • MCP**

---

## 🟢 Phase 1 — Python & FastAPI Fundamentals

### API Foundation

- [x] Create Python virtual environment
- [x] Install FastAPI
- [x] Install and run Uvicorn
- [x] Create FastAPI application
- [x] Create `/health` endpoint
- [x] Test API through Swagger UI
- [x] Understand basic GET requests
- [x] Understand basic POST requests
- [x] Understand PATCH requests
- [x] Understand HTTP status codes

### Support Case API

- [x] Create `SupportCase` model
- [x] Create support cases with `POST /cases`
- [x] Automatically assign case IDs
- [x] Add case status
- [x] Retrieve support cases
- [x] Retrieve individual case by ID
- [x] Add `404 Not Found` handling
- [x] Update case status
- [x] Add status validation with `Literal`
- [x] Understand FastAPI `422 Unprocessable Entity` validation

---

## 🟡 Phase 2 — Automated Case Triage

### Rule-Based Triage Engine

- [x] Create `triage_case()` function
- [x] Analyze case descriptions
- [x] Include case titles in triage
- [x] Create category scoring system
- [x] Detect application issues
- [x] Detect deployment issues
- [x] Detect networking issues
- [x] Detect authentication issues
- [x] Detect database issues
- [x] Assign priority based on score
- [x] Use `max()` to determine highest scoring category
- [x] Identify tied categories
- [x] Begin handling multiple top categories
- [ ] Finalize tie-handling logic
- [ ] Improve keyword weighting
- [ ] Add more realistic support scenarios
- [ ] Refactor triage logic for readability
- [ ] Add automated tests for triage behavior

**Current Focus:** 🧠 Understanding Python fundamentals through the triage engine rather than simply copying code.

---

## ⚪ Phase 3 — Data Persistence

- [ ] Replace temporary in-memory `cases = []`
- [ ] Learn basic database concepts
- [ ] Add database
- [ ] Create support case table/model
- [ ] Save cases permanently
- [ ] Retrieve cases from database
- [ ] Update cases in database
- [ ] Learn basic SQL queries
- [ ] Add database error handling

---

## ⚪ Phase 4 — Linux Environment

- [ ] Run application on Linux
- [ ] Practice Linux navigation
- [ ] Practice file permissions
- [ ] Practice process management
- [ ] Practice networking commands
- [ ] Learn ports and listening services
- [ ] Troubleshoot application from Linux CLI
- [ ] Deploy project to Raspberry Pi lab
- [ ] Access API across local network

---

## ⚪ Phase 5 — Docker

- [ ] Learn containers vs virtual machines
- [ ] Create `Dockerfile`
- [ ] Build application image
- [ ] Run FastAPI inside container
- [ ] Understand port mapping
- [ ] Add environment variables
- [ ] Troubleshoot container logs
- [ ] Create Docker Compose configuration
- [ ] Containerize database

---

## ⚪ Phase 6 — AWS Deployment

- [ ] Design AWS architecture
- [ ] Configure IAM permissions
- [ ] Create networking environment
- [ ] Configure VPC
- [ ] Configure subnets
- [ ] Configure security groups
- [ ] Deploy compute resources
- [ ] Deploy application
- [ ] Configure storage/database
- [ ] Test application connectivity
- [ ] Troubleshoot AWS networking
- [ ] Review AWS costs

---

## ⚪ Phase 7 — Terraform / Infrastructure as Code

- [ ] Install Terraform
- [ ] Learn providers
- [ ] Learn resources
- [ ] Learn variables
- [ ] Learn outputs
- [ ] Create AWS infrastructure with Terraform
- [ ] Manage VPC through Terraform
- [ ] Manage security groups through Terraform
- [ ] Manage compute resources through Terraform
- [ ] Understand Terraform state
- [ ] Practice `terraform plan`
- [ ] Practice `terraform apply`
- [ ] Practice `terraform destroy`

---

## ⚪ Phase 8 — CI/CD

- [ ] Learn CI/CD fundamentals
- [ ] Configure GitHub Actions
- [ ] Automatically run tests
- [ ] Build application automatically
- [ ] Build Docker image automatically
- [ ] Add deployment pipeline
- [ ] Troubleshoot failed pipelines

---

## ⚪ Phase 9 — Kubernetes

- [ ] Learn Kubernetes architecture
- [ ] Learn Pods
- [ ] Learn Deployments
- [ ] Learn Services
- [ ] Learn ConfigMaps
- [ ] Learn Secrets
- [ ] Deploy application to Kubernetes
- [ ] Scale application
- [ ] Practice `kubectl`
- [ ] Inspect pod logs
- [ ] Troubleshoot failed deployments
- [ ] Troubleshoot networking issues
- [ ] Deploy to AWS Kubernetes environment

---

## ⚪ Phase 10 — Observability

- [ ] Add application logging
- [ ] Add structured logs
- [ ] Create application metrics
- [ ] Learn Prometheus
- [ ] Connect Prometheus
- [ ] Learn Grafana
- [ ] Build Grafana dashboard
- [ ] Monitor API health
- [ ] Monitor errors
- [ ] Monitor latency
- [ ] Practice troubleshooting using telemetry
- [ ] Explore incident-management workflows

---

## ⚪ Phase 11 — AI-Powered Support

- [ ] Connect application to an LLM
- [ ] Send support case information to AI
- [ ] Generate AI-assisted case summaries
- [ ] Generate troubleshooting recommendations
- [ ] Compare AI triage with rule-based triage
- [ ] Add safeguards around AI recommendations
- [ ] Track AI decisions and outputs
- [ ] Handle failed AI requests

---

## ⚪ Phase 12 — MCP & Tool Calling

- [ ] Learn Model Context Protocol fundamentals
- [ ] Understand MCP clients and servers
- [ ] Build MCP server
- [ ] Expose support tools through MCP
- [ ] Allow AI to retrieve support cases
- [ ] Allow AI to inspect application health
- [ ] Allow AI to query troubleshooting information
- [ ] Connect AI to cloud tooling safely
- [ ] Explore multi-tool workflows
- [ ] Build an AI-assisted incident investigation workflow

---

# 🧪 Troubleshooting Skills Tracker

As I build the project, I want to practice diagnosing problems instead of immediately looking for the answer.

- [ ] HTTP/API troubleshooting
- [ ] Python exceptions
- [ ] FastAPI validation errors
- [ ] Linux troubleshooting
- [ ] DNS troubleshooting
- [ ] TCP/IP troubleshooting
- [ ] AWS networking
- [ ] IAM permission errors
- [ ] Docker failures
- [ ] Kubernetes failures
- [ ] CI/CD pipeline failures
- [ ] Database connectivity
- [ ] Application logs
- [ ] Metrics and observability
- [ ] AI/tool-calling failures

---

# 🧠 Python Fundamentals Tracker

Because the goal is to **understand the code I am building**, not just make it work.

- [x] Variables
- [x] Functions
- [x] Function parameters
- [x] Return values
- [x] `if / else`
- [x] Dictionaries
- [x] Lists
- [x] Basic type hints
- [x] `Literal`
- [x] `max()`
- [ ] `for` loops
- [ ] List comprehensions
- [ ] Dictionary comprehensions
- [ ] Classes
- [ ] Exception handling
- [ ] Imports/modules
- [ ] Reading/writing files
- [ ] Environment variables
- [ ] Unit testing
- [ ] Debugging with breakpoints
- [ ] Async/await

---

# 🎯 Current Milestone

**Rule-Based Support Case Triage**

Current work:

```text
Support Case
      ↓
Title + Description
      ↓
Keyword Analysis
      ↓
Category Scores
      ↓
Highest Score
      ↓
Category + Priority
```

### Definition of Done

This milestone is complete when:

- [ ] Cases are categorized reliably
- [ ] Priority is calculated reliably
- [ ] Ties are handled intentionally
- [ ] I can explain the triage function without looking at the code
- [ ] I can debug incorrect classifications
- [ ] Basic automated tests pass

---

# 🏁 Long-Term Goal

By the end of this project, I want to be able to explain and demonstrate how a support issue travels through a modern cloud environment:

```text
Customer Issue
      ↓
REST API
      ↓
Triage
      ↓
Database
      ↓
AI Analysis
      ↓
MCP / Tool Calling
      ↓
Cloud Infrastructure
      ↓
Observability
      ↓
Troubleshooting
      ↓
Resolution
```

The finished project should demonstrate both **cloud engineering** and **technical support engineering** skills—not just that I can deploy an application, but that I understand how to investigate it when something goes wrong.

---

## 📌 Status Legend

🟢 **Completed / Active Foundation**

🟡 **Currently Learning / Building**

⚪ **Planned**

> **Project philosophy:** Build it. Break it. Understand why it broke. Fix it. Document what I learned.
