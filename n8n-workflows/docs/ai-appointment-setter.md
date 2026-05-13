# 🗓️ AI Appointment Setter

**Status:** ⚠️ Unstable (under active development)  
**Trigger:** Telegram message  
**File:** [`AI_Appointment_Setter.json`](../workflows/AI_Appointment_Setter.json)

---

## Overview

A conversational AI agent that lets clients book appointments directly from **Telegram**. The agent collects their info, checks Google Calendar for availability, handles scheduling conflicts, and confirms the booking — all in natural language.

---

## Workflow Diagram

```
Telegram Message
      │
      ▼
  AI Agent (GPT-4.1-mini)
  ├── Simple Memory (20-turn context window)
  ├── [Tool] Check Google Calendar availability
  └── [Tool] Create Google Calendar event
      │
      ▼
  Send reply to Slack (#all-n8n-marketing-team)
```

---

## How It Works

1. **User sends a message** on Telegram
2. The **AI Agent** greets the user and collects:
   - Full name
   - Email address
   - Desired date & time
3. The agent **checks Google Calendar** for the requested slot
4. **If available** → confirms with the user → creates the event
5. **If unavailable** → suggests the next open slot (1 hour after existing booking) → waits for user confirmation
6. A **Slack notification** is sent after booking

### Business Rules
- Business hours: **8:00 AM – 5:00 PM**
- Conflict resolution: suggest next slot **+1 hour** after the existing appointment
- Memory: retains last **20 messages** per Telegram chat

---

## Required Credentials

| Credential | Node |
|---|---|
| OpenAI API | OpenAI Chat Model |
| Google Calendar OAuth2 | Get availability · Create event |
| Telegram Bot Token | Telegram Trigger |
| Slack OAuth2 | Send a message |

---

## Setup

1. Import the workflow JSON into n8n
2. Connect your **OpenAI** credential (or swap to another LLM)
3. Connect **Google Calendar OAuth2** — update the calendar email in both calendar tool nodes
4. Connect your **Telegram Bot** credential
5. Connect **Slack OAuth2** — update the channel ID in the Slack node
6. Activate the workflow

### Swap the Calendar Email

In both Google Calendar nodes, replace `dexdecierdo@gmail.com` with your own calendar:

```
"value": "your-email@gmail.com"
```

---

## Known Issues / TODO

- [ ] Slack node currently sends a hardcoded `"NEw"` message — needs to be updated to send the AI agent's actual response
- [ ] No email notification sent to the client after booking
- [ ] Business hours not enforced at the tool level (relies on AI prompt)
- [ ] Labeled `[unstable]` — test thoroughly before production use
