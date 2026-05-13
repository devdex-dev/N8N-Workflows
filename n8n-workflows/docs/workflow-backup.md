# 💾 N8N Workflow Backup

**Status:** ✅ Stable  
**Trigger:** Schedule (automated)  
**File:** [`N8N-WORKFLOW-BACKUP.json`](../workflows/N8N-WORKFLOW-BACKUP.json)

---

## Overview

Automatically exports all n8n workflows and saves them as JSON files to a **Google Drive** folder on a schedule. Keeps backups clean by creating a fresh folder each run and deleting the old one.

---

## Workflow Diagram

```
Schedule Trigger
      │
      ▼
  n8n (fetch all workflows via API)
      │
      ▼
  Loop Over Items
      │
      ├── Filter (skip inactive/system workflows)
      │
      ├── Convert to File (JSON export)
      │
      ├── create new folder (Google Drive)
      │
      └── Google Drive (upload file)
           │
           ▼
      Get folders (list existing backup folders)
           │
           ▼
         If (is old folder?)
           │
           ▼
      delete folder (remove old backup)
```

---

## How It Works

1. **Schedule Trigger** fires on your configured interval (daily recommended)
2. The n8n API is called to **fetch all workflows**
3. Each workflow loops through:
   - **Filtered** to skip unwanted items
   - **Converted** to a JSON file
   - **Uploaded** to a dated folder on Google Drive
4. After upload, old backup folders are **listed and deleted** to prevent clutter

---

## Required Credentials

| Credential | Node |
|---|---|
| n8n API | n8n node (self-hosted API key) |
| Google Drive OAuth2 | create new folder · Google Drive upload · Get folders · delete folder |

---

## Setup

1. Import the workflow JSON
2. Connect **Google Drive OAuth2**
3. Set up your **n8n API** credential:
   - In n8n: go to **Settings → API → Create API Key**
   - Use the key in the n8n API credential
4. Update the **Google Drive** nodes to point to your target backup folder
5. Set your preferred **schedule** in the Schedule Trigger node (e.g., daily at 2 AM)
6. Test with a manual execution first
7. Activate

---

## Recommended Schedule

| Frequency | Use Case |
|---|---|
| Daily | Active development |
| Weekly | Stable/production setups |
| Before major changes | Manual trigger |

---

## Notes

- The `delete folder` node has a known 404 error when a previous folder doesn't exist on the first run — the Error Trigger workflow will catch and notify you
- Consider keeping 7 days of backups by adjusting the `If` filter logic
- Restore by importing the JSON files back into n8n via **Workflows → Import**
