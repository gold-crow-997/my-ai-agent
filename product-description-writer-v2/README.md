> ⚠️ Kindly review the docs once more before saving.

## Product Description Writer V2
![Latest-0.0.1](https://img.shields.io/badge/Latest-0.0.1-blue)

Product Description Writer V2 is an automated workflow designed in n8n that transforms structured product data files into SEO-optimized product descriptions and meta descriptions. It seamlessly integrates Google Drive for file management, Google Gemini AI for natural language generation, and Gmail for notification, delivering a full AI-powered product description pipeline with scoring and reporting.

![Product Description Writer V2 screenshot](./workflow.png)

> ⚠️ Note: This image is a sample and does not represent the actual workflow

---

### 💡 Why Use Product Description Writer V2?
- Automates creation of customer-focused, SEO-compliant product and meta descriptions.
- Supports multiple input file formats: CSV, XLS, XLSX.
- Uses Google Gemini AI model for high-quality natural language generation.
- Validates keyword usage with a custom SEO scoring node.
- Automatically uploads output back to Google Drive.
- Notifies stakeholders via Gmail on completion.
- Supports manual and trigger-based operation.
- Ensures strict keyword inclusion and style/tone adherence.

---

### ⚡ Who Is This For?
- E-commerce businesses needing scalable, quality product descriptions.
- Digital marketers optimizing SEO content for product catalogs.
- Content teams automating repetitive writing tasks.
- Developers integrating AI-generated content in workflows.
- Agencies managing large product inventories with minimal manual writing.

---

### ❓ What Problem Does It Solve?
Manually writing SEO-optimized product descriptions is time-consuming, inconsistent, and hard to scale. This workflow uses structured input data and AI to deliver consistent, keyword-rich, benefit-driven descriptions and meta descriptions automatically. It minimizes errors in keyword inclusion and maintains style tone, increasing productivity and SEO effectiveness.

---

### 🔧 How This Workflow Works
1. **Trigger:** Watches a specific Google Drive folder (/product-description) for new product data files.
2. **File Validation:** Checks that the uploaded file name matches allowed formats (`products_data.csv`, `products_data.xls`, `products_data.xlsx`).
3. **Download:** Downloads the valid file from Google Drive.
4. **Route & Extract:** Routes processing based on file extension, extracts product data accordingly.
5. **Preprocess:** Cleans and structures raw product fields; splits keywords into arrays.
6. **AI Generation:** Sends preprocessed data to the Google Gemini Chat AI agent to generate detailed product descriptions and meta descriptions following strict keyword rules.
7. **SEO Scoring:** Evaluates the output description's keyword coverage and length to assign an SEO score and produce an audit report.
8. **Output Preparation:** Converts output to an XLSX file with timestamped naming.
9. **Upload:** Uploads the final output XLSX back to Google Drive.
10. **Notify:** Sends Gmail notification to reviewers with download link.
11. **API Endpoint:** Supports triggering via a secure webhook with API key validation.

---

### 🔐 Setup Instructions
- ✅ **Google Drive OAuth2 Credential:** Connect Google Drive with OAuth for folder monitoring, download, and upload.
- ✅ **Google Gemini (PaLM) API Credential:** Provide API key for AI text generation.
- ✅ **Gmail OAuth2 Credential:** Configure Gmail OAuth for notification emails.
- ✅ **n8n Environment Variable:** Set `PRODUCT_WRITER_API_KEY` to a secure API key for webhook authentication (example: `nms-product-writer-test-key`).
- ✅ **Google Drive Folder:** Create and use folder with ID `1ROsLN-r3rNvhzwHTzWY2-mzMMDUMx6zU` for input/output files.
- ✅ **Place Input Files:** Upload product data files (`products_data.csv/xls/xlsx`) into that folder.
- ✅ **Webhook Setup (Optional):** Configure API webhook endpoint for remote triggering with API key.

---

### 📅 Payload
| Key            | Definition                                                    |
| -------------- | -------------------------------------------------------------|
| sku            | Product Stock Keeping Unit identifier                          |
| name           | Product Name                                                  |
| tone           | Style/Voice tone guide for description (e.g., Friendly)       |
| keywords       | Array of target keywords to be exactly included                |
| product_data   | Concatenated product specifications, features, material, etc. |
| target_url     | URL of the product                                            |
| description    | AI-generated product description (100-200 words)               |
| meta           | AI-generated meta description (under 160 characters)           |
| keywords_used  | Keywords successfully included in descriptions                 |
| seo_score      | Composite SEO score based on keyword coverage and length       |
| audit          | Detailed audit report (word count, keywords used/missing, coverage) |
| note           | Warnings or notes related to keyword inclusion                  |

**Example JSON Payload:**
```json
{
  "sku": "ABC123",
  "description": "This product offers exceptional features including keyword1 and keyword2...",
  "meta": "Quality product featuring keyword1 and keyword2, perfect for your needs.",
  "keywords_used": ["keyword1", "keyword2"]
}
```

**Example cURL Test:**
```bash
curl -X POST https://n8n.popapps.ai/webhook-test/product-description-writer-v2 \
  -H "Content-Type: application/json" \
  -d '{"apiKey": "nms-product-writer-test-key"}'
```

---

### 🔨 Tools/Node Used
- **Google Drive Trigger:** Watches for new files in `/product-description` folder.
- **If Node (Check File Type Validity):** Ensures file matches valid names (`products_data.csv/xls/xlsx`).
- **Switch Node (Route by File Format):** Routes execution depending on file extension.
- **Extract From File Node:** Extracts product data from CSV, XLS, or XLSX formats.
- **Code Node (Preprocess Product Data):** Cleans and formats data fields, splits keywords, prepares text.
- **Google Gemini Chat Model Node:** AI engine generating descriptions with strict keyword rules.
- **Structured Output Parser:** Parses Gemini output ensuring valid JSON structure.
- **Code Node (SEO Score Check):** Scores descriptions for keyword coverage and appropriate length.
- **Convert To File Node:** Creates final XLSX output with timestamp.
- **Google Drive Upload Node:** Uploads final description file back to Google Drive.
- **Gmail Notification Node:** Sends completion email with download link.
- **Webhook & Validation Nodes:** API entry point with API key authentication.
- **Sticky Notes:** Documentation and human-readable notes within workflow.

---

### ⚙️ Reactive & Proactive Behavior
- **Reactive:** Automatically triggers when a new product file is uploaded to Google Drive.
- **Proactive:** Supports manual triggering using secure webhook with API key.
- **Retry:** AI generation node is configured to retry on failure for robustness.

---

### 🐞 Error Handling
- Invalid file types are filtered early with If node to prevent processing errors.
- API key validation throws error for unauthorized webhook calls.
- SEO scoring warns if no keywords detected or used.
- Workflow does not run unless required credentials are configured.
- Nodes configured with retries to handle intermittent API failures.

---

### 🧩 Requirements
- n8n instance with internet access.
- Valid Google OAuth credentials for Drive and Gmail with proper scopes.
- Google Gemini API key for PaLM chat model.
- Access to specific Google Drive folder with read/write permission.
- Environment variable `PRODUCT_WRITER_API_KEY` set for webhook security.
- Input files must be named exactly `products_data.csv`, `.xls`, or `.xlsx`.

---

### 📚 Resources
- [n8n Documentation](https://docs.n8n.io/)
- [Google Drive API](https://developers.google.com/drive)
- [Google Gemini (PaLM API)](https://developers.generativeai.google/)
- [Gmail API](https://developers.google.com/gmail/api)
- [JavaScript Array.map() Reference](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map)

---

### 🐞 Troubleshooting
- **File Not Processing:** Verify file is uploaded with exact valid filename into monitored Google Drive folder.
- **No Keyword Matches:** Check if input keywords are correct and in the “Target Keywords” column.
- **API Authentication Fails:** Confirm the environment variable `PRODUCT_WRITER_API_KEY` matches the key sent in webhook calls.
- **Google Drive Credential Errors:** Reauthenticate Google Drive OAuth in n8n credentials.
- **Gemini API Errors:** Ensure your API key has sufficient quota and is properly set.
- **Emails Not Sent:** Verify Gmail OAuth credentials and their permissions.
- **Webhook Not Triggering:** Check n8n webhook URL and method, ensure no firewall blocking.
- **Output File Missing:** Confirm upload node’s configuration for folder and Drive ID.
- **SEO Score Low:** Review keywords used and product description length rules.
- **Retries on AI Node:** If AI generation node keeps failing, check internet connectivity and Gemini API status.

---

> ℹ️ This section can be removed once you're fully satisfied with the content

### 📌 Recommended Improvements
- Rename “Check File Type Validity” node to **“Validate Input Filename”** for clarity.
- Rename “Preprocess Product Data” node to **“Clean & Structure Product Fields”**.
- Rename “Generate Description + Meta (Gemini)” node to **“AI SEO Description Generator”**.
- Consolidate duplicate Google Drive OAuth credentials if possible.
- Add configurable environment variable for Google Drive folder ID for flexibility.
- Add more granular error reporting on keyword inclusion failures.
- Provide optional multi-language support in AI generation prompt.
- Add post-processing node to detect keyword density spikes.
- Enable webhook to accept parameters to customize tone/style dynamically.
- Include a node or logic for archiving processed input files.
- Add retry and alerting on file upload failures.
- Integrate a monitoring dashboard for workflow health and usage stats.
- Document all environment variables and credential requirements explicitly in a separate file.
- Include unit test nodes or manual trigger examples for easier development and debugging.
- Create a user-facing summary report email with SEO score analytics details.

# END