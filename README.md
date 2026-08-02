# NodePilot

> Secure AI-powered cross-platform system administration using LangGraph, Model Context Protocol, and controlled Linux node agents.

![Status](https://img.shields.io/badge/status-foundation-yellow)
![Python](https://img.shields.io/badge/Python-3.12%2B-blue)
![LangGraph](https://img.shields.io/badge/orchestration-LangGraph-purple)
![MCP](https://img.shields.io/badge/protocol-MCP-green)
![License](https://img.shields.io/badge/license-MIT-lightgrey)

## Project Overview

NodePilot is a production-oriented AI system administration platform that allows users to monitor, troubleshoot, and safely operate Linux machines from a centralized control plane.

The platform accepts natural-language requests, routes them through a LangGraph workflow, selects an approved MCP tool, evaluates security policies, requests human approval when necessary, executes the operation on a managed Linux node, and stores an auditable result.

NodePilot is not an unrestricted AI shell. The AI selects an approved tool, while the security layer decides whether that operation is allowed.

## Project Problem

Linux administration normally requires users to:

- Remember operating-system and Docker commands
- Connect manually through SSH
- Inspect system logs across different tools
- Monitor multiple machines separately
- Handle permissions and approvals manually
- Track who executed each operation
- Recover from failed or timed-out commands
- Translate raw command output into understandable reports

These challenges increase when multiple servers, users, environments, and security requirements are involved.

Directly connecting an LLM to a shell would also introduce serious risks, including command injection, unrestricted execution, accidental destructive actions, privilege escalation, and poor auditability.

## Proposed Solution

NodePilot adds a secure orchestration layer between the user and managed Linux systems.

The solution consists of:

1. A centralized control plane that receives requests.
2. LangGraph workflows that classify, plan, route, and validate tasks.
3. An MCP gateway that exposes standardized administration tools.
4. A policy engine that checks roles, permissions, risk levels, and target nodes.
5. An approval workflow for system-changing actions.
6. A lightweight Linux node agent that executes only validated operations.
7. PostgreSQL and Redis for workflow state, sessions, approvals, and audit records.
8. Observability services for logs, metrics, traces, and alerts.

## Repository Structure

NodePilot is divided into three repositories.

| Repository | Responsibility | Runtime |
|---|---|---|
| `NodePilot` | Project documentation, architecture, roadmap, security model, and release coordination | GitHub |
| `nodepilot-control-plane` | LangGraph orchestration, APIs, MCP gateway, policies, approvals, node registry, and audit services | Windows or server |
| `nodepilot-node-agent` | Controlled Linux, Docker, process, network, service, package, and filesystem operations | Ubuntu/Linux |

### Related Repositories

- [NodePilot Control Plane](https://github.com/puvithk/nodepilot-control-plane)
- [NodePilot Linux Node Agent](https://github.com/puvithk/nodepilot-node-agent)

## Architecture Diagram
![GraphBash Architecture](./docs/architecture/graphbash-architecture.svg)
## Technology Stack

### Agent and AI

- LangGraph
- LangChain
- Model Context Protocol SDK
- Mistral, Llama, or Nemotron
- Optional RAG and embedding service

### Backend

- Python 3.12+
- FastAPI
- Uvicorn
- Pydantic
- SQLAlchemy
- Alembic

### Data and Messaging

- PostgreSQL
- Redis
- Redis Streams, RabbitMQ, or NATS
- S3-compatible object storage for diagnostic bundles and reports

### Security

- OIDC or SSO
- Role-based access control
- Tool-level and node-level permissions
- Signed requests or mutual TLS
- Secrets manager
- Approval gates
- Command allowlists and input validation

### Observability

- Prometheus
- Grafana
- Loki
- OpenTelemetry
- LangSmith
- Alertmanager

### Testing and Delivery

- Pytest
- Ruff
- Black
- MyPy
- Bandit
- pip-audit
- Trivy
- Pre-commit
- Docker
- GitHub Actions

## Risk Classification

| Risk | Description | Example | Default Behaviour |
|---|---|---|---|
| Low | Read-only operation | Check CPU or memory | Execute automatically |
| Medium | Reversible operation | Restart a service | Require confirmation |
| High | System-changing action | Install a package | Require explicit approval |
| Critical | Destructive action | Delete system files | Block by default |

## Planned Capabilities

- Natural-language system administration requests
- Linux node registration and heartbeat monitoring
- CPU, memory, disk, uptime, and load inspection
- Process listing and resource analysis
- Docker container listing, logs, stats, and approved lifecycle operations
- systemd service inspection and approved restarts
- Listening-port and network-connection inspection
- Safe filesystem search and size analysis
- Package update checks
- Human approval for risky actions
- Complete audit history
- Structured system-health reports
- Multi-node comparison
- Metrics, logs, traces, alerts, retries, and timeouts

## Current Development Status

**Current phase: Project foundation**

| Area | Status |
|---|---|
| Project name and repository strategy | Completed |
| High-level architecture | Completed |
| Security principles | Defined |
| Main documentation repository | In progress |
| Control-plane application skeleton | Planned |
| Linux node-agent skeleton | Planned |
| LangGraph workflow | Planned |
| MCP tool registry | Planned |
| Node registration and communication | Planned |
| Core monitoring tools | Planned |
| Approval workflow | Planned |
| Audit persistence | Planned |
| Observability | Planned |
| CI/CD | Planned |
| Production release | Planned |

> The project is under active development and is not ready for production use.

## Roadmap

### Phase 1 — Foundation

- Initialize all repositories
- Add README, license, security policy, and contribution guide
- Define coding standards and branch strategy
- Create FastAPI service skeletons
- Add health, readiness, version, and metrics endpoints
- Add basic automated checks

### Phase 2 — Node Connectivity and MCP

- Implement node registration
- Add node identity and authentication
- Create MCP gateway and tool registry
- Establish secure control-plane-to-agent communication
- Add node heartbeat and availability status
- Implement request IDs and structured errors

### Phase 3 — Read-Only Administration Tools

- System information
- CPU, memory, disk, load, kernel, and uptime
- Process listing and inspection
- Docker listing, inspection, logs, and stats
- Service status and logs
- Network interfaces, ports, routes, and connections
- Safe filesystem inspection
- Package update checks

### Phase 4 — LangGraph and Security

- Intent classification
- Task planning and tool selection
- Policy evaluation
- RBAC
- Risk classification
- Approval workflow
- Result validation
- Audit records and workflow checkpoints

### Phase 5 — Reliability and Observability

- Retries and exponential backoff
- Timeouts and circuit breakers
- Queue-based long-running tasks
- Prometheus metrics
- Grafana dashboards
- Centralized logs
- OpenTelemetry traces
- Alerting and node-offline detection

### Phase 6 — Release

- Integration and end-to-end tests
- Security testing
- Docker deployment
- Installation documentation
- Demonstration environment
- Three public progress posts
- Release candidate
- Version `1.0.0`

## Development Principles

- No unrestricted shell access
- Read-only operations first
- Least-privilege execution
- Explicit approval for state-changing operations
- Critical operations blocked by default
- Every operation must be traceable
- Tools must use typed and validated parameters
- Secrets must never be committed
- Security policy takes precedence over AI decisions
- Tests and documentation are part of every feature

## Public Progress Plan

Before version `1.0.0`, at least three development posts will be published:

1. Problem statement, architecture, and planned capabilities
2. Working LangGraph and MCP prototype with Linux tools
3. Security, approvals, observability, testing, and release-candidate results

## Project Status Disclaimer

NodePilot is currently an educational and portfolio project under development. Do not use it to administer production infrastructure until authentication, authorization, transport security, audit persistence, tests, and security reviews are complete.

## License

This project is planned to be released under the MIT License.

## Author

**Puvith Kumar**

- GitHub: [puvithk](https://github.com/puvithk)
