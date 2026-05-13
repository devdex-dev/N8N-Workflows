# 🤖 n8n Automation Workflows

A collection of production-ready **n8n** automation workflows covering AI agents, lead capture, calendar scheduling, data pipelines, and system monitoring.

> Built and maintained by **DEX D** · Philippines 🇵🇭

---

## 📦 Workflows

| Workflow | Description | Integrations | Status |
|---|---|---|---|
| [AI Appointment Setter](./workflows/AI_Appointment_Setter.json) | Telegram-based AI agent that books appointments via Google Calendar | Telegram · OpenAI · Google Calendar · Slack | ⚠️ Unstable |
| [Lead Capture v1](./workflows/Lead_Capture_v1___Stable.json) | Webhook-triggered lead intake with deduplication and Slack alerts | Webhook · OpenAI · Google Sheets · Slack | ✅ Stable |
| [Real Estate Lead Capture](./workflows/real_estate_lead_capture_workflow.json) | Free-tier compatible real estate lead pipeline with validation and logging | Webhook · Google Sheets · Slack | ✅ Stable |
| [Retrieval-Augmented Generation](./workflows/Retrieval-Augmeted_Generation.json) | RAG pipeline — watches Google Drive for file events and syncs vectors into Supabase | Google Drive · Supabase · OpenAI Embeddings | ✅ Stable |
| [N8N Workflow Backup](./workflows/N8N-WORKFLOW-BACKUP.json) | Scheduled backup of n8n workflows exported to Google Drive | Google Drive · Schedule Trigger | ✅ Stable |
| [Error Trigger](./workflows/Error-Trigger.json) | Global error handler — sends Telegram alerts when any workflow fails | Error Trigger · Telegram | ✅ Stable |

---

## 🚀 Getting Started

### Prerequisites
- [n8n](https://n8n.io) — self-hosted or cloud (n8n.cloud)
- Node.js 18+ (for self-hosted)
- Credentials for the relevant services (see each workflow's docs)

### Import a Workflow

1. Open your n8n instance
2. Go to **Workflows → Import from file**
3. Select the `.json` file from the `/workflows` folder
4. Configure your credentials in each node
5. Activate the workflow

---

## 📁 Repository Structure

```
n8n-workflows/
├── workflows/          # All workflow JSON files
├── docs/               # Per-workflow documentation
│   ├── ai-appointment-setter.md
│   ├── lead-capture.md
│   ├── real-estate-lead-capture.md
│   ├── rag-pipeline.md
│   ├── workflow-backup.md
│   └── error-trigger.md
├── .github/
│   └── CONTRIBUTING.md
└── README.md
```

---

## 🔧 Credential Setup

Each workflow requires its own credentials. A quick reference:

| Service | Used In |
|---|---|
| OpenAI API | AI Appointment Setter, Lead Capture, RAG |
| Google Calendar OAuth2 | AI Appointment Setter |
| Google Sheets OAuth2 | Lead Capture, Real Estate Lead Capture |
| Google Drive OAuth2 | RAG Pipeline, Workflow Backup |
| Supabase | RAG Pipeline |
| Telegram Bot | AI Appointment Setter, Error Trigger |
| Slack OAuth2 | AI Appointment Setter, Lead Capture, Real Estate Lead Capture |

> **Tip:** Set up credentials once in n8n and reuse them across workflows.

---

## ⚠️ Disclaimer

These workflows are provided as-is for learning and personal use. Always review and test in a staging environment before deploying to production. Remove or replace any hardcoded IDs, calendar emails, or chat IDs before sharing.

---

## 📄 License

[MIT](./LICENSE)

---

<p align="center">Made with ☕ and n8n</p>
