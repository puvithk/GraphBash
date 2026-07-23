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
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 1600 1310">

<style>
  :root {
    --bg: #ffffff;
    --card-bg: #ffffff;
    --border: #d0d7de;
    --border-strong: #24292f;
    --text: #24292f;
    --text-muted: #57606a;
    --green-bg: #dafbe1; --green-border: #2da44e; --green: #116329;
    --orange-bg: #fff1e5; --orange-border: #bc4c00; --orange: #953800;
    --purple-bg: #fbefff; --purple-border: #8250df; --purple: #6639ba;
    --blue-bg: #ddf4ff;  --blue-border: #0969da;  --blue: #0550ae;
    --yellow-bg: #fff8c5; --yellow-border: #9a6700; --yellow: #7d5700;
    --node-item-bg: #fff6ee;
    --arrow: #57606a;
  }
  @media (prefers-color-scheme: dark) {
    :root {
      --bg: #0d1117;
      --card-bg: #161b22;
      --border: #30363d;
      --border-strong: #e6edf3;
      --text: #e6edf3;
      --text-muted: #8b949e;
      --green-bg: #0f2b1d; --green-border: #3fb950; --green: #56d364;
      --orange-bg: #341a05; --orange-border: #d29922; --orange: #e3b341;
      --purple-bg: #271052; --purple-border: #a371f7; --purple: #c297ff;
      --blue-bg: #051d33;  --blue-border: #58a6ff;  --blue: #79c0ff;
      --yellow-bg: #2d2402; --yellow-border: #d29922; --yellow: #e3b341;
      --node-item-bg: #21150a;
      --arrow: #8b949e;
    }
  }
  text { font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Helvetica, Arial, sans-serif; fill: var(--text); }
  .mono { font-family: ui-monospace, SFMono-Regular, "SF Mono", Menlo, Consolas, monospace; }
  .title-main { fill: var(--border-strong); }
  .muted { fill: var(--text-muted); }
  .card { fill: var(--card-bg); stroke: var(--border); stroke-width: 1.4; }
  .frame { fill: var(--card-bg); stroke: var(--border-strong); stroke-width: 1.6; }
  .panel-green { fill: var(--green-bg); stroke: var(--green-border); stroke-width: 1.6; }
  .panel-orange { fill: var(--orange-bg); stroke: var(--orange-border); stroke-width: 1.6; }
  .panel-purple { fill: var(--purple-bg); stroke: var(--purple-border); stroke-width: 1.6; }
  .panel-blue { fill: var(--blue-bg); stroke: var(--blue-border); stroke-width: 1.4; }
  .panel-yellow { fill: var(--yellow-bg); stroke: var(--yellow-border); stroke-width: 1.6; }
  .subcard-green { fill: var(--card-bg); stroke: var(--green-border); stroke-width: 1.2; }
  .subcard-orange { fill: var(--card-bg); stroke: var(--orange-border); stroke-width: 1.2; }
  .subcard-yellow { fill: var(--card-bg); stroke: var(--yellow-border); stroke-width: 1.1; }
  .chip-neutral { fill: var(--card-bg); stroke: var(--border); stroke-width: 1.1; }
  .chip-red { fill: var(--orange-bg); stroke: var(--border); stroke-width: 1.1; }
  .chip-blue { fill: var(--blue-bg); stroke: var(--border); stroke-width: 1.1; }
  .chip-orange-fill { fill: var(--orange-border); }
  .node-item { fill: var(--node-item-bg); stroke: var(--orange-border); stroke-width: 1; }
  .node-bar { fill: var(--orange-border); }
  .pill { fill: var(--card-bg); stroke: var(--purple-border); stroke-width: 1.2; }
  .text-strong { fill: var(--border-strong); }
  .green-text { fill: var(--green); }
  .orange-text { fill: var(--orange); }
  .purple-text { fill: var(--purple); }
  .blue-text { fill: var(--blue); }
  .yellow-text { fill: var(--yellow); }
  .dot-green { fill: var(--green-border); }
  .dot-purple { fill: var(--purple-border); }
  .arrow-line { stroke: var(--arrow); stroke-width: 1.6; }
  .arrow-line-green { stroke: var(--green-border); stroke-width: 1.6; }
  .arrow-line-purple { stroke: var(--purple-border); stroke-width: 1.6; }
  .arrow-line-yellow { stroke: var(--yellow-border); stroke-width: 1.6; }
  .divider { stroke: var(--border); stroke-width: 1; }
  .bgrect { fill: var(--bg); }
</style>
<defs>
  <marker id="arrow" markerWidth="9" markerHeight="9" refX="7" refY="3.5" orient="auto" markerUnits="strokeWidth">
    <path d="M0,0 L7,3.5 L0,7 Z" class="arrow-marker" style="fill:var(--arrow)"/>
  </marker>
  <marker id="arrowG" markerWidth="9" markerHeight="9" refX="7" refY="3.5" orient="auto" markerUnits="strokeWidth">
    <path d="M0,0 L7,3.5 L0,7 Z" style="fill:var(--green-border)"/>
  </marker>
  <marker id="arrowP" markerWidth="9" markerHeight="9" refX="7" refY="3.5" orient="auto" markerUnits="strokeWidth">
    <path d="M0,0 L7,3.5 L0,7 Z" style="fill:var(--purple-border)"/>
  </marker>
  <marker id="arrowY" markerWidth="9" markerHeight="9" refX="7" refY="3.5" orient="auto" markerUnits="strokeWidth">
    <path d="M0,0 L7,3.5 L0,7 Z" style="fill:var(--yellow-border)"/>
  </marker>
