# DevOps/SRE Flavor

Incident management, infrastructure documentation, and operational runbooks.

## Configuration

```yaml
name: devops-sre
display_name: DevOps/SRE
description: Incident response, runbooks, infrastructure decisions, and postmortems
```

## Directory Structure

```yaml
directories:
  - incidents      # Incident reports, postmortems, RCAs
  - runbooks       # Operational procedures, playbooks
  - decisions      # Infrastructure and architecture choices
  - troubleshooting # Common issues and debugging guides
  - architecture   # System diagrams, service maps
```

## Protocol Examples

Use these domain-specific examples in the note-taking protocol.

### When to Load Additional Files

**User says:** "We had a deployment failure last night"
**You respond:** "I see from INDEX.md we had a similar incident on 2025-01-10. Should I read `incidents/2025-01-10-deploy-rollback.md` for the postmortem?"

**User says:** "How do I restart the payment service?"
**You respond:** "Should I check `runbooks/payment-service-restart.md` for the procedure?"

**User says:** "Why did we choose Kubernetes over ECS?"
**You respond:** "Should I read `decisions/2025-01-08-container-orchestration.md` for the rationale?"

**User says:** "I'm seeing high latency on the API"
**You respond:** "Should I check `troubleshooting/api-latency.md` for known causes and solutions?"

**User says:** "How does auth flow through our services?"
**You respond:** "Should I read `architecture/auth-flow.md` for the service diagram?"

### Progressive Disclosure Example Session

```
[Session starts]
Claude: [Reads notes/INDEX.md - 1K tokens]
Claude: [Reads notes/quick-reference.md - 500 tokens]
Claude: "I've loaded your project notes index. What are you working on?"

User: "We need to investigate yesterday's outage"

Claude: "From INDEX.md, I see we had an incident yesterday affecting
the checkout service. Should I read the incident report?"

User: "Yes"

Claude: [Uses view on notes/incidents/2025-01-14-checkout-outage.md - 2K tokens]
Claude: "Based on that report, the root cause was database connection
pool exhaustion. The runbook was followed but step 3 needs updating.
Want to update the runbook or write a follow-up action item?"

[Works on task...]

[Session ends]
Claude: "Should I create an incident note for today's investigation?"

User: "Yes, and update the runbook"

Claude: [Creates notes/incidents/2025-01-15-checkout-followup.md]
Claude: [Updates notes/runbooks/checkout-recovery.md]
Claude: "Done. Should I update INDEX.md with the new action items?"

Total context used: ~5K tokens instead of 50K+ if all files loaded
```

## INDEX.md Examples

```markdown
## All Incidents
<!-- Each entry has: Title, Tags, Finding, File link -->
<!-- Example:
**2025-01-14: Checkout service outage (SEV-2)**
- Tags: checkout, database, connection-pool, outage
- Finding: Connection pool exhaustion under load; increased pool size
- File: [incidents/2025-01-14-checkout-outage.md](incidents/2025-01-14-checkout-outage.md)
-->

## All Runbooks
<!-- Example:
**Checkout service recovery**
- Tags: checkout, recovery, database, restart
- Finding: Step-by-step recovery for checkout failures
- File: [runbooks/checkout-recovery.md](runbooks/checkout-recovery.md)
-->

## All Decisions
<!-- Example:
**2025-01-08: Choose Kubernetes over ECS**
- Tags: kubernetes, ecs, containers, infrastructure
- Finding: K8s chosen for multi-cloud flexibility and ecosystem
- File: [decisions/2025-01-08-container-orchestration.md](decisions/2025-01-08-container-orchestration.md)
-->

## All Troubleshooting Guides
<!-- Example:
**API latency issues**
- Tags: api, latency, performance, database
- Finding: Common causes: DB queries, connection limits, cache misses
- File: [troubleshooting/api-latency.md](troubleshooting/api-latency.md)
-->

## All Architecture
<!-- Example:
**Authentication flow**
- Tags: auth, jwt, oauth, services
- Finding: OAuth2 with JWT, 15-min access tokens, refresh via auth-service
- File: [architecture/auth-flow.md](architecture/auth-flow.md)
-->
```

## quick-reference.md Format

```yaml
# Configuration format for "What's Working Right Now"
format: mixed
example: |
  ## Current On-Call
  Primary: @alice
  Secondary: @bob
  Escalation: #incident-response

  ## Key Thresholds
  API latency SLO: p99 < 200ms
  Error rate SLO: < 0.1%
  Uptime SLI: 99.9%

  ## Critical Services
  - payment-service: prod-payment-1.internal:8080
  - auth-service: prod-auth-1.internal:8081
  - checkout-api: prod-checkout-1.internal:8082

  ## Quick Commands
  # Check service health
  kubectl get pods -n production

  # View recent logs
  kubectl logs -f deployment/payment-service -n production

  # Rollback deployment
  kubectl rollout undo deployment/payment-service -n production
```

## Tool Integration

- **Infrastructure**: Terraform, Pulumi, CloudFormation, Ansible
- **Containers**: Kubernetes, Docker, Helm, ArgoCD
- **Monitoring**: Prometheus, Grafana, Datadog, New Relic
- **Alerting**: PagerDuty, OpsGenie, VictorOps
- **Logging**: ELK Stack, Loki, Splunk, CloudWatch
- **CI/CD**: GitHub Actions, GitLab CI, Jenkins, Spinnaker

## Incident Note Format

Incidents have a specific format for postmortems:

```markdown
# Incident: [Title] (SEV-X)

**Date:** YYYY-MM-DD
**Duration:** HH:MM - HH:MM (X hours)
**Impact:** [User-facing impact description]
**Status:** Resolved | Monitoring | Open

## Timeline
- HH:MM - Alert fired
- HH:MM - On-call paged
- HH:MM - Root cause identified
- HH:MM - Mitigation applied
- HH:MM - Resolved

## Root Cause
[Technical explanation]

## Resolution
[What fixed it]

## Action Items
- [ ] [Action 1] - Owner: @name - Due: YYYY-MM-DD
- [ ] [Action 2] - Owner: @name - Due: YYYY-MM-DD

## Lessons Learned
- What went well
- What went poorly
- Where we got lucky
```

## Runbook Format

Runbooks should be action-oriented and copy-pasteable:

```markdown
# Runbook: [Service/Procedure Name]

**Last tested:** YYYY-MM-DD
**Owner:** @team-name

## When to Use
[Symptoms or alerts that trigger this runbook]

## Prerequisites
- [ ] Access to X
- [ ] Permissions for Y

## Steps

### 1. Verify the Issue
\```bash
kubectl get pods -n production | grep service-name
\```
Expected: [what you should see]

### 2. Apply Fix
\```bash
kubectl rollout restart deployment/service-name -n production
\```

### 3. Verify Resolution
\```bash
curl -s https://service/health | jq .status
\```
Expected: "healthy"

## Escalation
If steps above don't resolve:
1. Page secondary on-call
2. Check #incident-response
```
