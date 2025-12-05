> ⚠️ Kindly review the docs once more before saving.

## Customer Lifetime Value (CLV) Forecaster  
![Latest-0.0.1](https://img.shields.io/badge/Latest-0.0.1-blue)

The **Customer Lifetime Value (CLV) Forecaster** is an automation workflow designed to predict and provide actionable insights on a customer's future value based on their transaction behavior. It leverages data extraction from Google Sheets, AI-powered analysis via Google Gemini (PaLM), and integrates results into Notion and Slack for real-time notification and record keeping.

![Customer Lifetime Value (CLV) Forecaster screenshot](./workflow.png)

> ⚠️ Note: This image is a sample and does not represent the actual workflow.

---

### 💡 Why Use Customer Lifetime Value (CLV) Forecaster?
- Automate prediction of customer lifetime value using transaction data.
- Generate concise AI-driven insight summaries with confidence scores.
- Visualize key customer metrics and drivers influencing CLV.
- Seamlessly log predictive insights into Notion databases.
- Notify sales or marketing teams instantly via Slack.
- Handle error conditions gracefully and notify stakeholders.
- Streamline customer segmentation for targeted marketing strategies.

---

### ⚡ Who Is This For?
- Data analysts looking to automate CLV calculations.
- Marketing teams needing dynamic customer insights.
- Sales managers aiming to identify high-value customers.
- Product managers wanting data-driven customer segmentation.
- Automation engineers integrating AI with business data.
- Anyone looking to use AI for customer analytics within n8n.

---

### ❓ What Problem Does It Solve?
Manual calculation and analysis of customer lifetime value (CLV) from raw transaction data can be time-consuming and error-prone. This workflow automates:

- Extraction of customer transactions from Google Sheets,
- Calculation of Recency, Frequency, and Monetary (RFM) features,
- Prediction of CLV using a custom formula augmented by Google Gemini's AI-generated insight,
- Logging the enriched results into Notion for record-keeping,
- Instant Slack alerts for high-value customers to trigger marketing actions.

It enables timely, accurate, and actionable CLV forecasting at scale.

---

### 🔧 How This Workflow Works

1. **Webhook Trigger:** Listens for incoming POST requests at the `/clv` endpoint with customer ID payload.
2. **Get row(s) in sheet:** Retrieves customer data from a Google Sheet based on customer ID.
3. **If Node:** Verifies if the customer exists; if not found, responds with a "not find customer id" JSON message.
4. **Get Transaction Rows:** Fetches all transactions for the found customer from another Google Sheet.
5. **Second If Node:** Checks if transactions exist; if empty, responds with "not find transactions".
6. **RFM Feature Engineering:** Computes Recency (days since last purchase), Frequency (total purchases), and Monetary (total spent) metrics using transaction data.
7. **Feature Engineering (RFM):** Applies a simple predictive model combining RFM metrics to estimate predicted CLV and confidence score; also determines key drivers.
8. **Generate Insight Summary:** Sends RFM features to Google Gemini AI to get a JSON-formatted insight summary, confidence metrics, and CLV prediction.
9. **Code in JavaScript:** Parses and cleans the AI response ensuring the output follows the strict JSON schema expected.
10. **Create a database page:** Inserts or updates a page in a Notion database named "CLV Predictions" with all customer metrics and AI insights.
11. **Send a message:** Posts a detailed message with highlighted CLV insights and drivers to a designated Slack channel.
12. **Respond to Webhook:** Returns JSON response summarizing the predicted CLV and insights back to the API caller.
13. **Error Handling:** Includes an Error Trigger node which, if activated, sends error notifications to an admin Slack channel.

---

### 🔐 Setup Instructions
- ✅ **Google Sheets Configuration:**  
  Prepare two Google Sheets: one for customer master data, one for transaction data. Share with the OAuth2 service account used in credentials.
- ✅ **Google Gemini (PaLM) API:**  
  Obtain Google PaLM API key and create credentials in n8n to enable AI-driven insight generation.
- ✅ **Slack OAuth2:**  
  Set up Slack OAuth2 credentials with permission to post messages to desired channels.
- ✅ **Notion API Token:**  
  Create a Notion integration, share the target "CLV Predictions" database, and add credentials in n8n.
- ✅ **Webhook Exposure:**  
  Ensure n8n workflow webhook is accessible publicly or via proxy for API POST calls.
- ✅ **Node Configuration:**  
  Verify all nodes have correct IDs for Google Sheets, Notion database, Slack channels, and Google Gemini model set.
- ✅ **Error Monitoring:**  
  Configure the Error Trigger node Slack channel for real-time error notifications.

---

### 📅 Payload
| Key            | Definition                                        |
| -------------- | ------------------------------------------------ |
| `customer_id`  | Unique customer identifier (input to POST webhook) |
| `predicted_clv`| Forecasted customer lifetime value (output)      |
| `confidence_score` | AI model confidence in prediction (output)    |
| `insight_summary` | Short AI-generated paragraph summarizing key insights (output) |
| `top_drivers`  | Array of top factors driving predicted CLV (output) |

**Example JSON Payload (Input to Webhook):**  
```json
{
  "customer_id": "12345"
}
```

**Example JSON Response (Output from Webhook):**  
```json
{
  "customer_id": "12345",
  "predicted_clv": 1500,
  "confidence": 0.85,
  "recency": "",
  "frequency": 12,
  "monetary": 3200,
  "top_drivers": "frequency, monetary",
  "insight_summary": "This customer shows consistent purchasing behavior likely to increase revenue."
}
```

**Example cURL Test:**  
```bash
curl -X POST https://your-n8n-instance.url/webhook/clv \
     -H "Content-Type: application/json" \
     -d '{"customer_id": "12345"}'
```

---

### 🔨 Tools/Node Used
- **Webhook Node:** Receives incoming requests triggering the workflow.
- **Google Sheets Nodes:** Fetch customer and transaction data based on provided customer ID.
- **If Nodes:** Conditional routing to handle missing data scenarios.
- **Code Nodes:** Compute RFM metrics and apply predictive scoring logic.
- **Google Gemini Node:** Calls Google PaLM AI for insight generation with strict JSON output rules.
- **Notion Node:** Creates or updates customer predictive insights in Notion database.
- **Slack Nodes:** Sends notifications to relevant Slack channels.
- **Respond to Webhook Node:** Sends well-structured response back to API caller.
- **Error Trigger Node:** Captures workflow errors and notifies admins.

---

### ⚙️ Reactive & Proactive Behavior
- **Reactive:** Waits for customer ID payload before processing.
- **Proactive:** Pushes notifications to Slack automatically on successful insights generation.
- **Error Handling:** Proactively informs admins when errors occur.

---

### 🐞 Error Handling
- Returns structured JSON error messages for:
  - Missing customer ID.
  - Missing transaction data.
  - Failure to parse AI-generated JSON.
- Sends Slack alerts to error channel via Error Trigger node.

---

### 🧩 Requirements
- n8n instance with public webhook endpoint.
- Google Sheets with customer master and transaction data.
- Google Cloud PaLM / Gemini API access.
- Slack workspace and token for messaging.
- Notion workspace with database for CLV data.
- Valid OAuth2 credentials configured for all external services.

---

### 📚 Resources
- [Google Sheets API](https://developers.google.com/sheets/api)  
- [Google PaLM API (Gemini)](https://developers.generativeai.google)  
- [Slack API OAuth2](https://api.slack.com/authentication/oauth-v2)  
- [Notion API](https://developers.notion.com)  
- [n8n Documentation](https://docs.n8n.io)  

---

### 🐞 Troubleshooting
- Verify all OAuth2 credentials are valid and not expired.
- Check Google Sheets document and sheet IDs match configuration.
- Ensure webhook URL is reachable from clients.
- Confirm Slack channels exist and app has posting permissions.
- If AI JSON parsing fails, review raw Gemini node response logs.
- Monitor Error Trigger channel for incoming alerts.
- Handle rate limits from APIs by adding retries in n8n.
- Validate JSON schema compliance in AI prompts to avoid parsing errors.
- Inspect logs if Notion page creation fails.
- Restart n8n workflow or instance if encountering unexpected execution states.

---

> ℹ️ This section can be removed once you're fully satisfied with the content

### 📌 Recommended Improvements
- Rename nodes with concise descriptive names (e.g., "Fetch Customer Data", "Validate Customer ID", "Parse AI Response").
- Add retry logic on Google Sheets and Notion nodes for resiliency.
- Parameterize Slack channel IDs to allow dynamic routing per environment.
- Enhance RFM model to use time-weighted metrics or ML models.
- Implement authentication around webhook for security (e.g., API key).
- Add logging node for monitoring workflow progress.
- Modularize AI prompt into a reusable template node.
- Include unit testing steps in workflow for each logical section.
- Introduce caching mechanism for repeated calls on same customer ID.
- Add timeouts and error notifications for slow or failed API calls.
- Create dashboard to visualize CLV trends over time.
- Localize Slack message content for multi-language teams.
- Equip workflow with version control tagging in node names.
- Document node credentials access restrictions in internal wiki.

# END