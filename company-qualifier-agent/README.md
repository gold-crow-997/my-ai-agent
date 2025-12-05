> ⚠️ Kindly review the docs once more before saving.

## Company Qualifier  
![Latest-0.0.1](https://img.shields.io/badge/Latest-0.0.1-blue)  

An automated `n8n AI agent workflow` designed to qualify companies based on data scraped from Google Sheets and enriched via the Apollo API. It processes company records, verifies qualification criteria (e.g., company size), updates statuses in Google Sheets, and logs errors for disqualified entries.

---

### 💡 Why Use Company Qualifier?  
- Automates qualification of companies by integrating Google Sheets with Apollo's company data enrichment API.  
- Continuously monitors new or updated company entries for processing via Google Sheets triggers.  
- Efficiently batches records for scalable processing within n8n.  
- Automatically updates qualification statuses ("Qualified" or "Disqualified") in Google Sheets for clear tracking.  
- Logs any errors or unmatched companies for manual review, improving data accuracy and oversight.

---

### ⚡ Who Is This For?  
- Sales and marketing teams needing to qualify leads automatically from scraped data.  
- Data analysts managing company databases seeking automated enrichment and validation.  
- Automation specialists looking to integrate cloud spreadsheets with third-party APIs and AI logic.  
- n8n users wanting an example of linking triggers, filters, HTTP requests, and conditional logic in workflows.  

---

### ❓ What Problem Does It Solve?  
Manual company qualification based on scraped data is time-consuming, prone to errors, and difficult to scale. This workflow automates the process by:  
- Detecting new company entries in Google Sheets.  
- Leveraging Apollo's API for enriched company details such as industry, employee count, and revenue.  
- Applying qualification logic based on company size and other parameters.  
- Automatically updating the qualification status in sheets and logging unmatched companies.  

This reduces manual effort, improves data reliability, and accelerates sales readiness.

---

### 🔧 How This Workflow Works  
1. **Trigger:** Starts polling every minute for new rows added to the "Maps Scraped Data" sheet in Google Sheets.  
2. **Filter Record:** Retrieves all rows with the status "For Company Qualifying".  
3. **Batch Processing:** Splits these rows into manageable batches to loop over.  
4. **Input Retrieval:** For each record, fetch detailed input data from the "INPUT" sheet corresponding to the row ID.  
5. **Initialize Variables:** Converts comma-separated industries into arrays; prepares data for API call.  
6. **API Request:** Calls Apollo's mixed companies search API to enrich company data based on name and industry.  
7. **Qualification Logic:** Uses a code node to extract company size, revenue, phone, and checks if company meets size (≥50 employees) to qualify.  
8. **Conditional Branch:**  
   - If company qualified, it:  
     - Waits briefly,  
     - Adds enriched data to the "Output" sheet,  
     - Updates the company status to "Qualified" in "Maps Scraped Data".  
   - If not qualified ("Organization could not be found" or size too small), it:  
     - Waits briefly,  
     - Updates an error log sheet with company name and description,  
     - Marks status as "Disqualified" in the source sheet.  
9. **Loop Continuation:** Continues looping through batches for all records with "For Company Qualifying" status.

---

### 🔐 Setup Instructions  
- ✅ **Google Sheets Setup:** Ensure you have the following spreadsheets and sheets available with proper access:  
  - Source spreadsheet with "Maps Scraped Data" sheet for company entries.  
  - "INPUT" sheet containing detailed input data linked by row ID.  
  - "Company_Locator_Template" spreadsheet to log errors under "Error Log" sheet.  
  - Spreadsheet with an "Output" sheet to save qualified companies’ enriched data.  
- ✅ **Google Sheets Credentials:** Configure OAuth2 credentials for Google Sheets access in n8n and link to Google Sheets nodes.  
- ✅ **Apollo API Access:** Obtain an API key from Apollo for the company search API; set this in the HTTP Request node header (`x-api-key`).  
- ✅ **Polling Frequency:** The workflow triggers every minute to scan for new entries needing qualification. Customize intervals as per load.  
- ✅ **Node Names:** For clarity, node names correspond to key process steps (e.g., "Google Sheets Trigger", "Search Company via Apollo").  
- ✅ **Field Mappings:** Update column names in Google Sheets nodes if your sheets differ in structure. Matching columns and schema must align with your data.

---

### 📅 Payload  

| Key                 | Definition                                                    |  
|---------------------|---------------------------------------------------------------|  
| companyName         | Company name extracted/enriched from Apollo data               |  
| industry            | Industry tags, parsed into an array                            |  
| companySize         | Number of employees used as qualification criteria            |  
| revenue             | Company revenue (from Apollo or other attributes)             |  
| keyContacts         | Contact phone numbers from Apollo data                         |  
| qualificationStatus | Result of qualification logic: "Qualified" or "Not Qualified"  |  

**Example JSON Payload from Initialize Variables node:**  
```json
{
  "companyName": "Acme Corp",
  "industry": ["Saas"],
  "companySize": 120,
  "revenue": "$10M",
  "keyContacts": ["+1234567890"],
  "qualificationStatus": "Qualified"
}
```

---

### 🔨 Tools/Nodes Used  
- **Google Sheets Trigger:** Watches for new rows in Google Sheets "Maps Scraped Data".  
- **SplitInBatches:** Efficiently processes company data in smaller groups.  
- **Filter:** Selects rows with status "For Company Qualifying".  
- **Google Sheets (multiple):** Reads, updates, and appends data across various sheets.  
- **HTTP Request:** Calls Apollo API for company data enrichment using POST and API key.  
- **Code (JavaScript):** Implements qualification logic and data transformation on API response.  
- **If Node:** Branches workflow on qualification result.  
- **Wait Nodes:** Add delays between API calls and sheet updates to avoid rate limits and ensure data consistency.  

---

### ⚙️ Reactive & Proactive Behavior  
- **Reactive:** Automatically detects new company data rows as they appear in Google Sheets.  
- **Proactive:** Calls external Apollo API to enrich and validate company information.  
- **Batch processing:** Splits workload to enhance performance and prevent API/timeout issues.  

---

### 🐞 Error Handling  
- Companies not found or failing qualification update the error log sheet with relevant info.  
- Statuses are updated explicitly ("Qualified" or "Disqualified") to avoid ambiguity.  
- Wait nodes reduce API call frequency to mitigate request throttling errors.  
- The workflow loops continually to reprocess or catch missed entries.

---

### 🧩 Requirements  
- n8n environment (self-hosted or cloud).  
- Google Sheets OAuth2 credentials with read/write access.  
- Apollo API key with appropriate permissions.  
- Structured Google Sheets as per schema used in nodes.  
- Network connectivity for API calls.  

---

### 📚 Resources  
- [Apollo API Documentation](https://apollo.io/docs/api/)  
- [Google Sheets API for n8n](https://docs.n8n.io/integrations/builtin/app-nodes/n8n-nodes-base.googlesheets/)  
- [n8n Official Documentation](https://docs.n8n.io/)  

---

### 🐞 Troubleshooting  
- **No new processing happening:** Check trigger polling settings and sheet event triggers.  
- **API errors:** Verify your Apollo API key and rate limits; add longer wait times if frequent errors occur.  
- **Google Sheets updates fail:** Confirm OAuth2 credentials permissions and correct spreadsheet IDs.  
- **Qualification logic errors:** Review the "Initialize Variables" code node for correct field mapping and defaults.  
- **Unexpected empty data:** Ensure the expected sheets and columns exist and match node configuration exactly.  

---

> ℹ️ This section can be removed once you're fully satisfied with the content  

### 📌 Recommended Improvements  
- Add comments inside code nodes for clarity on business logic steps.  
- Introduce configurable environment variables for API keys, sheet IDs, and polling intervals to ease deployment.  
- Add error catch nodes for API calls to gracefully handle failures and retries.  
- Implement logging node to capture process runtime metrics.  
- Include enhanced data validation code inside "Initialize Variables" for edge cases.  
- Modularize the flow for lightweight testing of individual parts (e.g., test Apollo enrichment separately).  
- Add output node(s) to archive qualified companies to a backup sheet for audit purposes.  
- Incorporate webhook URLs to trigger flow externally when new data arrives (reduce polling overhead).  
- Document field schema assumptions explicitly in the README for end users to align spreadsheet structures.  
- Add an optional notification node (email/Slack) to alert users on disqualified companies or errors automatically.  

# END