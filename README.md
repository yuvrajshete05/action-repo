# action-repo

**GitHub Repository that Triggers Webhook Events**

This repository is configured to send webhook events to the webhook-repo whenever code is pushed, pull requests are created, or branches are merged. It acts as the **event source** for the webhook integration system.

---

## 📌 Overview

| Component | Role |
|-----------|------|
| **action-repo** | Sends GitHub events via webhooks |
| **webhook-repo** | Receives and processes webhook events |
| **MongoDB** | Stores event data |
| **Dashboard UI** | Displays events in real-time |

---

## 🚀 Getting Started

### Prerequisites

- Git installed
- GitHub account
- ngrok running (forwarding to webhook-repo)
- webhook-repo already set up

### 1. Clone the Repository

```bash
git clone https://github.com/YOUR-USERNAME/action-repo.git
cd action-repo
```

### 2. Configure GitHub Webhook

Go to your action-repo on GitHub:

1. Click **Settings** (top menu)
2. Select **Webhooks** (left sidebar)
3. Click **Add webhook**

Fill in these details:

| Field | Value |
|-------|-------|
| **Payload URL** | `https://your-ngrok-url.ngrok-free.dev/webhook` |
| **Content type** | `application/json` |
| **Secret** | Same as `GITHUB_WEBHOOK_SECRET` in webhook-repo `.env` |
| **Events** | Select: Push events, Pull requests |
| **Active** | ✓ Checked |

**Example Payload URL:**
```
https://jayse-zippy-nonestimably.ngrok-free.dev/webhook
```

### 3. Verify Webhook Setup

After saving:
1. Go to **Webhooks** page
2. Click your webhook
3. Check **Recent Deliveries** tab
4. You should see successful (green) delivery status

---

## 📤 Triggering Webhook Events

### PUSH Event (Commit and Push)

```bash
# Create a file
echo "# My Changes" > changes.txt

# Commit
git add changes.txt
git commit -m "Add changes"

# Push to GitHub
git push origin main
```

**Result:** Dashboard shows `PUSH` event from `main` branch

---

### PULL_REQUEST Event (Create Pull Request)

```bash
# Create new branch
git checkout -b feature/my-feature

# Make changes
echo "New feature" > feature.txt
git add feature.txt
git commit -m "Add new feature"

# Push branch
git push origin feature/my-feature
```

Then on GitHub:
1. Go to action-repo
2. Click **Pull requests** tab
3. Click **New pull request**
4. Set base: `main`, compare: `feature/my-feature`
5. Click **Create pull request**

**Result:** Dashboard shows `PULL_REQUEST` event

---

### MERGE Event (Merge Pull Request)

On GitHub Pull Request page:
1. Click **Merge pull request**
2. Confirm merge

**Result:** Dashboard shows `MERGE` event

---

## ✅ Testing the Full Flow

**Step 1: Push code to action-repo**
```bash
git push origin main
```

**Step 2: Check webhook delivery** (GitHub → Settings → Webhooks → Recent Deliveries)
- Status should be **200** (green checkmark)

**Step 3: View in dashboard** (http://127.0.0.1:5000/)
- Event appears in 15 seconds
- Shows: Author, Action, Branches, Timestamp

---

## 🔍 Troubleshooting

| Problem | Solution |
|---------|----------|
| Webhook showing 404 | ngrok URL expired - restart ngrok and update GitHub settings |
| Secret mismatch | Ensure `GITHUB_WEBHOOK_SECRET` in webhook-repo matches GitHub secret |
| No events in dashboard | Check webhook delivery logs in GitHub for errors |
| ngrok not forwarding | Restart: `ngrok http 5000` |
| Flask not running | Start: `cd webhook-repo && python app.py` |

---

## 📝 GitHub Webhook Data

The webhook sends this data structure:

```json
{
  "ref": "refs/heads/main",
  "before": "abc123...",
  "after": "def456...",
  "pusher": {
    "name": "username",
    "email": "user@example.com"
  },
  "repository": {
    "name": "action-repo",
    "full_name": "username/action-repo"
  }
}
```

This data is processed by webhook-repo and stored in MongoDB.

---

## 📊 Event Types Supported

- ✅ **Push** - Code pushed to branch
- ✅ **Pull Request** - PR created/updated
- ✅ **Merge** - Branch merged into main

---

## 🔗 Related Repositories

- [webhook-repo](https://github.com/YOUR-USERNAME/webhook-repo) - Receives and processes webhooks

---

## 📋 Notes

- Webhooks are **automatic** - no manual setup needed after initial configuration
- Each action triggers exactly **one webhook delivery**
- Events appear in dashboard within **15 seconds**
- All data is stored in MongoDB for history
- GitHub webhook delivery logs help with debugging

---

**Project Status:** ✅ Complete and tested
