> ⚠️ Kindly review the docs once more before saving.

## Topic Idea Generator  
![Latest-0.0.1-blue](https://img.shields.io/badge/Latest-0.0.1-blue)  

The **Topic Idea Generator** is an automated workflow designed within **n8n** that generates prioritized content topic ideas based on input themes received via Telegram. Leveraging AI models and multiple data sources (Google Sheets, Airtable), it evaluates content opportunities by analyzing keyword demand, trends, and competitive gaps. The process culminates in a detailed content brief report stored in Google Docs and notified via Telegram.

![Topic Idea Generator screenshot](./workflow.png)

> ⚠️ Note: This image is a sample and does not represent the actual workflow

---

### 💡 Why Use Topic Idea Generator?  
- Automatically generate SEO-driven content ideas organized by thematic clusters  
- Integrate real-time inputs from Telegram commands, making workflow interactive and user-driven  
- Fuse data from multiple sources (keyword demand, trend velocity, competitor data) for well-rounded topic evaluation  
- Calculate ROI scores to prioritize content ideas based on demand, trend, and competition  
- Produce structured content briefs to guide content teams effectively  
- Log inputs and outputs to Google Sheets for traceability and further analysis  
- Deliver final reports as Google Docs and notify users via Telegram  

---

### ⚡ Who Is This For?  
- Content strategists seeking data-driven topic ideation  
- Marketing teams looking to automate content planning  
- SEO specialists wanting a multi-dimensional analysis of content opportunities  
- AI enthusiasts interested in integrating language models within automation workflows  
- Teams using Telegram as a communication channel for triggering content requests  

---

### ❓ What Problem Does It Solve?  
This workflow eliminates the manual and fragmented process of content topic research by:  
- Accepting natural Telegram commands specifying themes and priorities  
- Automatically normalizing input and validating commands to avoid errors  
- Pulling in vital data like keyword volumes (demand), trend velocities, and competitor analysis from external API and spreadsheet sources  
- Scoring and ranking ideas quantitatively to identify high-ROI topics  
- Synthesizing these insights into actionable content briefs using Google Gemini AI models  
- Automating the creation, update, and sharing of content reports for organizational efficiency  

---

### 🔧 How This Workflow Works  
1. **Telegram Trigger Node**: Listens to Telegram for user commands specifying theme and priority. Example command:  
   `/generate theme: AI Governance 2025 priority: critical`  
2. **Normalize Trigger Payload (Code Node)**: Parses the Telegram text command to extract `theme` and `priority`. Validates that `theme` is present.  
3. **If Node (Validation)**: Checks if the command is valid. If invalid, sends a Telegram error message to user with usage instructions. If valid, proceeds.  
4. **Append Row in Google Sheet**: Logs the request details (`timestamp`, `trigger_type`, `theme`, `priority`, and status `"accepted"`) for audit.  
5. **Build Query Payload (Code Node)**: Constructs payload with normalized theme, priority, and sets `seed_keyword` for downstream API calls.  
6. **Get Competitor Contents Analysis Data & Get Trend Analysis Result**: Pull competitor data from Google Sheets and trend data from Airtable.  
7. **Format Competitor Data & Normalize Trend Data**: Standardizes the competitor and trend data into uniform JSON structures.  
8. **Merge - Source Agent Data**: Combines keyword, trend, and competitor datasets into a single unified object.  
9. **Normalize Data for Fusion**: Transforms merged data, applies validation to ensure each data source returns results, else throws error.  
10. **Fuse Data & Calculate ROI (Code Node)**: Uses weighted formula combining keyword volume (demand), trend velocity, and competitive gap to compute ROI scores for each topic. Sorts topics by ROI descending.  
11. **Append Raw Scores**: Stores raw scoring data to a Google Sheet for detailed analysis.  
12. **Select Top Topics**: Selects top 20 high-ROI topics for further AI-driven content clustering.  
13. **Topic Clustering & Idea Generator (LangChain AI Agent)**: Groups topics into content clusters and generates multiple creative content ideas per cluster, returning structured JSON.  
14. **Code – Parse LLM Output & Compute Idea Scores**: Parses the AI output and in parallel computes idea-specific scores with placeholders (for actual API integration).  
15. **Select Top Topics1 & Generate Content Briefs (LangChain AI Agent)**: Selects top ideas and requests AI to produce actionable, concise content briefs per idea.  
16. **Validate Briefs (Code Node)**: Validates the AI-generated briefs.  
17. **Append row in sheet1**: Logs content briefs in Google Sheets.  
18. **Create a Document & Build Final Report Body**: Generates Google Docs report containing executive summary and prioritized content ideas with statistics and recommendations.  
19. **Update a Document**: Updates the document with the final content.  
20. **Send a Text Message**: Sends Telegram notification to the user with the link to the generated report.  

---

### 🔐 Setup Instructions  
- ✅ Create and configure a Telegram Bot for receiving commands and sending replies; store credentials in n8n.  
- ✅ Set up Google Sheets credentials with OAuth2 and prepare spreadsheets for logging requests, raw scores, and content briefs.  
- ✅ Configure Google Docs API for report creation and updating.  
- ✅ Create Airtable base with trend analysis data and configure API access.  
- ✅ Add Google Gemini (PaLM) API credentials for AI text generation and parsing.  
- ✅ Import the workflow JSON into n8n and ensure all nodes reference correct credentials and spreadsheet/document IDs.  
- ✅ Test the Telegram command syntax strictly (e.g., `/generate theme:<topic> priority:<level>`) for proper functioning.  
- ✅ Optionally set environment variables or tweak weights in Fuse Data node for custom ROI scoring.  

---

### 📅 Payload  
| Key          | Definition                                                 |  
|--------------|------------------------------------------------------------|  
| trigger_type | Origin of the trigger — e.g., telegram_command             |  
| theme        | Content theme or topic provided by user                    |  
| priority     | Priority level set by user (e.g., normal, critical)        |  
| timestamp    | ISO timestamp of trigger processing                        |  
| valid        | Boolean indicating if the command had valid parameters     |  
| error        | Error message if invalid command                            |  

**Example JSON Payload:**  
```json
{
  "valid": true,
  "trigger_type": "telegram_command",
  "theme": "AI Governance 2025",
  "priority": "critical",
  "timestamp": "2024-06-11T10:00:00Z"
}
```

**Example Telegram Command:**  
```
/generate theme: AI Governance 2025 priority: critical
```

---

### 🔨 Tools/Node Used  
- **Telegram Trigger**: Listens for incoming Telegram bot messages.  
- **Code Nodes**: Used extensively for data parsing, normalization, scoring calculation, validation, and report generation.  
- **If Node**: Branching logic to validate inputs and control error handling.  
- **Google Sheets Node**: Reads and writes data for logging and storing analysis results.  
- **Google Docs Node**: Creates and updates Google Docs reports.  
- **Airtable Node**: Pulls trend data from Airtable base.  
- **LangChain Agents (AI nodes)**: Uses Google Gemini PaLM models for language understanding, output parsing, content clustering, and brief generation.  
- **Merge Node**: Combines datasets from multiple sources for unified processing.  

---

### ⚙️ Reactive & Proactive Behavior  
- Reactive: Workflow triggers immediately upon receiving Telegram commands.  
- Proactive: Automatically pulls trend and competitor data, invokes AI models for content synthesis, and sends back notifications without manual intervention.  

### 🐞 Error Handling  
- Invalid Telegram commands (missing theme/incorrect format) trigger immediate error messages sent back to the user.  
- Data validation code nodes throw errors if any source data is missing, halting further execution to prevent misleading output.  
- Expected JSON parsing errors from AI output are caught and raise descriptive errors.  
- Logs all incoming requests and results in Google Sheets for audit and error analysis.  

---

### 🧩 Requirements  
- Telegram Bot Token and configured webhook in n8n.  
- Google Cloud Project with Sheets, Docs, and OAuth2 credentials.  
- Airtable account with proper base and API token for trends data.  
- Google PaLM API credentials for AI-driven language model nodes.  
- Prepared Google Sheets templates for logging requests, raw scores, and briefs.  
- n8n environment with installed Google Docs, Sheets, Telegram, Airtable, and Code nodes.  

---

### 📚 Resources  
- [n8n Documentation](https://docs.n8n.io/)  
- [Google Sheets API](https://developers.google.com/sheets/api)  
- [Google Docs API](https://developers.google.com/docs/api)  
- [Telegram Bot API](https://core.telegram.org/bots/api)  
- [Airtable API](https://airtable.com/api)  
- [Google PaLM API (Gemini)](https://developers.generativeai.google)  
- [LangChain for n8n](https://n8n.io/integrations/langchain)  

---

### 🐞 Troubleshooting  
- **Telegram commands return "Invalid Command"**: Ensure exact syntax `/generate theme:<topic> priority:<level>` with no extra spaces or typos.  
- **AI model returns JSON parsing errors**: Verify API credentials and model endpoint availability. Check for unexpected outputs.  
- **Google Sheets nodes fail**: Confirm OAuth2 credentials, sheet IDs, and permissions. Sheets must exist and be accessible.  
- **Airtable trend data is empty**: Verify API token and base/table IDs; ensure data records exist.  
- **Google Docs API errors**: Ensure document creation folder permissions and document URLs are valid.  
- **ROI scores all zero**: Check if input data arrays for keywords/trends/competitors have valid numeric values.  
- **Long execution times**: Review rate limits on APIs and reduce fetched dataset sizes if needed.  

---

> ℹ️ This section can be removed once you're fully satisfied with the content

### 📌 Recommended Improvements  
1. Rename **"Normalize Tringger Payload"** to **"Normalize Trigger Payload"** (fix typo and clarify purpose).  
2. Add explicit environment variables for ROI weights (`W_DEMAND`, `W_TREND`, `W_COMP`) for easier tuning without code edits.  
3. Parameterize maximum topic count for AI brief generation to allow flexible report lengths.  
4. Implement retry mechanisms for external API calls (Google Sheets, Airtable, AI models) to enhance reliability.  
5. Include detailed command usage help sent automatically on trigger with `/help` or invalid commands.  
6. Introduce logging node to capture detailed error logs in a centralized Google Sheet or database.  
7. Add a "Test" Telegram command for validation that does not write to sheets but simulates processing for troubleshooting.  
8. Modularize the large scoring code node by splitting fusion, normalization, and scoring into separate smaller nodes for maintainability.  
9. Provide a fallback or default content cluster generation if LangChain output parsing fails.  
10. Securely store and rotate API keys/credentials used by nodes with n8n's credential management features.  
11. Update the "Generate Content Briefs" prompt and constraints regularly to reflect current SEO best practices.  
12. Add localization support for Telegram error and notification messages to support multi-languages.  
13. Use n8n expressions to dynamically refer to Telegram chatId to improve flexibility across chat types (groups, channels).  
14. Implement rate limiting or cooldown for Telegram commands to prevent abuse or flooding.  
15. Create a dashboard or visualization using the stored Google Sheets data to monitor trends and top topics over time.

# END