# 📋 Lead Capture v1

**Status:** ✅ Stable  
**Trigger:** Webhook (POST)  
**File:** [`Lead_Capture_v1___Stable.json`](../workflows/Lead_Capture_v1___Stable.json)

---

## Overview

A webhook-triggered lead intake pipeline with AI-powered processing, duplicate detection, and Slack notifications. Incoming leads are validated, deduplicated against a Google Sheet, and appended if new.

---

## Workflow Diagram

```
Webhook (POST)
      │
      ▼
  Edit Fields (normalize)
      │
      ▼
    If (valid?)
   /          \
  No           Yes
  │             │
error_msg    AI Agent (GPT-4.1-mini + Memory)
              │
              ▼
         checkEmailDup (Google Sheets lookup)
              │
         ┌────┴────┐
      Duplicate   New
         │         │
      Update     Append
       Row        Row
         └────┬────┘
              ▼
    Send a message in Slack
```

---

## How It Works

1. A **POST request** hits the webhook with lead data
2. Fields are **normalized** via Edit Fields
3. An **If node** validates the payload — invalid data routes to `Stop and Error`
4. The **AI Agent** processes and enriches the lead
5. **Duplicate check** queries Google Sheets by email
6. **New leads** are appended; **duplicates** update the existing row
7. A **Slack message** is sent either way

---

## Required Credentials

| Credential | Node |
|---|---|
| OpenAI API | OpenAI Chat Model |
| Google Sheets OAuth2 | checkEmailDup · Update Row · Append |
| Slack OAuth2 | Send a message in Slack |

---

## Setup

1. Import the workflow JSON
2. Configure **Google Sheets** OAuth2 — update the Sheet ID in the Sheets nodes
3. Set up your **OpenAI** credential
4. Connect **Slack OAuth2** and update the channel
5. Copy the **webhook URL** from the Webhook node
6. Send a test POST with your lead payload
7. Activate the workflow

### Example Webhook Payload

```json
{
  "name": "Juan dela Cruz",
  "email": "juan@example.com",
  "phone": "+639171234567",
  "source": "Facebook Ad"
}
```

---

## Notes

- The AI Agent uses a **20-message context window** (Simple Memory)
- Error handling routes to `Stop and Error` with a custom `error_msg`
- Designed to be extended — add email notifications or CRM nodes after the Slack step
