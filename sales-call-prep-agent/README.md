> ⚠️ Kindly review the docs once more before saving.

## Sales Call Assistant
![Latest-0.0.1](https://img.shields.io/badge/Latest-0.0.1-blue)

An intelligent workflow designed to assist salespeople in preparing for calls by automatically generating personalized briefing documents. This `n8n` AI agent collects prospect information from Telegram commands, enriches it with real-time news and web search context, synthesizes insights using a Google Gemini AI model, and outputs a structured briefing as a Google Doc, delivering notifications back to the user via Telegram.

![Sales Call Assistant screenshot](./workflow.png)

> ⚠️ Note: This image is a sample and does not represent the actual workflow

---

### 💡 Why Use Sales Call Assistant?
- Automates gathering and summarizing current, relevant information about prospects and their companies.
- Integrates multiple data sources (Google Custom Search, NewsAPI) to enrich sales call prep.
- Utilizes Google Gemini AI to distill research into concise, actionable insights.
- Produces organized call briefing documents directly in Google Docs.
- Seamlessly interacts with users through Telegram for input and notifications.
- Manages concurrency via locking to prevent overlapping requests.
- Provides fallback mechanisms to handle API failures gracefully.

---

### ⚡ Who Is This For?
- Sales professionals seeking tailored, data-driven preparation for calls.
- Sales support teams aiming to streamline research and briefing creation.
- Business development representatives who want timely and relevant company intelligence.
- Automation enthusiasts leveraging AI and n8n for sales enablement workflows.

---

### ❓ What Problem Does It Solve?
Manually gathering information before sales calls can be time-consuming, error-prone, and superficial. This workflow automates collecting and synthesizing data from multiple sources, providing a well-structured briefing that highlights pain points, opportunities, and conversation questions. It reduces prep time and improves meeting effectiveness by delivering AI-curated insights specific to each prospect and company.

---

### 🔧 How This Workflow Works
1. **Telegram Trigger Node** listens for incoming messages formatted as:  
   `Prepare me for a call with <Name> from <Company>`.
2. **Parsing Data Node** extracts `prospect` and `company` from the Telegram text, validating the format.
3. **Check: Request Format Node** confirms message correctness; invalid inputs trigger a helpful error message.
4. **Locking Logic** ensures a user cannot overload the workflow with simultaneous requests.
5. **Input: Prospect Info Node** assigns extracted prospect and company info for downstream use.
6. **Limit Nodes** throttle the number of results fetched (max 5 each) from APIs.
7. **Fetch Google Search Results Node** and **Fetch News Articles Node** retrieve topical data via Google CSE and NewsAPI.
8. **Split and Format Nodes** parse and clean individual news and search results for uniformity.
9. **Combine News + Google Data Node** merges data streams into one comprehensive list.
10. **Filter + Prepare Research Node** generates a human-readable markdown summary of all gathered data.
11. **Generate Call Prep Briefing Node** calls the Google Gemini AI model to analyze and synthesize key insights, pain points, opportunities, and suggested questions.
12. **Format Briefing for Doc Node** parses the AI response JSON safely.
13. **Google Doc: Create New + Update Existing Nodes** create and populate a Google Doc with the briefing content, using templated formatting.
14. **Success Message Node** sends a Telegram message with a link to the Google Doc.
15. **Release Lock Node** frees the user's request queue to enable new queries.
16. **Error Handling Nodes** (e.g., fallback for failed API calls, Gemini API error messaging) ensure reliable user feedback and graceful degradation.
17. A **Webhook + Validation Node** exists for NMS compliance, returning developer notes and does not start the real workflow.

---

### 🔐 Setup Instructions
- ✅ Create and configure a Telegram Bot. Obtain Bot Token and Bot Username.
- ✅ Set up Google Custom Search Engine and get API Key + Search Engine ID (CX).
- ✅ Register for NewsAPI and acquire an API key (optional but recommended fallback).
- ✅ Configure Google Docs OAuth credential (Email, Client ID, Client Secret).
- ✅ Set up Google Gemini (PaLM) API Key credential for AI generation.
- ✅ Store all credentials securely in n8n Credential Manager.
- ✅ Add an environment variable for API key validation:  
  `SALES_CALL_ASSISTANT_API_KEY = sales_call_assistant_test_key`.
- ✅ Ensure network access for external API calls.
- ✅ (Optional) Implement Redis/Airtable/n8n Memory nodes if scaling requires refined locking logic.
- ✅ Deploy the workflow in n8n and activate the Telegram Trigger node.
- ✅ Update API keys and tokens with your real secrets.

---

### 📅 Payload
| Key              | Definition                                               |
|------------------|----------------------------------------------------------|
| prospect         | Name of the sales prospect extracted from user input    |
| company          | Company name extracted from user input                   |
| formattedResults | Aggregated and formatted summary of news & search data  |
| prospect_background | AI-generated short summary of prospect’s role/history  |
| company_summary  | AI-generated company overview including industry and culture |
| pain_points      | List of company challenges or risks identified           |
| opportunities    | Potential alignment or sales openings                     |
| questions        | Open-ended questions to guide the sales conversation     |
| documentId       | Google Doc identifier for generated briefing document    |
| chat_id          | Telegram chat ID (used for messaging)                    |

**Example JSON Payload:**
```json
{
  "prospect": "Jane Doe",
  "company": "Acme Corp",
  "formattedResults": "### Research Results\n1. [Acme Launches New Product](https://example.com/article)...\n",
  "prospect_background": "Senior Manager with 10 years in tech sales",
  "company_summary": "Acme Corp is a mid-sized software provider known for cloud integration...",
  "pain_points": ["Increasing competition", "Scaling challenges"],
  "opportunities": ["New product synergy", "Expansion into European market"],
  "questions": ["What challenges are you facing with product adoption?", "How do you prioritize cloud security?"],
  "documentId": "1A2B3C4D5E6F",
  "chat_id": 123456789
}
```

**Example cURL Test:**
```bash
curl -X POST https://n8n.popapps.ai/webhook-test/sales-call-assistant \
     -H 'Content-Type: application/json' \
     -d '{"apiKey": "sales_call_assistant_test_key"}'
```

---

### 🔨 Tools/Node Used
- **Telegram Trigger:** Captures user input via Telegram messages.
- **Code Nodes:** For parsing, locking mechanism, formatting, and validation logic.
- **IF nodes:** Validate input formatting and locking state.
- **HTTP Request Nodes:** Query Google Custom Search and News API for information.
- **Split Out Nodes:** Process multiple search/news result items separately.
- **Merge Node:** Combine disparate data sources into a unified feed.
- **Google Gemini (PaLM) Node:** AI-based summarization and briefing generation.
- **Google Docs Nodes:** Automate document creation and update with AI content.
- **Telegram Send Message Nodes:** Communicate statuses/errors/results back to users.
- **Wait & Limit Nodes:** Control flow timing and result quantity to manage API rate limits.
- **Webhook Node:** Supports external API calls for integration.
- **Sticky Note:** Embedded developer notes and setup instructions.

---

### ⚙️ Reactive & Proactive Behavior
- **Reactive:** Responds immediately to Telegram messages with parsing and validation feedback.
- **Proactive:** Fetches current news and search data autonomously and generates AI-processed briefing without additional user input.
- **Locking Mechanism:** Prevents overlapping requests from the same user, ensuring workflow integrity.
- **Error Handling:** Proactively sends user notifications if APIs fail or input is invalid.

### 🐞 Error Handling
- Invalid request format triggers a clear Telegram message requesting proper format.
- API failures on News or Google Search fallback to safe default messages.
- Gemini AI processing errors send Telegram error notifications.
- Locking prevents overwhelming the workflow with concurrent requests.
- Webhook validation rejects unauthorized API key usage.

---

### 🧩 Requirements
- An active Telegram Bot with proper token and user access.
- Google Custom Search Engine credentials (API key, CX).
- News API key (optional).
- Google Docs OAuth credentials.
- Google Gemini API key.
- n8n environment capable of executing HTTP requests and connecting to these services.
- Environment variable `SALES_CALL_ASSISTANT_API_KEY` set for webhook validation.
- Network connectivity for external APIs.

---

### 📚 Resources
- [Telegram Bot API Documentation](https://core.telegram.org/bots/api)
- [Google Custom Search JSON API](https://developers.google.com/custom-search/v1/overview)
- [NewsAPI Documentation](https://newsapi.org/docs)
- [Google Docs API](https://developers.google.com/docs/api)
- [Google Gemini AI (PaLM) Docs](https://developers.generativeai.google/)
- [n8n Official Documentation](https://docs.n8n.io/)

---

### 🐞 Troubleshooting
- **No reply from Telegram:** Verify Telegram bot token and webhook setup.
- **Invalid input errors:** Confirm user input conforms to `Prepare me for a call with <Name> from <Company>`.
- **Google API failures:** Check API keys, quota limits, and billing status.
- **News API empty results:** Ensure News API key configured and active.
- **Gemini AI errors:** Validate API key and model name, retry with delays.
- **Document creation fails:** Confirm Google Docs OAuth scopes and permissions.
- **Concurrent request lock issues:** Review locking code or increase memory persistence.
- **Webhook requests unauthorized:** Check the `SALES_CALL_ASSISTANT_API_KEY` environment variable matches client calls.
- **Rate limiting:** Implement/wait between retries with `Wait` node or increase API quota.
- **Data formatting issues:** Review Code node JavaScript for any custom modifications.

---

> ℹ️ This section can be removed once you're fully satisfied with the content

### 📌 Recommended Improvements
- Rename nodes with clearer action-based names, e.g., "Parse Telegram Input," "Validate Format," "Check Request Lock."
- Add explicit comments or descriptions to complex code nodes for easier maintenance.
- Enhance fallback logic to dynamically retry failed API calls before fallback.
- Consolidate duplicate Code nodes that use identical JavaScript functions for news and CSE formatting.
- Implement more granular locking strategy with Redis or external state store for scalability.
- Include formal unit tests or workflow testing using n8n test runners or Postman.
- Add a node to sanitize prospect/company names against injection risks.
- Include logging nodes capturing workflow execution metadata for auditing.
- Provide user feedback for long-running operations beyond 10 seconds to improve UX.
- Allow configurable result limits (default 5) to be set via workflow parameters or environment variables.
- Introduce monitoring/alerting integrations on workflow errors or rate limiting events.
- Incorporate a dialog confirmation step in Telegram interaction to avoid accidental requests.
- Automate provisioning of Google Docs folder defaults or create fallback locations.
- Extend AI model prompts with customizable templates to tailor briefing style per team.
- Provide detailed inline examples of payload and responses within the workflow for developer reference.

# END