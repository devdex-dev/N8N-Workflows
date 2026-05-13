# 🏠 Real Estate Lead Capture v1.2

**Status:** ✅ Stable  
**Trigger:** Webhook (POST)  
**File:** [`real_estate_lead_capture_workflow.json`](../workflows/real_estate_lead_capture_workflow.json)  
**Compatibility:** Free-tier n8n compatible

---

## Overview

A robust real estate lead intake pipeline built for free-tier n8n. Handles validation, normalization, deduplication, structured logging in Google Sheets, and Slack alerts for new leads, duplicates, and errors.

---

## Workflow Diagram

```
Webhook — Lead Intake
      │
      ▼
  Respond 200 OK  ←──── (immediate response to caller)
      │
      ▼
  ⚙️ Config (centralized settings)
      │
      ▼
  Validate & Normalize
      │
      ▼
   Is Valid?
   /        \
  No         Yes
  │           │
Slack —    Google Sheets — Read All
Validation      │
Error        Deduplication Check
                 │
           Is Duplicate?
           /           \
          Yes            No
          │               │
  Google Sheets —   Google Sheets —
  Log Duplicate     Append New Lead
          │               │
  Slack —           Slack —
  Duplicate         New Lead
  Alert             Alert
          │               │
          └──────┬────────┘
                 │
           Error Logger
```

---

## How It Works

1. Lead data arrives via **webhook POST**
2. **200 OK** is sent immediately (non-blocking)
3. A centralized **Config node** holds reusable settings (sheet IDs, channel names, etc.)
4. The lead is **validated and normalized** (phone format, required fields, etc.)
5. Invalid leads trigger a **Slack validation error alert**
6. Valid leads are checked against **Google Sheets** for duplicates by email
7. **New leads** are appended to the main sheet and trigger a Slack new-lead alert
8. **Duplicates** are logged to a separate sheet and trigger a duplicate alert
9. An **Error Logger** node captures any unexpected failures

---

## Required Credentials

| Credential | Node |
|---|---|
| Google Sheets OAuth2 | Read All · Append New Lead · Log Duplicate |
| Slack OAuth2 | New Lead Alert · Duplicate Alert · Validation Error |

---

## Setup

1. Import the workflow JSON
2. Open the **⚙️ Config** node and update:
   - Google Sheets ID (main leads sheet)
   - Google Sheets ID (duplicates log sheet)
   - Slack channel name/ID
3. Connect **Google Sheets OAuth2** and **Slack OAuth2** credentials
4. Copy the **Webhook URL** from the Webhook node
5. Test with a sample lead payload
6. Activate

### Example Payload

```json
{
  "name": "Maria Santos",
  "email": "maria@email.com",
  "phone": "09171234567",
  "property_interest": "2BR Condo",
  "budget": "5000000",
  "location": "Makati"
}
```

---

## Why Free-Tier Compatible?

- No AI/LLM nodes (no OpenAI costs)
- Uses only Google Sheets as a datastore (no external DB)
- Minimal node count — stays within execution limits
- Immediate 200 response avoids webhook timeout issues

---

## Notes

- Extend this by adding an email notification node after "Append New Lead"
- The **⚙️ Config** node is the single source of truth — update settings there only
- Error Logger can be connected to the Error Trigger workflow for centralized monitoring