</defs>

<rect x="0" y="0" width="1600" height="1310" class="bgrect"/>
<text x="800.0" y="40.0" class="title-main sans" font-size="22" font-weight="700" text-anchor="middle">Production-Grade MCP + LangGraph Cross-Platform Architecture</text>
<text x="800.0" y="63.0" class="muted sans" font-size="13.5" font-weight="400" text-anchor="middle">Windows Control Plane Orchestrating Secure Ubuntu Managed Nodes</text>
<rect x="20.0" y="84.0" width="1560.0" height="100.0" rx="12" class="frame"/>
<text x="38.0" y="110.0" class="text-strong sans" font-size="15.5" font-weight="700" text-anchor="start">1 · Client &amp; Access Layer</text>
<rect x="34.0" y="124.0" width="236.0" height="46.0" rx="8" class="card"/>
<text x="50.0" y="152.0" class="muted mono" font-size="15" font-weight="400" text-anchor="start">🌐</text>
<text x="76.0" y="152.0" class="text sans" font-size="11.5" font-weight="600" text-anchor="start">Web Dashboard</text>
<rect x="286.0" y="124.0" width="236.0" height="46.0" rx="8" class="card"/>
<text x="302.0" y="152.0" class="muted mono" font-size="15" font-weight="400" text-anchor="start">💬</text>
<text x="328.0" y="152.0" class="text sans" font-size="11.5" font-weight="600" text-anchor="start">Chat Interface</text>
<rect x="538.0" y="124.0" width="236.0" height="46.0" rx="8" class="card"/>
<text x="554.0" y="152.0" class="muted mono" font-size="15" font-weight="400" text-anchor="start">&gt;_</text>
<text x="580.0" y="152.0" class="text sans" font-size="11.5" font-weight="600" text-anchor="start">CLI / Desktop</text>
<rect x="790.0" y="124.0" width="236.0" height="46.0" rx="8" class="card"/>
<text x="806.0" y="152.0" class="muted mono" font-size="15" font-weight="400" text-anchor="start">&lt;/&gt;</text>
<text x="832.0" y="152.0" class="text sans" font-size="11.5" font-weight="600" text-anchor="start">REST API Clients</text>
<rect x="1042.0" y="124.0" width="236.0" height="46.0" rx="8" class="card"/>
<text x="1058.0" y="152.0" class="muted mono" font-size="15" font-weight="400" text-anchor="start">🔔</text>
<text x="1084.0" y="152.0" class="text sans" font-size="11.5" font-weight="600" text-anchor="start">Team Chat / Notifications</text>
<rect x="1294.0" y="124.0" width="236.0" height="46.0" rx="8" class="card"/>
<text x="1310.0" y="152.0" class="muted mono" font-size="15" font-weight="400" text-anchor="start">👤</text>
<text x="1336.0" y="152.0" class="text sans" font-size="11.5" font-weight="600" text-anchor="start">SSO / Identity Provider</text>
<rect x="20.0" y="204.0" width="1160.0" height="520.0" rx="12" class="panel-green"/>
<text x="38.0" y="230.0" class="green-text sans" font-size="15.5" font-weight="700" text-anchor="start">2 · Windows Control Plane</text>
<rect x="35.0" y="250.0" width="214.0" height="460.0" rx="8" class="subcard-green"/>
<text x="142.0" y="272.0" class="green-text sans" font-size="12.5" font-weight="700" text-anchor="middle">Interaction Services</text>
<rect x="264.0" y="250.0" width="214.0" height="460.0" rx="8" class="subcard-green"/>
<text x="371.0" y="272.0" class="green-text sans" font-size="12.5" font-weight="700" text-anchor="middle">LangGraph Orchestration</text>
<rect x="493.0" y="250.0" width="214.0" height="460.0" rx="8" class="subcard-green"/>
<text x="600.0" y="272.0" class="green-text sans" font-size="12.5" font-weight="700" text-anchor="middle">MCP Services</text>
<rect x="722.0" y="250.0" width="214.0" height="460.0" rx="8" class="subcard-green"/>
<text x="829.0" y="272.0" class="green-text sans" font-size="12.5" font-weight="700" text-anchor="middle">Security &amp; Platform Services</text>
<rect x="951.0" y="250.0" width="214.0" height="460.0" rx="8" class="subcard-green"/>
<text x="1058.0" y="272.0" class="green-text sans" font-size="12.5" font-weight="700" text-anchor="middle">AI Services</text>
<circle cx="54.0" cy="296.0" r="4.0" class="dot-green"/>
<text x="67.0" y="300.0" class="text sans" font-size="11.5" font-weight="400" text-anchor="start">Chat UI</text>
<circle cx="54.0" cy="334.0" r="4.0" class="dot-green"/>
<text x="67.0" y="338.0" class="text sans" font-size="11.5" font-weight="400" text-anchor="start">Workflow Console</text>
<circle cx="54.0" cy="372.0" r="4.0" class="dot-green"/>
<text x="67.0" y="376.0" class="text sans" font-size="11.5" font-weight="400" text-anchor="start">Node Dashboard</text>
<circle cx="54.0" cy="410.0" r="4.0" class="dot-green"/>
<text x="67.0" y="414.0" class="text sans" font-size="11.5" font-weight="400" text-anchor="start">Approval Interface</text>
<circle cx="54.0" cy="448.0" r="4.0" class="dot-green"/>
<text x="67.0" y="452.0" class="text sans" font-size="11.5" font-weight="400" text-anchor="start">API Service</text>
<rect x="274.0" y="288.0" width="194.0" height="30.0" rx="5" class="chip-neutral"/>
<text x="371.0" y="307.0" class="text sans" font-size="10" font-weight="600" text-anchor="middle">Intent Classifier</text>
<line x1="371.0" y1="318.0" x2="371.0" y2="326.0" class="arrow-line"/>
<rect x="274.0" y="326.0" width="194.0" height="30.0" rx="5" class="chip-neutral"/>
<text x="371.0" y="345.0" class="text sans" font-size="10" font-weight="600" text-anchor="middle">Planner</text>
<line x1="371.0" y1="356.0" x2="371.0" y2="364.0" class="arrow-line"/>
<rect x="274.0" y="364.0" width="93.0" height="30.0" rx="5" class="chip-red"/>
<text x="320.5" y="383.0" class="orange-text sans" font-size="9.5" font-weight="600" text-anchor="middle">Tool Selector</text>
<rect x="375.0" y="364.0" width="93.0" height="30.0" rx="5" class="chip-red"/>
<text x="421.5" y="383.0" class="orange-text sans" font-size="9.5" font-weight="600" text-anchor="middle">Context / RAG</text>
<line x1="371.0" y1="394.0" x2="371.0" y2="402.0" class="arrow-line"/>
<rect x="274.0" y="402.0" width="194.0" height="30.0" rx="5" class="chip-red"/>
<text x="371.0" y="421.0" class="text sans" font-size="10" font-weight="600" text-anchor="middle">Policy &amp; Permission Check</text>
<line x1="371.0" y1="432.0" x2="371.0" y2="440.0" class="arrow-line"/>
<rect x="274.0" y="440.0" width="194.0" height="30.0" rx="5" class="chip-red"/>
<text x="371.0" y="459.0" class="text sans" font-size="10" font-weight="600" text-anchor="middle">Approval (If Needed)</text>
<line x1="371.0" y1="470.0" x2="371.0" y2="478.0" class="arrow-line"/>
<rect x="274.0" y="478.0" width="194.0" height="30.0" rx="5" class="chip-red"/>
<text x="371.0" y="497.0" class="text sans" font-size="10" font-weight="600" text-anchor="middle">Executor</text>
<line x1="371.0" y1="508.0" x2="371.0" y2="516.0" class="arrow-line"/>
<rect x="274.0" y="516.0" width="194.0" height="30.0" rx="5" class="chip-blue"/>
<text x="371.0" y="535.0" class="text sans" font-size="10" font-weight="600" text-anchor="middle">Result Validator</text>
<line x1="371.0" y1="546.0" x2="371.0" y2="554.0" class="arrow-line"/>
<rect x="274.0" y="554.0" width="194.0" height="30.0" rx="5" class="chip-blue"/>
<text x="371.0" y="573.0" class="text sans" font-size="10" font-weight="600" text-anchor="middle">Response Generator</text>
<circle cx="512.0" cy="296.0" r="4.0" class="dot-green"/>
<text x="525.0" y="300.0" class="text sans" font-size="11.5" font-weight="400" text-anchor="start">MCP Gateway</text>
<circle cx="512.0" cy="334.0" r="4.0" class="dot-green"/>
<text x="525.0" y="338.0" class="text sans" font-size="11.5" font-weight="400" text-anchor="start">Tool Registry</text>
<circle cx="512.0" cy="372.0" r="4.0" class="dot-green"/>
<text x="525.0" y="376.0" class="text sans" font-size="11.5" font-weight="400" text-anchor="start">Tool Adapters</text>
<circle cx="512.0" cy="410.0" r="4.0" class="dot-green"/>
<text x="525.0" y="414.0" class="text sans" font-size="11.5" font-weight="400" text-anchor="start">Tool Validation</text>
<circle cx="512.0" cy="448.0" r="4.0" class="dot-green"/>
<text x="525.0" y="452.0" class="text sans" font-size="11.5" font-weight="400" text-anchor="start">Result Normalizer</text>
<circle cx="739.0" cy="292.0" r="3.6" class="dot-green"/>
<text x="750.0" y="296.0" class="text sans" font-size="10.8" font-weight="400" text-anchor="start">RBAC / Authorization</text>
<circle cx="739.0" cy="325.0" r="3.6" class="dot-green"/>
<text x="750.0" y="329.0" class="text sans" font-size="10.8" font-weight="400" text-anchor="start">Policy Engine</text>
<circle cx="739.0" cy="358.0" r="3.6" class="dot-green"/>
<text x="750.0" y="362.0" class="text sans" font-size="10.8" font-weight="400" text-anchor="start">Secrets Manager</text>
<circle cx="739.0" cy="391.0" r="3.6" class="dot-green"/>
<text x="750.0" y="395.0" class="text sans" font-size="10.8" font-weight="400" text-anchor="start">Audit Service</text>
<circle cx="739.0" cy="424.0" r="3.6" class="dot-green"/>
<text x="750.0" y="428.0" class="text sans" font-size="10.8" font-weight="400" text-anchor="start">Scheduler / Jobs</text>
<circle cx="739.0" cy="457.0" r="3.6" class="dot-green"/>
<text x="750.0" y="461.0" class="text sans" font-size="10.8" font-weight="400" text-anchor="start">Rate Limiter</text>
<circle cx="739.0" cy="490.0" r="3.6" class="dot-green"/>
<text x="750.0" y="494.0" class="text sans" font-size="10.8" font-weight="400" text-anchor="start">Config Service</text>
<circle cx="968.0" cy="296.0" r="3.6" class="dot-green"/>
<text x="979.0" y="300.0" class="text sans" font-size="10.8" font-weight="400" text-anchor="start">LLM Gateway</text>
<circle cx="968.0" cy="334.0" r="3.6" class="dot-green"/>
<text x="979.0" y="338.0" class="text sans" font-size="10.8" font-weight="400" text-anchor="start">Embedding Service</text>
<circle cx="968.0" cy="372.0" r="3.6" class="dot-green"/>
<text x="979.0" y="376.0" class="text sans" font-size="10.8" font-weight="400" text-anchor="start">RAG / Knowledge Base</text>
<circle cx="968.0" cy="410.0" r="3.6" class="dot-green"/>
<text x="979.0" y="414.0" class="text sans" font-size="10.8" font-weight="400" text-anchor="start">Prompt Templates</text>
<circle cx="968.0" cy="448.0" r="3.6" class="dot-green"/>
<text x="979.0" y="452.0" class="text sans" font-size="10.8" font-weight="400" text-anchor="start">Model Fallback</text>
<line x1="142.0" y1="184.0" x2="142.0" y2="204.0" class="arrow-line" marker-end="url(#arrow)"/>
<line x1="371.0" y1="184.0" x2="371.0" y2="204.0" class="arrow-line" marker-end="url(#arrow)"/>
<line x1="600.0" y1="184.0" x2="600.0" y2="204.0" class="arrow-line" marker-end="url(#arrow)"/>
<line x1="829.0" y1="184.0" x2="829.0" y2="204.0" class="arrow-line" marker-end="url(#arrow)"/>
<line x1="1058.0" y1="184.0" x2="1058.0" y2="204.0" class="arrow-line" marker-end="url(#arrow)"/>
<rect x="1200.0" y="204.0" width="380.0" height="460.0" rx="12" class="panel-purple"/>
<text x="1218.0" y="230.0" class="purple-text sans" font-size="15.5" font-weight="700" text-anchor="start">7 · DevSecOps &amp; Governance</text>
<circle cx="1229.0" cy="258.0" r="4.0" class="dot-purple"/>
<text x="1244.0" y="262.0" class="text sans" font-size="12" font-weight="400" text-anchor="start">GitHub / Git Repo</text>
<circle cx="1229.0" cy="304.0" r="4.0" class="dot-purple"/>
<text x="1244.0" y="308.0" class="text sans" font-size="12" font-weight="400" text-anchor="start">CI/CD Pipeline</text>
<circle cx="1229.0" cy="350.0" r="4.0" class="dot-purple"/>
<text x="1244.0" y="354.0" class="text sans" font-size="12" font-weight="400" text-anchor="start">Infrastructure as Code</text>
<circle cx="1229.0" cy="396.0" r="4.0" class="dot-purple"/>
<text x="1244.0" y="400.0" class="text sans" font-size="12" font-weight="400" text-anchor="start">Image Scanning</text>
<circle cx="1229.0" cy="442.0" r="4.0" class="dot-purple"/>
<text x="1244.0" y="446.0" class="text sans" font-size="12" font-weight="400" text-anchor="start">Approval Gates</text>
<circle cx="1229.0" cy="488.0" r="4.0" class="dot-purple"/>
<text x="1244.0" y="492.0" class="text sans" font-size="12" font-weight="400" text-anchor="start">Compliance / Audit</text>
<circle cx="1229.0" cy="534.0" r="4.0" class="dot-purple"/>
<text x="1244.0" y="538.0" class="text sans" font-size="12" font-weight="400" text-anchor="start">Key Rotation / Secrets</text>
<circle cx="1229.0" cy="580.0" r="4.0" class="dot-purple"/>
<text x="1244.0" y="584.0" class="text sans" font-size="12" font-weight="400" text-anchor="start">SLA / SLO Monitoring</text>
<line x1="1180.0" y1="464.0" x2="1200.0" y2="464.0" class="arrow-line-purple" stroke-dasharray="5,4" marker-end="url(#arrowP)"/>
<rect x="20.0" y="744.0" width="1160.0" height="220.0" rx="12" class="panel-orange"/>
<text x="38.0" y="770.0" class="orange-text sans" font-size="15.5" font-weight="700" text-anchor="start">3 · Ubuntu Managed Node Cluster</text>
<rect x="35.0" y="786.0" width="363.3" height="162.0" rx="8" class="subcard-orange"/>
<circle cx="51.0" cy="804.0" r="5.0" class="chip-orange-fill"/>
<text x="63.0" y="808.0" class="orange-text sans" font-size="12" font-weight="700" text-anchor="start">Ubuntu Node 1</text>
<rect x="45.0" y="820.0" width="165.7" height="26.0" rx="4" class="node-item"/>
<text x="127.8" y="837.0" class="orange-text sans" font-size="9" font-weight="600" text-anchor="middle">Node Agent</text>
<rect x="216.7" y="820.0" width="165.7" height="26.0" rx="4" class="node-item"/>
<text x="299.5" y="837.0" class="orange-text sans" font-size="9" font-weight="600" text-anchor="middle">SSH Executor</text>
<rect x="45.0" y="852.0" width="165.7" height="26.0" rx="4" class="node-item"/>
<text x="127.8" y="869.0" class="orange-text sans" font-size="9" font-weight="600" text-anchor="middle">System &amp; Process</text>
<rect x="216.7" y="852.0" width="165.7" height="26.0" rx="4" class="node-item"/>
<text x="299.5" y="869.0" class="orange-text sans" font-size="9" font-weight="600" text-anchor="middle">Package Manager</text>
<rect x="45.0" y="884.0" width="165.7" height="26.0" rx="4" class="node-item"/>
<text x="127.8" y="901.0" class="orange-text sans" font-size="9" font-weight="600" text-anchor="middle">Network</text>
<rect x="216.7" y="884.0" width="165.7" height="26.0" rx="4" class="node-item"/>
<text x="299.5" y="901.0" class="orange-text sans" font-size="9" font-weight="600" text-anchor="middle">Logs &amp; Files</text>
<rect x="45.0" y="920.0" width="343.3" height="26.0" rx="5" class="node-bar"/>
<text x="216.7" y="938.0" font-size="9.5" font-weight="700" text-anchor="middle" style="fill:#ffffff" class="sans">Policy Enforcement &amp; Sandbox</text>
<rect x="418.3" y="786.0" width="363.3" height="162.0" rx="8" class="subcard-orange"/>
<circle cx="434.3" cy="804.0" r="5.0" class="chip-orange-fill"/>
<text x="446.3" y="808.0" class="orange-text sans" font-size="12" font-weight="700" text-anchor="start">Ubuntu Node 2</text>
<rect x="428.3" y="820.0" width="165.7" height="26.0" rx="4" class="node-item"/>
<text x="511.2" y="837.0" class="orange-text sans" font-size="9" font-weight="600" text-anchor="middle">Node Agent</text>
<rect x="600.0" y="820.0" width="165.7" height="26.0" rx="4" class="node-item"/>
<text x="682.8" y="837.0" class="orange-text sans" font-size="9" font-weight="600" text-anchor="middle">SSH Executor</text>
<rect x="428.3" y="852.0" width="165.7" height="26.0" rx="4" class="node-item"/>
<text x="511.2" y="869.0" class="orange-text sans" font-size="9" font-weight="600" text-anchor="middle">System &amp; Containers</text>
<rect x="600.0" y="852.0" width="165.7" height="26.0" rx="4" class="node-item"/>
<text x="682.8" y="869.0" class="orange-text sans" font-size="9" font-weight="600" text-anchor="middle">Package Manager</text>
<rect x="428.3" y="884.0" width="165.7" height="26.0" rx="4" class="node-item"/>
<text x="511.2" y="901.0" class="orange-text sans" font-size="9" font-weight="600" text-anchor="middle">Network</text>
<rect x="600.0" y="884.0" width="165.7" height="26.0" rx="4" class="node-item"/>
<text x="682.8" y="901.0" class="orange-text sans" font-size="9" font-weight="600" text-anchor="middle">Logs &amp; Files</text>
<rect x="428.3" y="920.0" width="343.3" height="26.0" rx="5" class="node-bar"/>
<text x="600.0" y="938.0" font-size="9.5" font-weight="700" text-anchor="middle" style="fill:#ffffff" class="sans">Policy Enforcement &amp; Sandbox</text>
<rect x="801.7" y="786.0" width="363.3" height="162.0" rx="8" class="subcard-orange"/>
<circle cx="817.7" cy="804.0" r="5.0" class="chip-orange-fill"/>
<text x="829.7" y="808.0" class="orange-text sans" font-size="12" font-weight="700" text-anchor="start">Ubuntu Node N</text>
<rect x="811.7" y="820.0" width="165.7" height="26.0" rx="4" class="node-item"/>
<text x="894.5" y="837.0" class="orange-text sans" font-size="9" font-weight="600" text-anchor="middle">Node Agent</text>
<rect x="983.3" y="820.0" width="165.7" height="26.0" rx="4" class="node-item"/>
<text x="1066.2" y="837.0" class="orange-text sans" font-size="9" font-weight="600" text-anchor="middle">SSH Executor</text>
<rect x="811.7" y="852.0" width="165.7" height="26.0" rx="4" class="node-item"/>
<text x="894.5" y="869.0" class="orange-text sans" font-size="9" font-weight="600" text-anchor="middle">System &amp; Process</text>
<rect x="983.3" y="852.0" width="165.7" height="26.0" rx="4" class="node-item"/>
<text x="1066.2" y="869.0" class="orange-text sans" font-size="9" font-weight="600" text-anchor="middle">Package Manager</text>
<rect x="811.7" y="884.0" width="165.7" height="26.0" rx="4" class="node-item"/>
<text x="894.5" y="901.0" class="orange-text sans" font-size="9" font-weight="600" text-anchor="middle">Network</text>
<rect x="983.3" y="884.0" width="165.7" height="26.0" rx="4" class="node-item"/>
<text x="1066.2" y="901.0" class="orange-text sans" font-size="9" font-weight="600" text-anchor="middle">Logs &amp; Files</text>
<rect x="811.7" y="920.0" width="343.3" height="26.0" rx="5" class="node-bar"/>
<text x="983.3" y="938.0" font-size="9.5" font-weight="700" text-anchor="middle" style="fill:#ffffff" class="sans">Policy Enforcement &amp; Sandbox</text>
<rect x="180.0" y="721.0" width="700.0" height="22.0" rx="11" class="pill"/>
<text x="530.0" y="736.0" class="purple-text sans" font-size="10" font-weight="600" text-anchor="middle">🔒  Secure Communication Channel (mTLS / SSH)   |   Signed Requests   |   Zero-Trust Policy Checks</text>
<line x1="216.7" y1="724.0" x2="216.7" y2="744.0" class="arrow-line-green" stroke-dasharray="5,4" marker-end="url(#arrowG)"/>
<line x1="600.0" y1="724.0" x2="600.0" y2="744.0" class="arrow-line-green" stroke-dasharray="5,4" marker-end="url(#arrowG)"/>
<line x1="983.3" y1="724.0" x2="983.3" y2="744.0" class="arrow-line-green" stroke-dasharray="5,4" marker-end="url(#arrowG)"/>
<rect x="1200.0" y="684.0" width="380.0" height="120.0" rx="12" class="panel-blue"/>
<text x="1218.0" y="706.0" class="blue-text sans" font-size="15.5" font-weight="700" text-anchor="start">5 · Communication Flows</text>
<line x1="1216.0" y1="718.0" x2="1250.0" y2="718.0" class="arrow-line"/>
<text x="1258.0" y="722.0" class="text sans" font-size="9.3" font-weight="400" text-anchor="start">Client to Control Plane (HTTPS)</text>
<line x1="1216.0" y1="734.5" x2="1250.0" y2="734.5" class="arrow-line-green" stroke-dasharray="5,4"/>
<text x="1258.0" y="738.5" class="text sans" font-size="9.3" font-weight="400" text-anchor="start">Control Plane to Nodes (mTLS/SSH)</text>
<line x1="1216.0" y1="751.0" x2="1250.0" y2="751.0" class="arrow-line" stroke-dasharray="5,4"/>
<text x="1258.0" y="755.0" class="text sans" font-size="9.3" font-weight="400" text-anchor="start">Control Plane to Data / Services</text>
<line x1="1216.0" y1="767.5" x2="1250.0" y2="767.5" class="arrow-line-purple" stroke-dasharray="5,4"/>
<text x="1258.0" y="771.5" class="text sans" font-size="9.3" font-weight="400" text-anchor="start">CI/CD &amp; Governance Flows</text>
<line x1="1216.0" y1="784.0" x2="1250.0" y2="784.0" class="arrow-line-yellow" stroke-dasharray="5,4"/>
<text x="1258.0" y="788.0" class="text sans" font-size="9.3" font-weight="400" text-anchor="start">Telemetry / Observability Data</text>
<rect x="1200.0" y="824.0" width="380.0" height="140.0" rx="12" class="card"/>
<text x="1218.0" y="846.0" class="text-strong sans" font-size="15.5" font-weight="700" text-anchor="start">6 · Key Features</text>
<text x="1216.0" y="864.0" class="green-text sans" font-size="10.5" font-weight="700" text-anchor="start">✓</text>
<text x="1230.0" y="864.0" class="text sans" font-size="9.2" font-weight="400" text-anchor="start">Natural Language Interface</text>
<text x="1400.0" y="864.0" class="green-text sans" font-size="10.5" font-weight="700" text-anchor="start">✓</text>
<text x="1414.0" y="864.0" class="text sans" font-size="9.2" font-weight="400" text-anchor="start">Policy Enforcement</text>
<text x="1216.0" y="880.2" class="green-text sans" font-size="10.5" font-weight="700" text-anchor="start">✓</text>
<text x="1230.0" y="880.2" class="text sans" font-size="9.2" font-weight="400" text-anchor="start">Secure Remote Execution</text>
<text x="1400.0" y="880.2" class="green-text sans" font-size="10.5" font-weight="700" text-anchor="start">✓</text>
<text x="1414.0" y="880.2" class="text sans" font-size="9.2" font-weight="400" text-anchor="start">Observability &amp; Alerts</text>
<text x="1216.0" y="896.4" class="green-text sans" font-size="10.5" font-weight="700" text-anchor="start">✓</text>
<text x="1230.0" y="896.4" class="text sans" font-size="9.2" font-weight="400" text-anchor="start">Approval Workflow</text>
<text x="1400.0" y="896.4" class="green-text sans" font-size="10.5" font-weight="700" text-anchor="start">✓</text>
<text x="1414.0" y="896.4" class="text sans" font-size="9.2" font-weight="400" text-anchor="start">RAG &amp; Knowledge Access</text>
<text x="1216.0" y="912.6" class="green-text sans" font-size="10.5" font-weight="700" text-anchor="start">✓</text>
<text x="1230.0" y="912.6" class="text sans" font-size="9.2" font-weight="400" text-anchor="start">RBAC &amp; Least Privilege</text>
<text x="1400.0" y="912.6" class="green-text sans" font-size="10.5" font-weight="700" text-anchor="start">✓</text>
<text x="1414.0" y="912.6" class="text sans" font-size="9.2" font-weight="400" text-anchor="start">CI/CD &amp; DevSecOps</text>
<text x="1216.0" y="928.8" class="green-text sans" font-size="10.5" font-weight="700" text-anchor="start">✓</text>
<text x="1230.0" y="928.8" class="text sans" font-size="9.2" font-weight="400" text-anchor="start">Audit &amp; Compliance</text>
<text x="1400.0" y="928.8" class="green-text sans" font-size="10.5" font-weight="700" text-anchor="start">✓</text>
<text x="1414.0" y="928.8" class="text sans" font-size="9.2" font-weight="400" text-anchor="start">High Availability</text>
<text x="1216.0" y="945.0" class="green-text sans" font-size="10.5" font-weight="700" text-anchor="start">✓</text>
<text x="1230.0" y="945.0" class="text sans" font-size="9.2" font-weight="400" text-anchor="start">Multi-Node Management</text>
<text x="1400.0" y="945.0" class="green-text sans" font-size="10.5" font-weight="700" text-anchor="start">✓</text>
<text x="1414.0" y="945.0" class="text sans" font-size="9.2" font-weight="400" text-anchor="start">Scalable &amp; Extensible</text>
<rect x="20.0" y="984.0" width="1160.0" height="200.0" rx="12" class="panel-yellow"/>
<text x="38.0" y="1010.0" class="yellow-text sans" font-size="15.5" font-weight="700" text-anchor="start">4 · Data, Messaging &amp; Observability Plane</text>
<rect x="35.0" y="1026.0" width="214.0" height="144.0" rx="8" class="subcard-yellow"/>
<text x="142.0" y="1046.0" class="yellow-text sans" font-size="10.3" font-weight="700" text-anchor="middle">PostgreSQL (Persistent)</text>
<text x="47.0" y="1064.0" class="muted sans" font-size="8.6" font-weight="400" text-anchor="start">• Workflows</text>
<text x="47.0" y="1079.5" class="muted sans" font-size="8.6" font-weight="400" text-anchor="start">• Nodes</text>
<text x="47.0" y="1095.0" class="muted sans" font-size="8.6" font-weight="400" text-anchor="start">• Executions</text>
<text x="47.0" y="1110.5" class="muted sans" font-size="8.6" font-weight="400" text-anchor="start">• Approvals</text>
<text x="47.0" y="1126.0" class="muted sans" font-size="8.6" font-weight="400" text-anchor="start">• Audit Logs</text>
<text x="47.0" y="1141.5" class="muted sans" font-size="8.6" font-weight="400" text-anchor="start">• Users &amp; Roles</text>
<text x="47.0" y="1157.0" class="muted sans" font-size="8.6" font-weight="400" text-anchor="start">• Policies</text>
<rect x="264.0" y="1026.0" width="214.0" height="144.0" rx="8" class="subcard-yellow"/>
<text x="371.0" y="1046.0" class="yellow-text sans" font-size="10.3" font-weight="700" text-anchor="middle">Redis (Cache &amp; State)</text>
<text x="276.0" y="1064.0" class="muted sans" font-size="8.6" font-weight="400" text-anchor="start">• Sessions</text>
<text x="276.0" y="1079.5" class="muted sans" font-size="8.6" font-weight="400" text-anchor="start">• Workflow State</text>
<text x="276.0" y="1095.0" class="muted sans" font-size="8.6" font-weight="400" text-anchor="start">• Cache</text>
<text x="276.0" y="1110.5" class="muted sans" font-size="8.6" font-weight="400" text-anchor="start">• Rate Limits</text>
<text x="276.0" y="1126.0" class="muted sans" font-size="8.6" font-weight="400" text-anchor="start">• Job Status</text>
<text x="276.0" y="1141.5" class="muted sans" font-size="8.6" font-weight="400" text-anchor="start">• Node Heartbeats</text>
<rect x="493.0" y="1026.0" width="214.0" height="144.0" rx="8" class="subcard-yellow"/>
<text x="600.0" y="1046.0" class="yellow-text sans" font-size="10.3" font-weight="700" text-anchor="middle">Object Storage</text>
<text x="505.0" y="1064.0" class="muted sans" font-size="8.6" font-weight="400" text-anchor="start">• Reports</text>
<text x="505.0" y="1079.5" class="muted sans" font-size="8.6" font-weight="400" text-anchor="start">• Logs</text>
<text x="505.0" y="1095.0" class="muted sans" font-size="8.6" font-weight="400" text-anchor="start">• Artifacts</text>
<text x="505.0" y="1110.5" class="muted sans" font-size="8.6" font-weight="400" text-anchor="start">• Backups</text>
<text x="505.0" y="1126.0" class="muted sans" font-size="8.6" font-weight="400" text-anchor="start">• Exports</text>
<rect x="722.0" y="1026.0" width="214.0" height="144.0" rx="8" class="subcard-yellow"/>
<text x="829.0" y="1046.0" class="yellow-text sans" font-size="10.3" font-weight="700" text-anchor="middle">Message Broker</text>
<text x="734.0" y="1064.0" class="muted sans" font-size="8.6" font-weight="400" text-anchor="start">• Jobs</text>
<text x="734.0" y="1079.5" class="muted sans" font-size="8.6" font-weight="400" text-anchor="start">• Events</text>
<text x="734.0" y="1095.0" class="muted sans" font-size="8.6" font-weight="400" text-anchor="start">• Notifications</text>
<text x="734.0" y="1110.5" class="muted sans" font-size="8.6" font-weight="400" text-anchor="start">• Retries</text>
<text x="734.0" y="1126.0" class="muted sans" font-size="8.6" font-weight="400" text-anchor="start">• Queues</text>
<rect x="951.0" y="1026.0" width="214.0" height="144.0" rx="8" class="subcard-yellow"/>
<text x="1058.0" y="1046.0" class="yellow-text sans" font-size="10.3" font-weight="700" text-anchor="middle">Observability Stack</text>
<text x="963.0" y="1064.0" class="muted sans" font-size="8.6" font-weight="400" text-anchor="start">• Prometheus (Metrics)</text>
<text x="963.0" y="1079.5" class="muted sans" font-size="8.6" font-weight="400" text-anchor="start">• Grafana (Dashboards)</text>
<text x="963.0" y="1095.0" class="muted sans" font-size="8.6" font-weight="400" text-anchor="start">• Loki / ELK (Logs)</text>
<text x="963.0" y="1110.5" class="muted sans" font-size="8.6" font-weight="400" text-anchor="start">• OpenTelemetry (Traces)</text>
<text x="963.0" y="1126.0" class="muted sans" font-size="8.6" font-weight="400" text-anchor="start">• LangSmith (LLM Traces)</text>
<text x="963.0" y="1141.5" class="muted sans" font-size="8.6" font-weight="400" text-anchor="start">• Alertmanager (Alerts)</text>
<line x1="216.7" y1="964.0" x2="216.7" y2="984.0" class="arrow-line-yellow" stroke-dasharray="5,4" marker-end="url(#arrowY)"/>
<line x1="600.0" y1="964.0" x2="600.0" y2="984.0" class="arrow-line-yellow" stroke-dasharray="5,4" marker-end="url(#arrowY)"/>
<line x1="983.3" y1="964.0" x2="983.3" y2="984.0" class="arrow-line-yellow" stroke-dasharray="5,4" marker-end="url(#arrowY)"/>
<rect x="20.0" y="1204.0" width="1560.0" height="70.0" rx="12" class="frame"/>
<text x="50.0" y="1234.0" class="muted mono" font-size="17" font-weight="400" text-anchor="start">🔒</text>
<text x="76.0" y="1231.0" class="text-strong sans" font-size="12" font-weight="700" text-anchor="start">Security First</text>
<text x="76.0" y="1250.0" class="muted sans" font-size="9.5" font-weight="400" text-anchor="start">Zero-Trust, mTLS, RBAC</text>
<text x="310.0" y="1234.0" class="muted mono" font-size="17" font-weight="400" text-anchor="start">📋</text>
<text x="336.0" y="1231.0" class="text-strong sans" font-size="12" font-weight="700" text-anchor="start">Auditable</text>
<text x="336.0" y="1250.0" class="muted sans" font-size="9.5" font-weight="400" text-anchor="start">Every action logged</text>
<line x1="280.0" y1="1216.0" x2="280.0" y2="1262.0" class="divider"/>
<text x="570.0" y="1234.0" class="muted mono" font-size="17" font-weight="400" text-anchor="start">🛡</text>
<text x="596.0" y="1231.0" class="text-strong sans" font-size="12" font-weight="700" text-anchor="start">Reliable</text>
<text x="596.0" y="1250.0" class="muted sans" font-size="9.5" font-weight="400" text-anchor="start">Retries, Timeouts, Fallbacks</text>
<line x1="540.0" y1="1216.0" x2="540.0" y2="1262.0" class="divider"/>
<text x="830.0" y="1234.0" class="muted mono" font-size="17" font-weight="400" text-anchor="start">↔</text>
<text x="856.0" y="1231.0" class="text-strong sans" font-size="12" font-weight="700" text-anchor="start">Scalable</text>
<text x="856.0" y="1250.0" class="muted sans" font-size="9.5" font-weight="400" text-anchor="start">Add more nodes easily</text>
<line x1="800.0" y1="1216.0" x2="800.0" y2="1262.0" class="divider"/>
<text x="1090.0" y="1234.0" class="muted mono" font-size="17" font-weight="400" text-anchor="start">📈</text>
<text x="1116.0" y="1231.0" class="text-strong sans" font-size="12" font-weight="700" text-anchor="start">Observable</text>
<text x="1116.0" y="1250.0" class="muted sans" font-size="9.5" font-weight="400" text-anchor="start">Metrics, Logs, Traces, Alerts</text>
<line x1="1060.0" y1="1216.0" x2="1060.0" y2="1262.0" class="divider"/>
<text x="1350.0" y="1234.0" class="muted mono" font-size="17" font-weight="400" text-anchor="start">⚙</text>
<text x="1376.0" y="1231.0" class="text-strong sans" font-size="12" font-weight="700" text-anchor="start">Automated</text>
<text x="1376.0" y="1250.0" class="muted sans" font-size="9.5" font-weight="400" text-anchor="start">CI/CD, IaC, Policy as Code</text>
<line x1="1320.0" y1="1216.0" x2="1320.0" y2="1262.0" class="divider"/>
</svg>

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
