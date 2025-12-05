> ⚠️ Kindly review the docs once more before saving.

## Trend Analysis AI Agent(V2)
![Latest-0.0.1](https://img.shields.io/badge/Latest-0.0.1-blue)

The **Trend Analysis AI Agent(V2)** is an advanced AI-powered intelligence engine designed to identify emerging market and cultural trends in real time by processing raw data streams such as news mentions. It transforms data into actionable insights before trends become mainstream, enabling timely strategic decisions.

![Trend Analysis AI Agent(V2) screenshot](./workflow.png)

> ⚠️ Note: This image is a sample and does not represent the actual workflow

---

### 💡 Why Use Trend Analysis AI Agent(V2)?
- Detect early-stage emerging trends across multiple thematic categories (Cultural, Product, Market, Meme).
- Automate continuous monitoring and analysis of keyword velocity using historical baselines.
- Harness Google Gemini AI for natural language synthesis of trend insights.
- Centralize trend logging and results storage in Airtable for historical tracking and reporting.
- Support webhook integration for API-driven workflow triggering.
- Provide a manual trigger for ad-hoc or scheduled runs.
- Flexible configuration via Airtable-based trend configuration records.
- Easily extensible and maintainable workflow structure.

---

### ⚡ Who Is This For?
- Market intelligence analysts needing timely trend detection.
- Product managers tracking competitor or market shifts.
- Cultural researchers monitoring social media and news trends.
- Data scientists integrating AI with real-time data pipelines.
- Automation enthusiasts building n8n AI agent workflows.
- Teams requiring centralized, automated insight dashboards.

---

### ❓ What Problem Does It Solve?
Identifying nascent trends rapidly from high-volume news data is challenging due to noise and data volume. This workflow automates the process:

- Fetches trend configuration keywords from Airtable.
- Fetches recent news mentions via NewsAPI.
- Calculates mention velocity vs historical baselines.
- Logs raw mention data continuously.
- Uses AI to analyze significant velocity spikes.
- Outputs structured insights including sentiment, category, and recommendations.
- Stores results back to Airtable for easy access.

This allows users to anticipate market or cultural shifts proactively rather than reactively.

---

### 🔧 How This Workflow Works
1. **Trigger**: Manual trigger initiates the process (Webhook available for API calls but does not initiate analysis loop).
2. **Fetch Trend Configs**: Load active trend keywords filtered for NewsAPI source from Airtable.
3. **Loop Keywords**: Iterates over each keyword record.
4. **Fetch News Mentions**: Queries NewsAPI for recent articles mentioning the keyword.
5. **Calculate Velocity & Baseline**: Counts mentions to compute current trend magnitude.
6. **Get 7d History**: Fetch recent 7-day mention history from Airtable for baseline calculation.
7. **Compute Dynamic Baseline**: Calculates average historical mentions to define baseline; computes velocity % change.
8. **Log Mentions**: Stores current mention data in Airtable trend history.
9. **Threshold Check**: Determines if velocity exceeds configured threshold.
   - If **yes**, processes to AI analysis.
   - If **no**, cleans output and loops to next keyword.
10. **AI Trend Analyzer**: Uses Google Gemini AI to generate JSON insight summary including sentiment, reason, and recommendation.
11. **Filter Null**: Filters out null AI responses.
12. **Save Trend Result**: Saves AI-generated insights to Airtable trends result table.
13. **Loop Continue**: Continues looping for all keywords.
14. **Webhook & Validation**: Provides an API endpoint with key validation returning developer notes but does **not** trigger analysis.

---

### 🔐 Setup Instructions
- ✅ **Configure Credentials in n8n**:
  - Google Gemini (PaLM) API key
  - Airtable Personal Access Token
  - NewsAPI Key (for news mention fetch)
- ✅ **Setup Airtable Base** with required tables and columns:
  - Tables: `trend_configs`, `trend_history`, `trend_results`
  - Ensure fields `keyword`, `category`, `baseline_window`, `velocity_threshold`, `Sources`, etc. are properly configured.
- ✅ **Environment Variables**:
  - `TREND_AGENT_API_KEY`: Set to `trend_agent_test_key` or your secret for webhook validation.
- ✅ **Manual Trigger**: Use the manual trigger node to start the analysis workflow.
- ✅ **Webhook Use**: Available for NMS compliance but does not initiate analysis—used to get developer notes or integrate with systems.
- ✅ **API Key for Webhook**: Provide `trend_agent_test_key` in JSON payload on webhook POST.

---

### 📅 Payload
| Key            | Definition                                                                                                    |
| -------------- | -------------------------------------------------------------------------------------------------------------|
| keyword        | The keyword or topic being tracked for trend mentions                                                        |
| velocity       | Percentage increase/decrease in mention count compared to historical baseline                                |
| reason         | Explanation or summary text from AI describing the trend growth                                              |
| sentiment      | Sentiment label: `positive`, `neutral`, or `negative` regarding the trend                                    |
| category       | One of the trend categories: `Cultural`, `Product`, `Market`, or `Meme`                                       |
| recommendation | AI-generated strategic recommendation based on trend analysis                                                |

**Example JSON Payload:**
```json
{
  "keyword": "electric vehicles",
  "velocity": 35.78,
  "reason": "Steady increase in news articles covering government subsidies and new releases",
  "sentiment": "positive",
  "category": "Market",
  "recommendation": "Explore partnerships with EV manufacturers to capitalize on growing interest"
}
```

**Example cURL Test:**
```bash
curl -X POST https://n8n.popapps.ai/webhook-test/trend-analysis-agent \
-H "Content-Type: application/json" \
-d '{"apiKey": "trend_agent_test_key"}'
```

---

### 🔨 Tools/Node Used
- **Manual Trigger**: Initiates the whole workflow manually.
- **Airtable nodes**: For querying configurations, history, and saving logs/results.
- **HTTP Request (NewsAPI)**: Fetches latest news articles mentioning keywords.
- **Code Nodes**: Custom JS logic to calculate velocities and baselines.
- **If Node**: Checks whether mention velocity surpasses threshold.
- **LangChain Google Gemini Chat Model**: AI language model generating trend insight.
- **AI Output Parser (Structured)**: Parses structured JSON responses from AI node.
- **Webhook Node + Validation + Respond to Webhook**: Provides API entry with API key validation for compliance.
- **Sticky Notes**: Provides in-workflow documentation and developer notes.
- **Split in Batches**: Efficiently loop over keyword records.

---

### ⚙️ Reactive & Proactive Behavior
- **Reactive**: Executes manually or by webhook call for on-demand trend analysis.
- **Proactive**: Although not included here, the manual trigger can be replaced with a Cron trigger for automated periodic runs.
- **Threshold Logic**: Only keywords with velocity exceeding a configured velocity threshold proceed to AI analysis, reducing noise.

### 🐞 Error Handling
- **API key validation node**: Throws explicit error on invalid webhook calls.
- **Data filtering nodes**: Filter null or incomplete items to avoid downstream failures.
- **Graceful velocity calculation fallbacks**: Default baseline fallback to avoid division by zero or no historical data.
- **Fallback URLs/cached names**: Use cached Airtable URLs in nodes for quick debugging.

---

### 🧩 Requirements
- Registered **Google Gemini (PaLM API)** credentials configured in n8n.
- **Airtable Personal Access Token** with appropriate read/write permissions.
- Valid **NewsAPI key** for fetching current articles.
- N8N environment variables setup including `TREND_AGENT_API_KEY`.
- Airtable base setup matching the schema referenced in workflow nodes.
- Nodes from n8n standard and LangChain community packages installed.

---

### 📚 Resources
- [n8n Documentation](https://docs.n8n.io)
- [Airtable API Reference](https://airtable.com/api)
- [NewsAPI](https://newsapi.org/docs)
- [Google PaLM API (Gemini)](https://developers.generativeai.google)
- [LangChain n8n Nodes](https://github.com/n8n-io/n8n-nodes-langchain)
- [JSON Schema for output](https://json-schema.org/understanding-json-schema)
  
---

### 🐞 Troubleshooting
- Verify API keys and credentials are correctly configured in n8n credential manager.
- Ensure Airtable base IDs and table names match those referenced in node parameters.
- Check NewsAPI quota and limits to avoid request failures.
- Use manual trigger during testing to isolate workflow runs.
- Confirm `TREND_AGENT_API_KEY` environment variable matches webhook validation key.
- Review node executions in n8n UI logs for error messages.
- Inspect AI model rate limits or quota issues if AI trend analysis fails.
- For null outputs, verify timely NewsAPI results and configured velocity thresholds.
- Make sure LangChain nodes are installed and configured properly.
- Confirm webhook endpoint URL and payload formats for API integration.

---

> ℹ️ This section can be removed once you're fully satisfied with the content

### 📌 Recommended Improvements
- Rename node **"Trigger-Manual Run"** to **"Start Manual Analysis"** for clarity.
- Rename **"Get Trend Configs"** → **"Fetch Active Trend Keywords (Airtable)"** for explicit purpose.
- Rename **"Loop Keywords"** → **"Process Each Keyword"** to better express iteration intent.
- Rename **"Fetch News Mentions"** → **"Query NewsAPI for Keyword"** for descriptive clarity.
- Rename **"Calculate Velocity & Baseline"** → **"Compute Daily Mention Stats"** to reflect computation.
- Rename **"Get 7d History"** → **"Retrieve 7-Day Mention History"** for precision.
- Rename **"Compute Dynamic Baseline"** → **"Calculate Baseline & Velocity %"** for clear function.
- Rename **"Log Mentions"** → **"Log Daily Mention Data (Airtable)"** to clarify storage.
- Rename **"Velocity > Threshold?"** → **"Check Velocity Threshold Exceeded?"** for decision point clarity.
- Rename **"AI Trend Analyzer"** → **"Google Gemini AI Insight Generation"** to state AI role.
- Rename **"Filter Null"** → **"Filter Valid AI Outputs"** to indicate filtering purpose.
- Rename **"Save Trend Result"** → **"Store AI Trend Analysis Result"** for directness.
- Rename **"validation"** → **"Webhook API Key Validation"** for security context.
- Consider adding a Cron trigger node for fully automated periodic execution.
- Add more detailed error handling with notification nodes for failure alerts.
- Document example Airtable base table structures and sample config records.
- Provide a sample payload template for users integrating via webhook.
- Modularize heavy code nodes into reusable snippets or external scripts.
- Expand AI prompt template to increase context relevance or experiment with additional attributes.
- Include environment variable validation node to catch missing configuration early.

# END