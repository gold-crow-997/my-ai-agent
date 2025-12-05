
## Hashtag Suggestion Agent
![Latest-0.0.1](https://img.shields.io/badge/Latest-0.0.1-blue)

The **Hashtag Suggestion Agent** is an automated n8n workflow designed to generate optimized hashtag strategies tailored for multiple social media platforms based on user-provided post content and optional image analysis. It intelligently collects hashtag recommendations for Instagram, Facebook, X (Twitter), Reddit, and LinkedIn, consolidates them into a cohesive report, and outputs a professional summary suitable for email delivery or document export.

---

### 💡 Why Use Hashtag Suggestion Agent?
- Automatically generate platform-specific, optimized hashtag sets customized for your social media posts.
- Integrate optional AI-driven image content analysis to enhance hashtag relevance.
- Receive tiered hashtag breakdowns by niche, medium, and high reach categories.
- Consolidate results from multiple specialized AI agents (one per social platform) into a unified, professional report.
- Automate delivery of final hashtag strategy reports via email and Google Docs.
- Minimize manual research and increase engagement effectiveness on social platforms.
- Benefit from confidence scores per platform's hashtag recommendations to inform decision-making.

---

### ⚡ Who Is This For?
- Social media managers and marketing teams seeking consistent cross-platform hashtag strategies.
- CMOs or digital marketers looking to automate hashtag discovery based on post content.
- Agencies handling multi-platform campaigns requiring fast, data-driven hashtag recommendations.
- Content creators wanting to leverage AI for hashtag suggestions tailored to target audiences.
- Businesses aiming to scale social media optimization processes with minimal manual overhead.

---

### ❓ What Problem Does It Solve?
Manually generating effective hashtags for multiple social platforms is time-consuming and requires platform-savvy judgment. Hashtag effectiveness varies widely per platform and context. This workflow automates the complex task by aggregating platform-specific AI agents that generate precise hashtags based on post content and imagery, structuring output clearly by platform and tier, and delivering ready-to-use reports — thus improving social media reach, engagement, and strategy execution speed.

---

### 🔧 How This Workflow Works
1. **Trigger:** The workflow starts with a **Google Sheets Trigger**, monitoring a specific Google Sheet for new social media post entries containing Caption, Target Platforms, Post Category, Content Keywords, Image URL, and email address fields.
2. **Data Preparation:** The **Edit Fields** node standardizes these inputs for downstream processing.
3. **Conditional Image Analysis:** Checks if an Image URL exists using the **Has No Image URL?** If an image is present, it is analyzed by the **Google Gemini image analysis node** to generate a detailed description.
4. **Prompt Initialization:** The **Initialize Prompt Message** node crafts a comprehensive prompt combining caption, platform info, category, keywords, and optional image analysis to feed into the AI agents.
5. **Platform-Specific Hashtag Generation:** Separate AI agent tool nodes generate hashtags optimized per platform:
   - Instagram, Facebook, X (Twitter), Reddit, LinkedIn, and Meta Threads—each with tailored system instructions.
6. **Consolidation and Thinking:** The **Think** node orchestrates combining outputs from specialist agents.
7. **Chief Marketing Officer (CMO) Agent:** Aggregates all platform results, compiles a multi-platform hashtag report in a structured format including:
   - Subject line summarizing campaign theme.
   - Platform-by-platform hashtag sets, tiered breakdown (Niche, Medium, High-Reach).
   - Confidence scores.
8. **Parsing Output:** The **Parse JSON** node extracts and structures the CMO Agent output into JSON for downstream use.
9. **Report Finalization:** The **Finalize data** node builds a text report with hashtags grouped by tier and platform and summarizes confidence scores.
10. **Document Creation & Storage:** The **Create a document** and **Inserts the Output in the new File** nodes generate a Google Doc report stored in Google Drive.
11. **File Download & Email:** The **Download file** node retrieves the Google Doc; the **Finalize html + binary** node generates an HTML email body and attaches the document for email delivery. Finally, the **Send a message** node emails the report to the specified recipient.
12. **Workflow Completes:** User receives a professional, actionable hashtag strategy report tailored by platform with insights.

---

### 🔐 Setup Instructions
- ✅ **Google Sheets:** Set up a Google Sheet with appropriate columns: Timestamp, Caption, Target Platform(s), Image URL, Post Category, Content Keywords, and Email.
- ✅ **Google Sheets Trigger Node:** Connect to your Google Sheets account and specify the Form Response sheet ID and sheet name to monitor new entries.
- ✅ **Google Gemini Credentials:** Configure Google Gemini (PaLM API) credentials for image analysis and chat model calls.
- ✅ **Google Docs OAuth2:** Authorize Google Docs node to create and update report documents.
- ✅ **Google Drive OAuth2:** Permit downloading of created Google Docs document.
- ✅ **Gmail OAuth2:** Connect Gmail node for sending the finalized email with attachment.
- ✅ **LangChain Agents:** Verify access and integration to LangChain AI nodes configured for each social platform agent with tailored prompts.
- ✅ **Node Naming & Positioning:** Maintain node names or adapt as needed for clarity; positions do not affect logic but help visual workflow organization.

---

### 📅 Payload
| Key                 | Definition                                                                                          |
|---------------------|---------------------------------------------------------------------------------------------------|
| Caption             | Text content describing the social media post                                                     |
| Target Platform     | Platforms for hashtag generation (Instagram, Facebook, X, Reddit, LinkedIn, or “All platforms”)   |
| Image URL           | Optional URL pointing to an image accompanying the post                                           |
| Post Category       | Describes the theme or type of post (e.g., Team Culture, Product Launch)                           |
| Content Keywords    | Keywords that provide context or topics for hashtag generation                                    |
| Email               | Recipient’s email address to send the final report                                               |
| user_prompt         | AI prompt message constructed from inputs and image description                                  |
| output              | Aggregated hashtag strategy report generated by CMO Agent                                        |
| subject             | Short, engaging subject line summarizing the campaign                                            |
| platforms           | Array of platform-specific hashtag results, with hashtags, tiered breakdowns, and confidence score|

**Example JSON Payload:**
```json
{
  "Caption": "At New Star Tech, we believe that our greatest asset is our team...",
  "Target Platform": "All of the platforms mentioned",
  "Image URL": "https://www.aib.edu.au/wp-content/uploads/2019/03/form-submission-8115-shutterstock_447145060.jpg",
  "Post Category": "Team Culture / Behind the scenes",
  "Content Keywords": "team collaboration, healthy environment",
  "Email": "demoagent@nms.ph"
}
```

**Example cURL Test:**
```bash
curl -X POST https://your-n8n-instance.com/webhook/your-webhook-id \
 -H 'Content-Type: application/json' \
 -d '{
    "Caption": "Excited to unveil our new product launch!",
    "Target Platform": "Instagram, Facebook",
    "Post Category": "Product Launch",
    "Content Keywords": "new product, innovation",
    "Email": "your.email@example.com"
 }'
```

---

### 🔨 Tools/Nodes Used
- **Google Sheets Trigger:** Listens for new form entries as workflow input.
- **Edit Fields (Set Node):** Normalizes and extracts relevant fields from incoming data.
- **If Node ("Has No Image URL?"):** Branches workflow to include or skip image analysis, depending on presence of image.
- **Google Gemini (Image analysis):** Uses AI to describe image content, refining hashtag relevance.
- **Initialize Prompt Message (Code Node):** Builds user prompt text that combines text inputs and image description.
- **Social Media Specialist Agents (LangChain Nodes):** Separate AI agents specialized for Instagram, Facebook, Twitter (X), Reddit, LinkedIn, and Meta Threads, each producing suitable hashtags.
- **Think (LangChain Tool):** Aggregates and coordinates AI agent outputs.
- **CMO Agent (LangChain Agent):** Chief Marketing Officer AI agent that consolidates platform hashtag outputs into a formatted summary and email subject.
- **Parse JSON (Code Node):** Extracts structured data from CMO Agent output text.
- **Finalize data (Code Node):** Crafts a clean text report with tier breakdowns and confidence.
- **Google Docs Nodes:** Creates a new document and inserts report text.
- **Google Drive Node:** Downloads generated document for attachment.
- **Finalize html + binary (Code Node):** Produces the styled HTML email body and attaches the report document.
- **Gmail Node:** Sends final email with report as attachment.

---

### ⚙️ Reactive & Proactive Behavior
- **Reactive:** Automatically triggers on new Google Sheets form responses.
- **Proactive:** Processes input data, performs optional image analysis, and proactively generates multi-platform hashtag recommendations.
- Utilizes AI agents with tailored system instructions per social platform to ensure relevance.
- Aggregates multiple platform outputs into a unified, succinct report with clarity.
- Automatically sends final report by email without user intervention.

---

### 🐞 Error Handling
- **Missing Image:** Workflow branches around image analysis if no image URL provided, ensuring robustness.
- **Empty or Invalid Fields:** Initialization and script nodes sanitize inputs and provide default values to avoid crashes.
- **API or Credential Failures:** Require proper OAuth2 setup; failures trigger n8n error handling and alerts.
- **Hashtag Formatting:** Output is cleansed to avoid duplicates or banned hashtags per platform via agent rules.
- **Email Delivery:** Gmail node configured to properly attach document or send without attachment if download fails.
- **Timeouts:** Google API nodes and AI calls may timeout; n8n retries or user alerts recommended.

---

### 🧩 Requirements
- n8n environment configured with:
  - Access to Google Sheets, Google Docs, Google Drive, Gmail via OAuth2 credentials.
  - LangChain nodes configured for AI text generation.
  - Google Gemini (PaLM API) credentials with enabled image analysis and chat models.
  - Proper permissions on monitored Google Sheet and Drive folders.
- Structured input data conforming to expected form field names.
- Network connectivity to Google's APIs.
- User to supply recipient email addresses.

---

### 📚 Resources
- [n8n Official Documentation](https://docs.n8n.io)
- [Google Sheets API](https://developers.google.com/sheets/api)
- [Google Docs API](https://developers.google.com/docs/api)
- [Google Drive API](https://developers.google.com/drive/api)
- [Gmail API](https://developers.google.com/gmail/api)
- [Google Gemini / PaLM API](https://developers.generativeai.google/products/palm)
- [LangChain AI Node Integration Docs](https://docs.n8n.io/integrations/builtin/nodes/langchain/)
- [n8n Community Forum](https://community.n8n.io)

---

### 🐞 Troubleshooting
- **No hashtag output or empty report:** Check input data fields and prompt generation logic.
- **Image analysis not running:** Verify Image URL field, Google Gemini API credentials, and network.
- **Failed document creation:** Ensure Google Docs API access with granted scopes and folder permissions.
- **Email not sent:** Verify Gmail API credential, recipient email validity, and binary attachment correctness.
- **AI agents timeout or errors:** Confirm LangChain nodes are properly configured with correct API keys and prompts.
- **Incorrect or missing tier breakdowns:** Confirm parsing regex in 'Parse JSON' node matches output format.
- **Duplicate hashtags:** Confirm agent specialization and prompt adherence to avoid duplicates.
- **Raw output formatting (e.g., markdown tags) in final report:** Check CMO Agent system prompt for output guidelines.
- **Google Sheets trigger not firing:** Verify proper spreadsheet ID and worksheet name setup.
- **Platform-specific hashtags missing:** Validate each specialized AI tool’s system prompts and input messages.

---

> ℹ️ This section can be removed once you're fully satisfied with the content

### 📌 Recommended Improvements
- Introduce test cases or sample input entries to Google Sheet and automated validation step.
- Include logging or notification node to alert on failures or unusual response patterns.
- Provide a user-friendly front-end input form instead of raw Google Sheets for input data capture.
- Add support for additional platforms or expand hashtag tiering beyond three tiers.
- Extend email sending node to support multiple recipients or dynamic email content based on input.
- Add localization option for system messages and hashtags based on region or language.
- Improve modularity by splitting workflow into smaller sub-workflows per social platform.
- Integrate sentiment analysis or performance feedback loop to adjust future hashtag suggestions dynamically.

# END