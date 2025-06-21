# Autoheal RPA Project Documentation

This project outlines the implementation of an automated remediation framework using RPA bots, Azure Monitor, and ServiceNow integration.

## RPA Implementation for Remediation

Implementation of an Autoheal solution leveraging RPA bots, ServiceNow, and Azure Monitor to remediate routine issues on Windows and Linux servers.

### 🧭 Business Requirement Summary

The organization requires an automated remediation framework that:

- Detects and resolves routine infrastructure issues on Windows and Linux servers.

- Integrates with ServiceNow for incident management and escalation.

- Leverages Azure Monitor for real-time telemetry and alerting.

- Engages support engineers when automated remediation fails.

### 🔍 Gap Analysis

| Capability |	Current State	| Gap |
| :--- | :--- | --- |
| **Monitoring**	| Azure Monitor and Prometheus in place	| No automated remediation |
| **Ticketing**	| ServiceNow operational	| No integration with autoheal |
| **Remediation**	| Manual intervention	| No RPA-based automation |
| **Escalation**	| Manual escalation	| Needs automated fallback logic |


### 🧩 Solution Design Overview

🔗 Integration Points

- **Azure Monitor / Prometheus**: Detects anomalies and triggers alerts.

- **ServiceNow**: Receives alerts via webhook/API and creates incident tickets.

- **RPA Orchestrator** (UiPath / Automation Anywhere):

   - Polls or receives triggers from ServiceNow.

   - Executes remediation workflows on target servers.

   - Updates ServiceNow tickets with diagnostics and resolution details.

### 🔄 Autoheal Workflow

1. **Issue Detection**: Azure Monitor detects a server issue (e.g., high CPU, service down).

2. **Alert Routing**: Alert sent to ServiceNow via webhook or Azure Action Group.

3. **Incident Creation**: ServiceNow logs the incident and categorizes it.

4. **Bot Trigger**: RPA Orchestrator triggers bot based on incident type.

5. **Remediation Execution**:

   - Windows: Restart services, clear temp files, kill hung processes.

   - Linux: Restart daemons, rotate logs, free disk space.

6. **Ticket Update**: Bot posts diagnostics, actions taken, and resolution status.

7. **Escalation Logic**: If issue persists, ticket is escalated to support engineer.

### 🖥️ Physical Network Blueprint

- **Monitoring Layer**: Azure Monitor, Prometheus agents on servers.

- **Integration Layer**: Azure Event Grid / Logic Apps → ServiceNow API.

- **Automation Layer**: RPA Orchestrator hosted in secure subnet.

- **Execution Layer**: RPA bots with access to Windows/Linux servers via WinRM/SSH.

- **ServiceNow MID Server** (optional): For secure internal communication.

### 🔌 Interface Definitions

| Interface       |	Protocol    |	Direction      |	Purpose |
|   :---          |    :---:   |       :---:     | ---: |
| **Azure Monitor** → **ServiceNow** | HTTPS/Webhook | Outbound | Alert delivery |
| **ServiceNow** → **RPA** **Orchestrator**	| REST API / Queue	| Outbound	| Bot trigger |
| **RPA Bot → Server**	| WinRM / SSH	| Bidirectional	| Remediation
| **RPA Bot → ServiceNow** | REST API	| Outbound	| Ticket update


### 🚀 Deployment Blueprint

- **Environment**: Hybrid cloud (Azure + on-prem)

- **Security**: Role-based access, encrypted credentials, audit logging

- **Scalability**: Horizontal scaling of bots based on ticket volume

- **Monitoring**: Bot health, execution logs, ticket resolution metrics
