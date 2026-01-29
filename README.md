# action-repo

This is the GitHub repository that **sends webhook events** to the webhook-repo.

## What This Repository Does

- **Tracks GitHub actions**: Every push, pull request, and merge triggers a webhook
- **Sends events**: Automatically sends event details to the webhook receiver
- **Integration point**: This is where developers make commits and pull requests

## Setup Instructions

### 1. Create This Repository

```bash
1. Go to GitHub
2. Click "New Repository"
3. Name it: action-repo
4. Initialize with README (recommended)
5. Click "Create repository"
```

### 2. Clone to Your Machine

```bash
git clone https://github.com/YOUR-USERNAME/action-repo.git
cd action-repo
```

### 3. Configure Webhook

**In GitHub (on action-repo):**

1. Go to **Settings** → **Webhooks** → **Add webhook**
2. **Payload URL**: `https://your-ngrok-url.ngrok.io/webhook`
   - Replace `your-ngrok-url` with your actual ngrok URL
   - Example: `https://abc123.ngrok.io/webhook`
3. **Content type**: `application/json`
4. **Secret**: Enter the SAME secret as in webhook-repo's `.env`
   - This prevents unauthorized access
5. **Events to trigger**: Select these events:
   - ✓ Push events
   - ✓ Pull requests
6. Click **Add webhook**

### 4. Test the Webhook

Make some changes and push to test:

```bash
# Create a test file
echo "# Test" > test.txt

# Add, commit, and push
git add test.txt
git commit -m "Test push event"
git push origin main
```

**Then check:**
1. Go to action-repo Settings → Webhooks → Your webhook → Recent Deliveries
2. You should see a successful delivery (green checkmark)
3. Check the webhook-repo dashboard to see the PUSH event

## Triggering Different Events

### PUSH Event
```bash
git add .
git commit -m "Some change"
git push origin main
```
Dashboard will show: `PUSH → main`

### PULL_REQUEST Event
```bash
# Create new branch
git checkout -b feature/new-feature

# Make changes and push
echo "New feature" > feature.txt
git add feature.txt
git commit -m "Add new feature"
git push origin feature/new-feature

# Go to GitHub and create a Pull Request
# Then check dashboard for PULL_REQUEST event
```
Dashboard will show: `PULL_REQUEST → feature/new-feature → main`

### MERGE Event
```bash
# After creating a PR, merge it on GitHub
# Click "Merge pull request" on the PR page

# Or merge via git
git checkout main
git merge feature/new-feature
git push origin main
```
Dashboard will show: `MERGE → feature/new-feature → main`

## Notes

- Keep this repository simple for testing
- The webhook is automatically triggered - no manual action needed
- Check webhook delivery logs in GitHub if events don't appear in dashboard
- Make sure ngrok is running before pushing code
- The webhook secret must match in both webhook-repo `.env` and GitHub settings

## Files You Can Add

Add any files to test:
```bash
# Create a file
echo "Some content" > file.txt

# Or modify existing README
echo "## New Section" >> README.md

# Commit and push
git add .
git commit -m "Update files"
git push origin main
```

Each push = One PUSH event in the dashboard! 🚀
