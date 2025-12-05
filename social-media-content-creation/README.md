# 📝 Social Media Content Creation Agent

An n8n workflow for **automated, multi-platform social media content creation**.  
This agent helps marketing teams quickly generate platform-optimized posts for Facebook, Twitter/X, LinkedIn, Reddit, and Meta Threads.

---

## 📌 Overview

This workflow turns a single topic or document (Google Docs or PDF) into actionable content for multiple social platforms.  
It uses a **CMO Agent** to coordinate platform-specific AI sub-agents, ensuring tone and call-to-actions are consistent across all posts.

---

## 🚀 Features

- Accepts prompts, Google Docs links, or uploaded PDFs.
- Generates platform-specific posts for:
  - Facebook
  - X/Twitter
  - LinkedIn
  - Reddit
  - Meta Threads
- Automatically creates and shares a Google Doc with the generated content.
- Sends the generated content back via Telegram.

---

## 🗂️ Directory Structure

agents/social-media-content-creation/
├── README.md # This file
├── workflow.json # n8n workflow export
├── config.yaml # Optional configuration (no secrets)
├── screenshots/ # Screenshots of the workflow
├── examples/ # Sample data and usage scenarios
└── docs/ # Additional documentation

yaml
Copy code

---

## 🛠️ How It Works

1. **Input:**  
   - Send a topic or upload a document (PDF or Google Docs link) via Telegram.
2. **Processing:**  
   - The **Manager Agent** consolidates the input into one actionable prompt.
   - The **CMO Agent** dispatches the prompt to platform-specific sub-agents:
     - Facebook Social Media Specialist
     - Twitter/X Social Media Specialist
     - LinkedIn Social Media Specialist
     - Reddit Social Media Specialist
     - Meta Threads Social Media Specialist
3. **Output:**  
   - Generated posts are combined and inserted into a Google Doc.
   - The Google Doc link is sent back to you in Telegram.

---

## 🔑 Requirements

- **n8n (latest version)** installed locally or on server.
- API credentials configured inside n8n:
  - Telegram Bot API
  - Google Drive & Google Docs APIs
  - OpenRouter API (for LLM)
  - (Optional) Google Gemini API for embeddings
- No secrets or API keys are stored in this workflow export.  
  You must configure credentials within your n8n instance.

---

## ⚙️ Setup Instructions

1. Import `workflow.json` into your n8n instance.
2. Configure the following credentials in n8n:
   - **Telegram API** — create a bot in @BotFather.
   - **Google Drive OAuth2** and **Google Docs OAuth2** — enable APIs in Google Cloud Console.
   - **OpenRouter API** — create an account at OpenRouter.
   - **Google Gemini API** (optional for embeddings).
3. Update the `config.yaml` (if used) with your desired defaults (no secrets).
4. Activate the workflow.

---

## 📤 Example Usage

- **Telegram Prompt:** Send your topic or upload a PDF to the Telegram bot.
- **Result:** You receive a Google Docs link with ready-to-post captions for each platform.

---

## 🧩 Tools & Nodes Used

- **Telegram Trigger** and **Send Message** nodes
- **Google Drive & Google Docs** nodes
- **LangChain OpenRouter Chat Models**
- **LangChain Vector Store & Embeddings (Google Gemini)**
- **Custom AI Agents for each social media platform**

---

## 🛡️ Security

- No API keys or tokens are included in this repo.
- All credentials are managed securely inside n8n’s credential store.

---

## 🆘 Troubleshooting

- **Bot not responding?**  
  Ensure your Telegram bot token is correct and the webhook is set up in n8n.
- **Google Docs not created?**  
  Check your Google Drive OAuth2 credentials and ensure the Drive folder exists.
- **No posts generated?**  
  Verify your OpenRouter API key is valid and your AI sub-agents are active.

---

## 📚 Resources

- [n8n Documentation](https://docs.n8n.io)
- [Telegram Bot API](https://core.telegram.org/bots/api)
- [Google Drive API](https://developers.google.com/drive)
- [OpenRouter](https://openrouter.ai)

---

© 2025 Pop AI PH Team