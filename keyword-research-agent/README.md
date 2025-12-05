## Keyword Research Agent V1
![Latest-0.0.1](https://img.shields.io/badge/Latest-0.0.1-blue)

An automated n8n AI workflow designed for comprehensive keyword research. It leverages Google Sheets, Google Custom Search API, Google Trends, and AI language models to discover, evaluate, and report on keyword opportunities with SEO-optimized content briefs and trend analysis.

![Keyword Research Agent screenshot](./workflow.png)

> ⚠️ Note: This image is a sample and does not represent the actual workflow

---

### 💡 Why Use Keyword Research Agent V1?
- Automates keyword discovery and analysis from a seed list stored in Google Sheets.
- Calculates keyword difficulty using SERP data enriched by domain authority heuristics.
- Integrates Google Trends for temporal interest scoring.
- Computes a combined HVLC (High Value, Low Competition) score for prioritizing keywords.
- Creates SEO content briefs using an AI agent for strategic content planning.
- Updates results back into Google Sheets automatically.
- Sends periodic and error notification emails for smooth operation.
- Generates Google Docs reports for keyword brief documentation.
- Runs on a scheduled weekly trigger for continuous keyword monitoring.

---

### ⚡ Who Is This For?
- SEO strategists and content marketers.
- Digital marketing teams.
- Agencies managing keyword research for clients.
- Website owners wanting data-driven SEO insights.
- n8n users aiming to integrate AI with keyword research.

---

### ❓ What Problem Does It Solve?
Manual keyword research and analysis is time-consuming and inconsistent. This workflow automates the process end-to-end, combining authoritative data sources and AI to surface promising keywords while assessing competition and trending interest. It reduces human error and dramatically improves productivity and content targeting precision.

---

### 🔧 How This Workflow Works

1. **Trigger:** Scheduled every Monday at 9:00 am for weekly execution.
2. **Load Seed Keywords:** Retrieves keywords from a Google Sheets document.
3. **Loop Over Seed Keywords:** Processes each keyword individually.
4. **AI Agent Content Brief Generation:** Uses Google Gemini language model to generate SEO content briefs.
5. **Google Custom Search API:** Fetches SERP results and extracts keyword competition metrics.
6. **Calculate SERP Difficulty:** Assesses difficulty based on domain authority heuristics from URL patterns.
7. **Google Trends Data:** Fetches token and trend data for keyword temporal interest analysis.
8. **Compute HVLC Score:** Combines trend scores and competition difficulty into a final priority score.
9. **Filter by HVLC > 5:** Only continues with keywords that pass this threshold.
10. **Check Existing Content:** Compares keyword with content inventory to avoid duplicates.
11. **Update Google Sheets:** Appends new keywords with metrics or updates existing records.
12. **Generate Google Docs:** Creates and updates SEO content briefs in Google Docs.
13. **Send Weekly Summary Email:** Reports top keyword opportunities via Gmail.
14. **Error Handling:** Captures errors and sends notification emails.

---

### 🔐 Setup Instructions

- ✅ Obtain and configure **Google Sheets OAuth2 credentials** with read/write access.
- ✅ Configure **Google Custom Search API** credentials (API key and CX ID).
- ✅ Set up **Google Trends** requests (no additional credentials but user-agent header required).
- ✅ Setup **Google Gemini (PaLM) API** credentials for AI agent interaction.
- ✅ Configure **Gmail OAuth2 credentials** to send email notifications.
- ✅ Import the workflow JSON into n8n.
- ✅ Verify Google Sheets document IDs and sheet IDs mapping your seed keywords and content inventory.
- ✅ Confirm triggers and schedule fit your preferred timing.
- ✅ Ensure nodes are properly connected, especially AI agents and Google Docs access.
- ✅ Enable active workflow execution.

---

### 📅 Payload
| Key            | Definition                                                  |
|----------------|-------------------------------------------------------------|
| keyword        | The seed keyword being analyzed                              |
| SERPDifficulty | Numeric score representing keyword difficulty on SERP      |
| trendAvg       | Average monthly interest on Google Trends                   |
| trendLast      | Most recent monthly interest value                           |
| HVLC           | Combined High-Value Low-Competition score                    |
| contentExists  | Boolean indicating if keyword exists in current content     |
| timestamp      | ISO timestamp of when data was updated                       |

**Example JSON Payload:**
```json
{
  "keyword": "best running shoes",
  "SERPDifficulty": 7,
  "trendAvg": 85,
  "trendLast": 95,
  "HVLC": 11.71,
  "contentExists": false,
  "timestamp": "2024-06-26T09:00:00Z"
}
```

**Example cURL Test:**
```bash
curl -X POST 'https://your-n8n-instance.com/webhook/c1425c11-9e1e-44a3-b704-c8675a88d050' \
-H 'Content-Type: application/json' \
-d '{"keyword":"best running shoes"}'
```

---

### 🔨 Tools/Node Used
- **Schedule Trigger:** Starts workflow every Monday at 9:00 am.
- **Google Sheets:** Read seed keywords and content inventory; update scores.
- **HTTP Request:** 
  - Call Google Custom Search API for SERP data.
  - Query Google Trends API for interest scores.
- **Code Nodes:** 
  - Extract and calculate SERP difficulty score.
  - Parse Google Trends API response.
  - Combine and compute final HVLC score.
  - Check content existence.
- **Langchain AI Agent:** Generate detailed SEO content briefs.
- **Google Docs:** Create and update SEO briefs documents.
- **IF Nodes:** Conditional filtering by HVLC score and content existence.
- **Gmail:** Email notifications for weekly report and error alerts.
- **Error Trigger:** Capture and notify workflow errors.
- **Merge / SplitInBatches:** Manage item loops and merges for batch processing.

---

### ⚙️ Reactive & Proactive Behavior
- **Reactive:** Automatically alerts via email on workflow errors.
- **Proactive:** Schedules weekly scans and report emails.
- **Conditional Execution:** Processes only high-value keywords passing HVLC > 5 and excluding existing content.
- **Automated Document Generation:** Proactively creates detailed SEO briefs ready for content teams.

### 🐞 Error Handling
- **Error Trigger** node listens for failures anywhere in the workflow.
- On error, sends an email with workflow name, error message, and stack trace.
- Continue execution option used in AI Agent node to avoid full workflow halt on agent errors.

---

### 🧩 Requirements
- Active n8n instance with API access.
- Google Cloud project with APIs enabled:
  - Custom Search API
  - Google Sheets API
  - Google Docs API
  - Gmail API
- Google OAuth2 credentials configured in n8n for above services.
- Google Gemini (PaLM) API credentials.
- Proper Google Sheets with keyword data and content inventory.
- Network access from n8n host to external APIs.
- Gmail account with SMTP permissions for sending emails.

---

### 📚 Resources
- [n8n Official Docs](https://docs.n8n.io/)
- [Google Custom Search API](https://developers.google.com/custom-search/v1/overview)
- [Google Trends API unofficial](https://trends.google.com/trends/)
- [Google Sheets API](https://developers.google.com/sheets/api)
- [Google Docs API](https://developers.google.com/docs/api)
- [Google Gemini (PaLM)](https://developers.generativeai.google/)
- [Gmail API](https://developers.google.com/gmail/api)
- [Langchain Node for n8n](https://docs.n8n.io/nodes/ai/langchain-agent/)

---

### 🐞 Troubleshooting
- **Invalid Google API credentials:** Verify OAuth token refresh and permission scopes.
- **Empty or incorrect Google Sheets IDs:** Confirm document and sheet IDs match your Google Drive files.
- **Google Custom Search quota exceeded:** Check API usage limits and billing status.
- **Google Trends API errors:** Ensure proper headers/user-agent besides absence of official API.
- **AI Agent errors:** Adjust prompt size and check Google Gemini quota.
- **Emails not sent:** Confirm Gmail OAuth2 credentials and access allowed for SMTP.
- **Workflow does not trigger:** Verify schedule trigger node configuration.
- **Data not appending/updating:** Validate matchingColumns keys in Google Sheets node.
- **JavaScript code fails:** Use debug pane in n8n to inspect input/output JSON payloads.
- **Delay issues:** Check node execution order or batch loop configurations.

---

> ℹ️ This section can be removed once you're fully satisfied with the content

### 📌 Recommended Improvements
- Add explicit labels for **Google Sheets** nodes such as "Load Seed Keywords" and "Load Content Inventory".
- Split complex code nodes into smaller dedicated functions for easier maintenance.
- Add environment variable support for all API keys and sensitive tokens.
- Enable incremental keyword processing with state storage to avoid reprocessing completed keywords.
- Implement more robust error logging to an external system or log file.
- Add rate limit handling and retries for Google API calls.
- Improve AI Agent prompts with dynamic competitor analysis from SERP snippets.
- Provide a dashboard or summary node for quick KPI at runtime.
- Include export functionality for final reports as PDF in addition to Google Docs.
- Optimize batch size in SplitInBatches for performance tuning on large datasets.
- Add optional Slack integration for alerting besides email.
- Extend HVLC formula customization through workflow parameters.
- Document Node ID references in a diagram for easier debugging.
- Implement a manual trigger node for ad-hoc executions.

# END