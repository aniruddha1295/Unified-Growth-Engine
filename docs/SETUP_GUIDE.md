# Setup Guide — Unified Growth Engine

## Prerequisites

1. **Node.js** v18+ installed
2. **n8n** installed globally: `npm install -g n8n`
3. **Google account** for Gmail OAuth2
4. **Gemini API Key** from [Google AI Studio](https://aistudio.google.com/apikey)

---

## Step 1: Start n8n

```bash
n8n start
```

Open `http://localhost:5678` in your browser.

---

## Step 2: Get Gemini API Key

1. Go to [https://aistudio.google.com/apikey](https://aistudio.google.com/apikey)
2. Click **Create API Key**
3. Copy and save it — you'll need it in Step 5

---

## Step 3: Setup Gmail OAuth2

### Create Google Cloud Project
1. Go to [https://console.cloud.google.com/](https://console.cloud.google.com/)
2. Create a new project (e.g., "n8n-growth-engine")
3. Go to **APIs & Services** → **Library**
4. Search and enable **Gmail API**

### Configure OAuth Consent Screen
1. Go to **APIs & Services** → **OAuth consent screen**
2. Select **External** → Create
3. Fill in App name, support email
4. Add your email as a test user
5. Save

### Create OAuth Credentials
1. Go to **APIs & Services** → **Credentials**
2. Click **Create Credentials** → **OAuth Client ID**
3. Application type: **Web application**
4. Add Authorized redirect URI: `http://localhost:5678/rest/oauth2-credential/callback`
5. Copy **Client ID** and **Client Secret**

### Add Gmail Credential in n8n
1. In n8n, go to **Credentials** → **Add Credential**
2. Search for **Gmail OAuth2**
3. Paste Client ID and Client Secret
4. Click **Sign in with Google** → authorize
5. Save the credential

---

## Step 4: Import Workflows

Import in this order:

1. **FINAL_Lead_Workflow.json** — import first
2. **FINAL_Content_Workflow.json** — import second
3. **FINAL_Master_Workflow.json** — import last

For each: Go to **Workflows** → **Add Workflow** → **Import from File**

---

## Step 5: Configure API Keys

In each workflow, find the Gemini Code nodes and replace `PASTE_YOUR_GEMINI_API_KEY_HERE` with your actual API key:

### Master Workflow
- Click **"Gemini Business Analyzer"** node → find and replace API key in code
- Click **"Gemini Analyze Image"** node → find and replace API key in code

### Content Workflow
- Click **"Gemini Generate Content"** node → find and replace API key in code

---

## Step 6: Link Gmail Credentials

Click on each Gmail node and select your Gmail OAuth2 credential:

### Content Workflow
- **"Send Preview Email"** → select Gmail credential

### Lead Workflow
- **"Send Lead Preview"** → select Gmail credential
- **"Send Gmail to Lead"** → select Gmail credential

---

## Step 7: Publish All Workflows

For each workflow:
1. Click the toggle in the top right to **Publish/Activate**
2. Confirm it shows as Active

**Important**: Publish in this order:
1. Lead Workflow (must be active before Content can trigger it)
2. Content Workflow (must be active before Master can trigger it)
3. Master Workflow (the entry point)

---

## Step 8: Test

1. Open the **Master Workflow**
2. Click on the **"Growth Engine Form"** node
3. Copy the **Production URL** (not Test URL)
4. Open it in your browser
5. Fill in the form:
   - Input Type: **Text**
   - Business Input: *"We help CA firms automate tax compliance and save 20 hours per month"*
   - Lead 1: Your name, your email, a company name
6. Click **Launch Growth Engine**
7. Check your Gmail for the content preview
8. Click **APPROVE ALL**
9. Check Gmail for the lead outreach preview
10. Click **SEND ALL EMAILS**
11. Done! 🎉

---

## Troubleshooting

### "JSON parameter needs to be valid JSON"
- You're running an old workflow version. Make sure you imported the FINAL versions where Gemini calls use Code nodes.

### Gemini 429 Rate Limit Error
- Free tier: 5 requests/minute, 20 requests/day
- Wait 2 minutes and try again
- Or upgrade to a paid Gemini plan

### Gmail "Precondition check failed"
- Re-authorize Gmail OAuth2 credential in n8n
- Make sure your email is added as a test user in Google Cloud Console

### Form returns 404
- Make sure the Master Workflow is Published/Active
- Use the Production URL, not the Test URL

### Content Workflow not triggering
- Make sure Content Workflow is Published/Active
- Check that the webhook path is exactly `content-workflow`

### Lead emails not sending
- Make sure Lead Workflow is Published/Active
- Check Gmail credential is linked to "Send Gmail to Lead" node
