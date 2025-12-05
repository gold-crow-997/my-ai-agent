

## Competitor Social Media Monitor
![Latest-0.0.1](https://img.shields.io/badge/Latest-0.0.1-blue)

The **Competitor Social Media Monitor** is an AI-powered `n8n` workflow designed to automate the collection, categorization, and analysis of competitors' social media content across multiple platforms (TikTok, Facebook, Instagram, LinkedIn). It leverages scraping services and Google Gemini AI models to generate insightful reports on competitor performance and content trends.


---

### 💡 Why Use Competitor Social Media Monitor?
- Automatically scrape competitor posts from TikTok, Facebook, Instagram, and LinkedIn using Apify actors.
- Categorize posts into content types like Politics/News, Product Promotion, Entertainment, etc.
- Analyze engagement metrics (likes, shares, comments, views) to determine engagement levels.
- Use AI to extract sentiment, insights, and generate actionable summaries.
- Aggregate data into structured reports for performance tracking and content gap analysis.
- Push analyzed data back to Google Sheets for centralized monitoring.
- Create dynamic Google Docs reports with detailed performance and strategic insights.
- Notify users via email upon report generation for proactive awareness.

---

### ⚡ Who Is This For?
- Digital marketers and social media managers monitoring competitor activity.
- Competitive intelligence analysts seeking automated social listening.
- Content strategists identifying trending content and engagement patterns.
- Businesses aiming to benchmark social media performance versus competitors.
- Automation enthusiasts wanting to integrate AI-driven analytics in workflows.

---

### ❓ What Problem Does It Solve?
Monitoring competitors' social media involves manual effort across diversified platforms with disparate metrics. This workflow automates data acquisition, cleans and processes content, applies AI for deep analysis, and produces comprehensive performance reports. It consolidates insights to reveal top-performing content, content gaps, activity trends, and urgent alerts for marketing teams to act swiftly and effectively.

---

### 🔧 How This Workflow Works
1. **Trigger:** The workflow can be initiated manually.
2. **Read Competitor Data:** Fetches competitor social media profile URLs and seeds to Google Sheets.
3. **Platform-based Branching:**
   - Uses a Switch node to route data fetching per platform: TikTok, Facebook, Instagram, LinkedIn.
4. **Data Scraping:**
   - Invokes respective Apify scraping actors to collect recent posts.
5. **Data Mapping:**
   - Normalizes raw scraped data into unified post objects with author, text, likes, shares, comments, views, URLs, and date.
6. **Batch Processing:**
   - Splits data into batches for processing efficiency.
7. **AI Category Analysis:**
   - Sends batches to Google Gemini Chat Model AI nodes to assign categories, analyze engagement, sentiment, and generate insights.
   - Uses structured output parsers to ensure AI responses adhere to expected JSON schemas.
8. **Filtering:**
   - Filters posts according to initialized competitor profiles to avoid irrelevant data.
9. **Data Persisting:**
   - Saves categorized and analyzed data back into Google Sheets in a "Scraped Report" sheet.
10. **Data Structuring:**
    - Removes empty or irrelevant data.
    - Sorts and groups data by competitor and platform.
    - Creates a competitor-wise array for detailed analysis.
11. **Social Media Expert Analysis:**
    - Runs AI agent to produce four reports:
      - Top-Performing Posts Report (ranked by engagement).
      - Content Gap Analysis identifying underexploited themes.
      - Activity Scorecard summarizing posting behavior and engagement.
      - Urgent Alerts signposting anomalies or trend shifts.
12. **Report Generation:**
    - Creates a Google Docs report with AI-generated summary and insights per competitor.
    - Links generated report back to Google Sheets for tracking.
13. **Notifications:**
    - Sends an email notification with report access info to designated recipients.

---

### 🔐 Setup Instructions
- **Prerequisites:**
  - `n8n` instance with access to external nodes like Apify, Google Sheets, Google Docs, Gmail.
  - Google OAuth2 credentials configured for Sheets and Docs integrations.
  - Google Palm API credential created and connected for Google Gemini chat model usage.
  - Apify API account and integration setup to run required social media scraper actors.
  - Gmail OAuth credential for sending email notifications.
  - Competitor social media profiles listed in a Google Sheets document (used as input).
- **Steps to Deploy & Run:**
  1. Import the workflow JSON into your `n8n` instance.
  2. Configure all credentials:
     - Google Sheets and Docs OAuth2
     - Google Palm API (Google Gemini)
     - Apify API
     - Gmail OAuth2
  3. Customize Google Sheets document IDs and sheet names for input and output as per your environment.
  4. Provide the list of competitor profile URLs in the designated Sheets.
  5. Trigger the workflow manually or schedule it as per need.
  6. Verify output reports in Google Docs and summary data in Google Sheets.
  7. Check email notifications for success messages.
- **Credential Security:** Use secure vault secrets or environment variables for API keys.

---

### 📅 Payload
| Key                 | Definition                                                                                   |
|---------------------|----------------------------------------------------------------------------------------------|
| profiles            | Array of competitor profile URLs for scraping and filtering.                                |
| text                 | Post caption or text content from social media posts.                                        |
| hashtags             | String of comma-separated hashtags extracted from post text.                               |
| author               | Name or username of the author/creator of the post.                                         |
| url                  | Direct URL to the social post.                                                             |
| likes                | Number of likes received by the post.                                                      |
| shares               | Number of shares received by the post.                                                     |
| comments             | Number of comments/responses on the post.                                                 |
| views                | View count where applicable (e.g., videos).                                                |
| date                 | Date and time when the post was created/published.                                        |
| Category             | AI-generated category label (e.g., Politics/News, Product Promotion).                       |
| EngagementLevel      | AI-assessed engagement level (e.g., Low, Moderate, High).                                 |
| Sentiment            | AI-determined sentiment or tone (e.g., Informative, Promotional, Critical).                |
| Insight              | AI-generated insight summarizing key observations about the post's performance or content. |

**Example JSON Payload:**
```json
{
  "Category": "Politics / News",
  "Author": "rappler",
  "URL": "https://www.tiktok.com/@rappler/video/7562448720797650183",
  "Likes": "9617",
  "Shares": "902",
  "Comments": "270",
  "Views": "111500",
  "Date": "2025-10-18T06:43:28.000Z",
  "EngagementLevel": "High",
  "Sentiment": "Critical",
  "Insight": "The post achieved high engagement due to its strong political stance and topical relevance."
}
```

**Example cURL Test:**
```bash
curl -X POST https://api.example.com/analyze \
  -H 'Content-Type: application/json' \
  -d '{
    "text": "Some sample post caption text",
    "hashtags": "#example, #test",
    "author": "example_author",
    "url": "https://socialmedia.com/example_post",
    "likes": 123,
    "shares": 45,
    "comments": 6,
    "views": 789,
    "date": "2025-06-01T12:00:00Z"
  }'
```

---

### 🔨 Tools/Node Used
- **Manual Trigger:** Initiates the workflow manually.
- **Google Sheets Nodes:** Read competitor profiles and write analyzed data and reports links.
- **Switch Node:** Routes input based on social media platform.
- **Apify Nodes:** Invoke dedicated actors to scrape social media posts from LinkedIn, TikTok, Facebook, Instagram.
- **Code Nodes (JavaScript):** Process and normalize raw scraped data, filter relevant profiles, and restructure grouped data.
- **SplitInBatches:** Handles batch processing of large data sets.
- **Google Gemini Chat Model Nodes:** AI language model nodes for categorizing posts and generating analysis.
- **Structured Output Parsers:** Ensure AI outputs conform to required JSON structure.
- **Filter Nodes:** Refine data by matching to known competitor profiles.
- **Merge and Sort Nodes:** Combine and order datasets.
- **Google Docs Nodes:** Create and update detailed competitor performance reports.
- **Gmail Node:** Send notifications with report details.

---

### ⚙️ Reactive & Proactive Behavior
- **Reactive:**
  - Triggered manually to perform real-time competitor data monitoring.
  - Filters inputs to process only relevant competitor data dynamically.
- **Proactive:**
  - Generates insights and alerts from AI agent based on data trends.
  - Automatically crafts reports highlighting opportunities and risks.
  - Sends notifications for timely user awareness.

### 🐞 Error Handling
- The workflow includes filtering to remove empty or irrelevant data.
- Node execution depends on successful data retrieval; failure in scraping will stop downstream processes.
- Use n8n’s built-in retry and error handling mechanisms to manage API failures or quota limits.
- Monitor API quota (Google Palm, Apify) to avoid disruptions.
- Verify and update credentials regularly.

---

### 🧩 Requirements
- n8n v0.195+ with support for Google, Apify, and Gmail nodes.
- Google Sheets with competitor profile URLs and Google Docs for reports.
- Apify account to run scraping actors:
  - TikTok Scraper
  - Facebook Posts Scraper
  - Instagram Scraper
  - LinkedIn Company Posts Scraper
- Google Palm API access for AI models (Google Gemini).
- Gmail account for notification emails.
- JavaScript knowledge to tweak normalization codes if needed.

---

### 📚 Resources
- [n8n Documentation](https://docs.n8n.io)
- [Google Sheets API](https://developers.google.com/sheets/api)
- [Google Docs API](https://developers.google.com/docs/api)
- [Apify Platform](https://apify.com)
- [Google PaLM API](https://developers.generativeai.google)
- [Gmail API](https://developers.google.com/gmail/api)
- [Google Gemini Chat Model Docs](https://developers.generativeai.google/models)

---

### 🐞 Troubleshooting
- **AI Authentication Errors:** Check Google Palm API credentials, token validity.
- **Apify Scraper Fails:** Verify Apify API key, actor IDs, and input format.
- **Google Sheets Access Denied:** Ensure OAuth scopes and sharing permissions.
- **Rate Limits:** Monitor and increase quotas as needed.
- **Empty Reports:** Confirm competitor profile URLs are entered correctly.
- **Slow Execution:** Use batch processing settings to optimize performance.
- **Data Mismatch:** Debug filter and code nodes to ensure proper JSON data structure.
- **Notification Failures:** Validate Gmail OAuth and recipient email correctness.
- **Workflow Inactive:** Activate the workflow and confirm all nodes are enabled.
- **Unexpected Node Failures:** Review n8n logs and enable node execution snapshots.

---

> ℹ️ This section can be removed once you're fully satisfied with the content

### 📌 Recommended Improvements
1. Rename **"When clicking ‘Execute workflow’"** node to **"Manual Trigger (Start)"** for clarity.
2. Standardize naming for **Code nodes**, e.g., "Normalize TikTok Data", "Normalize Facebook Data".
3. Create a reusable **"Initialize Profiles"** node grouping to consolidate profile extraction logic.
4. Add comments inside each **Code node** to explain transformations more explicitly.
5. Include fallback or retry logic in scraper calls to handle API transient failures.
6. Add logging nodes after major steps for easier troubleshooting.
7. Abstract AI prompt template to a dedicated node for easier adjustment.
8. Implement a global error workflow to capture and notify failed runs.
9. Introduce pagination handling in scrapers for comprehensive data gathering.
10. Provide environment variable support for API keys and sheet IDs to ease deployment.
11. Add configuration UI node for user to select competitors dynamically.
12. Separate AI classification by social media platform to enable finer-tuned models.
13. Consider adding sentiment calorimeter visualization within reports.
14. Configure scheduling in n8n with cron trigger for automated updates.
15. Validate all URLs and inputs before scraping to reduce errors.
16. Add a summary email with report highlights alongside full docs link.
17. Enable multi-language support in AI prompts for wider use.
18. Add support for additional social platforms like Twitter or YouTube.
19. Integrate data visualization tools (Charts) for richer reports.
20. Cache AI outputs to avoid duplicated API costs.

# END