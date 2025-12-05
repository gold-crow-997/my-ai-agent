> ⚠️ Kindly review the docs once more before saving.

## SEO Keywords Optimizer  
![Latest-0.0.1-blue](https://img.shields.io/badge/Latest-0.0.1-blue)

An automated SEO analysis and optimization workflow that leverages AI to analyze a draft content’s target keyword against live SERP results, producing actionable SEO improvement recommendations and a detailed HTML report sent via email.

![SEO Keywords Optimizer screenshot](./workflow.png)

> ⚠️ Note: This image is a sample and does not represent the actual workflow

---

### 💡 Why Use SEO Keywords Optimizer?
- Automatically fetches current top Google search results for your target keyword  
- Uses AI (Google Gemini model) to analyze draft content relative to competitors  
- Identifies missing subtopics, long-tail keywords, and SEO score to enhance content  
- Generates a structured SEO report in HTML format  
- Sends the SEO report via Gmail automatically  
- Provides webhook API integration for standardized entry and validation  

---

### ⚡ Who Is This For?
- Content marketers and SEO specialists looking to improve organic rankings  
- Digital agencies wanting automated content evaluation workflows  
- Website owners tracking keyword relevancy and content gaps  
- Developers building AI-powered SEO automation tools  

---

### ❓ What Problem Does It Solve?
Manually analyzing SEO effectiveness of content relative to competitors can be tedious and subjective. This workflow automates the evaluation and scoring process using real-time search data and advanced AI analysis, streamlining keyword research and content refinement.

---

### 🔧 How This Workflow Works
1. **Webhook Trigger & API Key Validation:** Receives standardized POST requests with API key validation for access control (does not start main workflow).  
2. **Manual Trigger Start:** The actual SEO analysis begins here via a trigger node for controlled executions.  
3. **Input Setup:** Static content draft, keyword, and persona are set via a Set node (can be replaced with dynamic inputs).  
4. **Google SERP Fetch:** Uses Google Custom Search API to fetch current top search results for the keyword.  
5. **Parse SERP Results:** Extracts essential details (title, link, snippet) from raw API response for competitor context.  
6. **SEO Analyzer (AI Agent):** Sends the draft content, keyword, and competitor SERP snippets to the Google Gemini Chat Model for SEO score analysis and actionable recommendations in JSON format.  
7. **Generate HTML Report:** Uses JavaScript to format AI output into a styled, user-friendly HTML report summarizing scores, missing topics, keywords, and recommendations.  
8. **Send SEO Report via Gmail:** Sends the formatted report to a predefined email address automatically.  

---

### 🔐 Setup Instructions
- ✅ **Create and configure credentials in n8n:**
  - Google Custom Search API (API Key and CX/Search Engine ID)  
  - Gmail OAuth2 Credential for sending emails  
  - Google Gemini (PaLM) API key for AI model access  
- ✅ **Add environment variables:**
  - `SEO_OPTIMIZER_API_KEY` with your secret API key for webhook validation  
- ✅ **Set draft content and keyword:**  
  - Replace content and keyword in the “Input - Draft & Keyword” Set node for quick testing or connect this node to receive dynamic inputs (e.g., from webhook or external forms).  
- ✅ **Update the Gmail node recipient with your desired recipient email.**  
- ✅ **Activate the workflow and run manually to test or integrate webhook calls for triggering validation and documentation purposes.**  

---

### 📅 Payload
| Key                   | Definition                                 |
|-----------------------|--------------------------------------------|
| content               | Text draft content to be analyzed          |
| keyword               | The target SEO keyword                      |
| persona               | Intended audience/persona for the content  |
| score                 | SEO score between 0 and 100                 |
| missing_subtopics      | List of content subtopics missing from draft|
| long_tail_keywords     | Suggested long-tail keywords to target      |
| recommendations       | SEO improvement recommendations as actionable text|

**Example JSON Payload (Input to SEO Analyzer):**  
```json
{
  "content": "Artificial Intelligence is changing SEO rapidly...",
  "keyword": "AI content optimization",
  "persona": "Marketing professional"
}
```

**Example JSON Response from AI (SEO Analysis):**  
```json
{
  "score": 85,
  "missing_subtopics": ["AI tools overview", "SEO trends 2024"],
  "long_tail_keywords": ["AI SEO tools comparison", "optimize AI content"],
  "recommendations": [
    "Add more internal links",
    "Use more examples",
    "Include latest keyword trends"
  ]
}
```

**Example cURL to test webhook validation:**  
```bash
curl -X POST https://your-n8n-instance/webhook/seo-keywords-optimizer \
  -H 'Content-Type: application/json' \
  -d '{"apiKey": "seo_optimizer_test_key"}'
```

---

### 🔨 Tools/Node Used
- **Webhook:** Standardized API entry point for external requests with API key validation  
- **Code (validation):** Validates API key for secure access control before responding  
- **Manual Trigger:** Starts the actual SEO content analysis workflow manually  
- **Set Node:** Defines input draft content and keyword for analysis  
- **HTTP Request:** Fetches Google SERP data via the Custom Search JSON API  
- **Code (Parse SERP):** Extracts relevant search results snippets from raw response  
- **Langchain Google Gemini Chat Model:** AI language model for SEO analysis and scoring  
- **SEO Analyzer Agent:** Node that couples AI output parser and sends final JSON to next steps  
- **Code (Generate HTML):** Formats the AI-generated JSON into a readable HTML report  
- **Gmail:** Sends the SEO report to designated recipient(s)  

---

### ⚙️ Reactive & Proactive Behavior
- **Reactive:** Waits for manual trigger or webhook validation before proceeding  
- **Proactive:** Automatically emails SEO report once analysis and report generation completes  

### 🐞 Error Handling
- API key mismatch throws explicit authorization error blocking further process  
- Downstream nodes rely on structured AI JSON output; malformed AI responses could cause failures (recommend retry or manual check)  
- HTTP request errors should be monitored for quota or key validity  

---

### 🧩 Requirements
- n8n instance version compatible with Langchain and Google APIs  
- Google Custom Search API enabled with valid API Key and Search Engine ID (CX)  
- Gmail account configured with OAuth2 for report sending  
- Google Gemini API key set up for LLM requests  
- Environment variable for workflow API key validation: `SEO_OPTIMIZER_API_KEY`  
- Manual trigger permission or alternative triggering mechanism  

---

### 📚 Resources
- [Google Custom Search API Documentation](https://developers.google.com/custom-search/v1/overview)  
- [Google Gemini (PaLM) API Reference](https://developers.generativeai.google/api/gemini)  
- [n8n.io Official Documentation](https://docs.n8n.io/)  
- [Gmail OAuth2 Setup Guide](https://developers.google.com/identity/protocols/oauth2)  
- [Langchain n8n Nodes Overview](https://github.com/n8n-io/n8n-nodes-langchain)  

---

### 🐞 Troubleshooting
- **Workflow not starting:** Ensure manual trigger node is activated or webhook calls use correct API key  
- **No data in email:** Verify Google Custom Search API quota and response correctness  
- **AI analysis fails:** Check Gemini API key validity and usage limits  
- **Email not sending:** Confirm Gmail OAuth credential and user permissions  
- **Malformed JSON from AI:** Adjust prompt or validate network communication to Google Gemini  
- **Environment variables missing:** Confirm `SEO_OPTIMIZER_API_KEY` is set in n8n environment  
- **Webhook endpoint errors:** Check path and method configuration in webhook node  

---

> ℹ️ This section can be removed once you're fully satisfied with the content

### 📌 Recommended Improvements  
- Rename “Input - Draft & Keyword” to **"Input Content & Target Keyword"** for clarity  
- Rename "Google SERP Fetch" to **"Fetch Google Search Results"** for readability  
- Rename "Parse SERP Results" to **"Extract SERP Insights"** to reflect function  
- Rename "SEO Analyzer" to **"AI SEO Content Analyzer"** to clarify purpose  
- Rename "Generate HTML Report" to **"Format SEO Report (HTML)"**  
- Rename "Send SEO Report" to **"Email SEO Report"**  
- Enhance webhook node description and provide option to start full workflow in future versions  
- Add dynamic input support to replace static Set node for real-time content and keywords  
- Implement retry logic on AI calls for improved reliability  
- Integrate logging on error nodes for easier debugging  
- Add UI form or trigger options for non-technical users to input draft/keyword  
- Include token usage monitoring for Gemini API to control costs  
- Add workflow versioning and changelog embedded in a sticky note or metadata  
- Clean up sticky note to summarize usage and replace confidential placeholders with environment variable references  
- Enable workflow activation flag by default for quick testing after deployment  

# END