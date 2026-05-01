# 🚀 Unified Growth Engine

> AI-powered marketing content generation and lead outreach automation built with n8n + Gemini API

![n8n](https://img.shields.io/badge/n8n-2.7.5-orange) ![Gemini](https://img.shields.io/badge/Gemini_2.5-Flash-blue) ![License](https://img.shields.io/badge/license-MIT-green)

---

## 📋 Overview

The **Unified Growth Engine** is a fully automated marketing pipeline that takes any business input (text, URL, or image), generates multi-platform marketing content using Google's Gemini AI, and sends personalized outreach emails to target leads — all through a simple web form.

### What It Does

1. **You fill a form** → describe your business, add target leads
2. **AI analyzes** → Gemini extracts value proposition, ideal customer profile, themes
3. **Content generated** → LinkedIn posts, X tweets, Instagram captions, email drafts, image prompts
4. **You review & approve** → preview email with all content variations
5. **Leads contacted** → personalized emails sent to each lead automatically

### Built For

- Startups needing quick marketing content
- Freelancers managing multiple client campaigns
- Small businesses without a marketing team
- Anyone who wants AI-powered outreach at scale

---

## 🏗️ Architecture

```
┌─────────────────────────────────────────────────────────┐
│                    USER (Browser)                         │
│              Fills n8n Form & Submits                     │
└──────────────────────┬──────────────────────────────────┘
                       │
                       ▼
┌─────────────────────────────────────────────────────────┐
│              MASTER WORKFLOW (n8n)                        │
│                                                          │
│  ┌──────────┐   ┌───────────┐   ┌────────────────────┐  │
│  │ n8n Form │──▶│ Parse &   │──▶│ Switch Input Type  │  │
│  │ Trigger  │   │ Extract   │   │ (Text/URL/Image)   │  │
│  └──────────┘   └───────────┘   └────────┬───────────┘  │
│                                           │              │
│                    ┌──────────────────────┼──────┐       │
│                    ▼                      ▼      ▼       │
│              ┌──────────┐          ┌─────────┐ ┌──────┐  │
│              │ Normalize│          │Fetch URL│ │Gemini│  │
│              │   Text   │          │& Extract│ │Vision│  │
│              └────┬─────┘          └────┬────┘ └──┬───┘  │
│                   │                     │         │      │
│                   └──────────┬──────────┘─────────┘      │
│                              ▼                           │
│                   ┌────────────────────┐                 │
│                   │ Gemini Business    │                 │
│                   │ Analyzer (AI)      │                 │
│                   └────────┬───────────┘                 │
│                            │ Triggers                    │
└────────────────────────────┼────────────────────────────┘
                             ▼
┌─────────────────────────────────────────────────────────┐
│             CONTENT WORKFLOW (n8n)                        │
│                                                          │
│  ┌──────────┐   ┌────────────────┐   ┌───────────────┐  │
│  │ Webhook  │──▶│ Gemini Content │──▶│ Build Preview │  │
│  │ Receive  │   │ Generator (AI) │   │ Email (HTML)  │  │
│  └──────────┘   └────────────────┘   └──────┬────────┘  │
│                                              │           │
│  Generated Content:                          ▼           │
│  • 3 LinkedIn posts            ┌──────────────────────┐  │
│  • 2 X/Twitter tweets          │  Send Preview Email  │  │
│  • 2 Instagram captions        │  (Gmail OAuth2)      │  │
│  • 2 Email drafts              └──────────┬───────────┘  │
│  • 2 Image prompts                        │              │
│                                           ▼              │
│                              ┌──────────────────────┐    │
│                              │  Wait For Approval   │    │
│                              │  (Email buttons)     │    │
│                              └──────────┬───────────┘    │
│                                         │ APPROVE        │
│                                         ▼                │
│                              ┌──────────────────────┐    │
│                              │ Trigger Lead Workflow │    │
└──────────────────────────────┼──────────────────────────┘
                               ▼
┌─────────────────────────────────────────────────────────┐
│              LEAD WORKFLOW (n8n)                          │
│                                                          │
│  ┌──────────┐   ┌──────────────┐   ┌─────────────────┐  │
│  │ Webhook  │──▶│ Personalize  │──▶│ Send Lead       │  │
│  │ Receive  │   │ Emails (AI   │   │ Preview Email   │  │
│  │          │   │ content)     │   │ (Gmail OAuth2)  │  │
│  └──────────┘   └──────────────┘   └────────┬────────┘  │
│                                              │           │
│  For each lead:                              ▼           │
│  • Replace {{name}}          ┌──────────────────────┐    │
│  • Replace {{company}}       │  Wait For Send       │    │
│  • Apply AI email draft      │  Approval            │    │
│                              └──────────┬───────────┘    │
│                                         │ SEND ALL       │
│                                         ▼                │
│                              ┌──────────────────────┐    │
│                              │ Send Gmail to Each   │    │
│                              │ Lead Individually    │    │
│                              └──────────────────────┘    │
└─────────────────────────────────────────────────────────┘
```

---

## 🛠️ Tech Stack

| Component | Technology |
|-----------|-----------|
| Workflow Engine | n8n (Self-hosted, v2.7.5) |
| AI Model | Google Gemini 2.5 Flash |
| Email | Gmail API (OAuth2) |
| Form | n8n Built-in Form Trigger |
| Language | JavaScript (n8n Code Nodes) |
| API Protocol | REST (HTTP/JSON) |

---

## 📦 Project Structure

```
unified-growth-engine/
├── workflows/
│   ├── FINAL_Master_Workflow.json      # Form + Business Analysis
│   ├── FINAL_Content_Workflow.json     # AI Content Generation
│   └── FINAL_Lead_Workflow.json        # Lead Outreach
├── docs/
│   ├── SETUP_GUIDE.md                  # Step-by-step setup
│   ├── ARCHITECTURE.md                 # Technical deep dive
│   └── screenshots/                    # UI screenshots
├── README.md                           # This file
└── LICENSE
```

---

## ⚡ Quick Start

### Prerequisites

- [Node.js](https://nodejs.org/) v18+
- [n8n](https://n8n.io/) v2.7+ (Self-hosted)
- Google Cloud Account (for Gmail OAuth2)
- Gemini API Key ([Get one free](https://aistudio.google.com/apikey))

### Installation

**1. Install n8n**
```bash
npm install -g n8n
n8n start
```

**2. Import Workflows**

Open `http://localhost:5678` and import all 3 JSON files from the `workflows/` folder:
- `FINAL_Lead_Workflow.json` (import first)
- `FINAL_Content_Workflow.json`
- `FINAL_Master_Workflow.json`

**3. Configure Gemini API Key**

In each workflow, find the Code nodes that call Gemini and replace `PASTE_YOUR_GEMINI_API_KEY_HERE` with your actual key:
- Master: "Gemini Business Analyzer" and "Gemini Analyze Image" nodes
- Content: "Gemini Generate Content" node

**4. Configure Gmail OAuth2**

1. Go to [Google Cloud Console](https://console.cloud.google.com/)
2. Create a project, enable Gmail API
3. Create OAuth2 credentials (Web application)
4. Add redirect URI: `http://localhost:5678/rest/oauth2-credential/callback`
5. In n8n, create Gmail OAuth2 credential with Client ID & Secret
6. Link to all Gmail nodes in Content and Lead workflows

**5. Publish & Launch**

1. Publish all 3 workflows
2. Open the Form URL from Master Workflow's "Growth Engine Form" node
3. Fill in business details + leads → Submit
4. Check email for preview → Approve → Leads get contacted!

---

## 🎯 Features

### Input Flexibility
- **Text**: Describe your business in plain text
- **URL**: Paste a website — AI scrapes and analyzes it
- **Image**: Paste an image URL or describe it — Gemini Vision analyzes it

### Multi-Platform Content Generation
- **LinkedIn**: 3 professional posts with hooks and CTAs
- **X/Twitter**: 2 tweets under 280 chars with hashtags
- **Instagram**: 2 captions with 5-10 relevant hashtags
- **Email**: 2 drafts with compelling subjects and CTAs
- **Image Prompts**: 2 detailed prompts for AI image generation

### Human-in-the-Loop Approval
- Preview ALL generated content via email before anything is sent
- Two-step approval: content approval → lead outreach approval
- Reject and regenerate at any stage

### Personalized Lead Outreach
- AI-generated email content with {{name}} and {{company}} placeholders
- Automatic personalization for each lead
- Individual Gmail sends (not bulk/BCC)

---

## 📊 How It Works (Detailed)

### Step 1: Business Analysis
The Master Workflow accepts form input and routes it based on type:
- **Text** → Direct pass-through
- **URL** → HTTP fetch → Strip HTML → Extract text (max 3000 chars)
- **Image** → Gemini Vision API → Text description

All paths converge at the **Gemini Business Analyzer** which extracts:
```json
{
  "value_proposition": "one sentence core value",
  "icp": "ideal customer profile description",
  "content_themes": "theme1, theme2, theme3"
}
```

### Step 2: Content Generation
Gemini 2.5 Flash generates marketing content with specific constraints:
- LinkedIn: Professional tone, 150-300 words, hook + CTA
- X: Punchy, under 280 chars, 2-3 hashtags
- Email: Includes {{name}} and {{company}} placeholders for personalization

### Step 3: Approval Flow
- HTML preview email sent via Gmail OAuth2
- Contains all content variations organized by platform
- APPROVE ALL / REJECT buttons using n8n's `$execution.resumeUrl`

### Step 4: Lead Outreach
- First email draft from AI content used as template
- {{name}}, {{company}} replaced for each lead
- Preview email sent for final approval
- On approval: individual Gmail sent to each lead

---

## 🔐 Security Notes

- **API keys** are stored in n8n Code nodes (replace before deploying)
- **Gmail OAuth2** tokens managed by n8n credential system
- **No data storage** — all data flows through and isn't persisted
- **Human approval required** — nothing sends without your click

---

## ⚠️ Known Limitations

1. **Gemini Free Tier Rate Limits**: 5 RPM, 20 RPD for Gemini 2.5 Flash
2. **Image Input**: Supports image URLs and text descriptions (not direct file upload)
3. **Max 3 Leads**: Form supports up to 3 leads per submission
4. **Gmail OAuth**: Requires Google Cloud project setup with test users
5. **Local Only**: Runs on localhost (deploy to cloud for production)

---

## 🗺️ Roadmap

- [ ] Image generation using Imagen 4 / Nano Banana
- [ ] Direct image file upload via form
- [ ] CRM integration (HubSpot, Salesforce)
- [ ] Google Sheets lead import
- [ ] Content scheduling (Buffer, Hootsuite API)
- [ ] A/B testing for email subject lines
- [ ] Analytics dashboard for outreach performance

---

## 🧪 Testing

### Test Text Input
Fill the form with:
- Input Type: **Text**
- Business Input: *"We help CA firms automate tax compliance and save 20 hours per month. Our platform handles GST filing, TDS returns, and audit preparation automatically."*
- Lead 1: Your test email

### Test URL Input
- Input Type: **URL**
- Business Input: *https://www.freshbooks.com*

### PowerShell Direct Test
```powershell
Invoke-RestMethod -Uri "http://localhost:5678/webhook/content-workflow" -Method POST -ContentType "application/json" -Body '{"value_proposition":"Automate tax compliance","icp":"CA firms","normalized_content":"Tax automation platform","content_themes":"automation, compliance","leads":[{"name":"Test","email":"you@email.com","company":"TestCo"}]}'
```

---

