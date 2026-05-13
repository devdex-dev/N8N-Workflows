# 🚨 Error Trigger

**Status:** ✅ Stable · Active  
**Trigger:** n8n Error Trigger (global)  
**File:** [`Error-Trigger.json`](../workflows/Error-Trigger.json)

---

## Overview

A global error handler for your n8n instance. When **any workflow fails**, this workflow fires automatically, formats a detailed error message, and sends it to you via **Telegram** — including the workflow name, error message, severity level, and a direct link to the failed execution.

---

## Workflow Diagram

```
Error Trigger (any workflow fails)
      │
      ▼
  Set message (format error details)
      │
      ▼
  Telegram (send alert to your chat)
```

---

## Example Alert

```
⚠️ `N8N-WORKFLOW-BACKUP` Workflow failed to run!⚠️

Error Message: Request failed with status code 404 | File not found: 1tPxTxrwl1Jz1p2KcpvhtoUi41CoR84eu.
Level: warning

See More → https://your-n8n.com/workflow/xxx/executions/50
```

---

## How It Works

1. n8n's built-in **Error Trigger** fires when any workflow throws an unhandled error
2. The **Set message** node formats a Telegram-friendly alert with:
   - Workflow name
   - Error message (HTTP status + detailed API error if present)
   - Error level (warning / error)
   - Direct link to the execution for debugging
3. The message is sent to your **Telegram chat**

---

## Required Credentials

| Credential | Node |
|---|---|
| Telegram Bot Token | Telegram |

---

## Setup

1. Import the workflow JSON
2. Connect your **Telegram Bot** credential
3. Update the `chatId` in the Telegram node to your own Telegram user or group ID:
   ```
   "chatId": "YOUR_TELEGRAM_CHAT_ID"
   ```
4. **Activate the workflow** — this is critical, it must be active to catch errors
5. To attach it to other workflows: open any workflow → **Settings → Error Workflow** → select `Error-Trigger`

### Getting Your Telegram Chat ID
1. Message `@userinfobot` on Telegram
2. It will reply with your chat ID
3. Paste that ID into the Telegram node

---

## Attaching to Other Workflows

For each workflow you want monitored:

1. Open the workflow
2. Go to **⋮ → Settings**
3. Under **Error Workflow**, select **Error-Trigger**
4. Save

Or set it as the **global error workflow** in n8n instance settings so it catches all workflows by default.

---

## Notes

- The message template uses `$json.execution.error.messages[0]` for the primary error and `$json.execution.error.context.data.error.message` for API-level detail — if either is missing, that portion will be blank
- The workflow is set to `"callerPolicy": "workflowsFromSameOwner"` — it only handles errors from workflows owned by the same account
- Keep this workflow **always active** — it cannot catch its own errors
