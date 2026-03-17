# InsightForce

**Agentforce-powered Slack knowledge base for the FDE team.**

InsightForce connects Salesforce Agentforce to Slack — ingesting messages, threads, and channel history to automatically surface, categorize, and maintain a living wiki of FDE team knowledge.

---

## What It Does

- Agentforce agent reads and indexes Slack channels, threads, and DMs
- Automatically categorizes insights, product feedback, corrections of error, and trends
- Stores structured knowledge in a custom Salesforce data model
- FDE team can query the agent in natural language to retrieve institutional knowledge
- Executive digest generation on demand or scheduled

---

## Architecture

```
Slack (Channels / Threads / DMs)
        ↓
Slack Bot (OAuth / Bot Token)
        ↓
External Service (OAS 3.0 spec → Salesforce)
        ↓
Named Credential + External Credential
        ↓
Agentforce Agent (Agent Builder)
        ↓
Agent Actions → Apex Invocable Actions
        ↓
Custom Data Model (Insight__c, Trend__c, etc.)
        ↓
FDE Team Query Interface
```

---

## Project Structure

```
insightforce/
├── force-app/main/default/
│   ├── classes/                    # Apex: Slack API, categorization, digest
│   ├── objects/                    # Custom objects: Insight__c, Trend__c, etc.
│   ├── permissionsets/             # Permission sets
│   ├── flows/                      # Auto-categorization flows
│   ├── prompts/                    # Agentforce prompt templates
│   ├── externalServices/           # OAS 3.0 Slack API spec
│   ├── namedCredentials/           # Named + External credentials
│   ├── customMetadata/             # Config metadata
│   └── lwc/insightforceDashboard/  # LWC dashboard component
├── slack/                          # Slack app manifest and config
├── scripts/                        # Deploy and data scripts
├── config/                         # Scratch org config
└── docs/                           # Architecture and setup docs
```

---

## Setup

See [`docs/SETUP.md`](docs/SETUP.md) for full deployment instructions.

---

## Data Model

| Object | Purpose |
|---|---|
| `Insight__c` | Core unit of knowledge captured from Slack |
| `Contributor__c` | Slack users who contributed insights |
| `Trend__c` | Recurring themes detected across insights |
| `Product_Feature_Request__c` | Feature requests extracted from Slack |
| `Correction_of_Error__c` | Documented mistakes and lessons learned |
| `Insight_Reference__c` | Junction: links insights to related records |

---

## Slack Permissions Required

- `channels:history`
- `channels:read`
- `groups:history`
- `groups:read`
- `im:history`
- `search:read`
- `users:read`
- `chat:write`
