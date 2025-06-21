# Autoheal Deployment Checklist and Interface Specification

## Deployment Checklist

### Environment Setup

- Provision RPA Orchestrator (e.g., UiPath, Automation Anywhere)

- Ensure network connectivity between RPA bots and target servers (Windows/Linux)

- Install and configure ServiceNow MID Server (if required)

- Ensure Azure Monitor and/or Prometheus agents are installed on all servers

### Integration Configuration

- Configure Azure Monitor alerts and Action Groups

- Set up webhook/API integration from Azure Monitor to ServiceNow

- Create ServiceNow incident rules for autoheal triggers

- Configure RPA Orchestrator to poll or receive triggers from ServiceNow

### Security and Access Control

- Create service accounts for RPA bots with least privilege access

- Configure WinRM (Windows) and SSH (Linux) access for bots

- Secure API credentials and tokens using vault or credential manager

- Enable audit logging on all components

### Testing and Validation

- Simulate alerts from Azure Monitor and verify ticket creation in ServiceNow

- Test RPA bot remediation workflows on test servers

- Validate ticket updates and resolution status in ServiceNow

- Perform end-to-end test with escalation scenario

### Escalation Handling

- Define escalation rules in ServiceNow for unresolved incidents

- Configure RPA bots to update ticket status and assign to support engineer

- Verify notification and SLA compliance for escalated tickets

## Interface Specification Document

 **Azure Monitor to ServiceNow**
- Protocol: HTTPS (Webhook)
- Direction: Outbound from Azure Monitor
- Authentication: OAuth2 or Shared Secret
- Data Format: JSON
- Payload Example: {"alertName": "HighCPU", "resource": "Server01", "severity": "Critical"}
  
 **ServiceNow to RPA Orchestrator**
- Protocol: REST API or Queue Trigger
- Direction: Outbound from ServiceNow
- Authentication: API Key or OAuth2
- Data Format: JSON
- Payload Example: {"incidentId": "INC0012345", "category": "CPU", "server": "Server01"}

 **RPA Bot to Target Server**
- Protocol: WinRM (Windows) / SSH (Linux)
- Direction: Bidirectional
- Authentication: Kerberos / SSH Key / Password
- Data Format: Command Execution
- Payload Example: Restart-Service -Name "Spooler" (Windows), systemctl restart apache2 (Linux)

 **RPA Bot to ServiceNow**
- Protocol: REST API
- Direction: Outbound from RPA Bot
- Authentication: OAuth2 or API Token
- Data Format: JSON
- Payload Example: {"incidentId": "INC0012345", "status": "Resolved", "notes": "Service restarted successfully."}
