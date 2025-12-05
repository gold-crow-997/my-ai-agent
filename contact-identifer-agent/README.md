
## Contact Identifier - AI Agent
![Latest-0.0.1](https://img.shields.io/badge/Latest-0.0.1-blue)

This `n8n` workflow automates the process of identifying and enriching contact information from a Google Sheets source using Apollo's API. It filters qualified contacts, enriches their data with job titles ranked by seniority, and saves the refined contact information back into Google Sheets. This integrates AI-based person searches to optimize and validate contact data automatically.

---

### 💡 Why Use Contact Identifier - AI Agent?
- Automates filtering and enrichment of contacts based on qualification status.
- Uses Apollo.io's API to perform intelligent person searches and data enrichment.
- Ranks job titles by seniority to aid prioritization in contact management.
- Ensures contact and company data consistency by appending or updating Google Sheets records.
- Saves time and reduces manual data entry errors in CRM or sales pipeline enrichment.

---

### ⚡ Who Is This For?
- Sales and marketing professionals looking to automate contact data enrichment.
- Business development teams requiring up-to-date contact qualification.
- Data analysts managing large lead databases seeking automation.
- Any user of `n8n` wanting to integrate Google Sheets with Apollo API for contact management.

---

### ❓ What Problem Does It Solve?
This workflow addresses the challenge of maintaining high-quality, qualified contact data by automating:
- Identification of qualified leads from Google Sheets.
- Retrieval of enriched contact information via Apollo’s AI-powered search.
- Prioritization of contacts based on job title seniority.
- Syncing modifications back to Google Sheets instantly.

---

### 🔧 How This Workflow Works
1. **Google Sheets Trigger**: Continuously polls a Google Sheet ("Apollo Scraped data") every minute to detect new or updated contacts.
2. **Filter Node**: Filters the incoming data so only rows marked "Qualified" in their "Qualification Status" proceed further.
3. **Initialize Job Titles**: Parses and normalizes job titles from the filtered data, preparing it for the AI search query.
4. **Search People in Apollo**: Sends a POST request to Apollo’s API to search for people based on parsed job titles and company info.
5. **Rank Job Titles**: Processes the returned people data, sorting contacts by seniority rank (from C-level to intern).
6. **Edit Fields**: Extracts and maps key contact fields (company name, contact name, job title, email, phone, LinkedIn URL) from the top-ranked contact.
7. **Save Contact Info**: Appends or updates the “Contacts” Google Sheet with enriched contact details and marks campaign status as "Unassigned."
8. **Update Status: Contact Info Found**: Updates the original Google Sheet to mark the "Qualification Status" as "Contact Found," signaling successful enrichment.

---

### 🔐 Setup Instructions
- ✅ Set up an `n8n` instance with access to:
  - Google Sheets account (OAuth 2 credentials authorized for reading/writing specific spreadsheets).
  - Apollo.io API key for the HTTP Request node.
- ✅ Prepare the Google Sheets:
  - "Apollo Scraped data" with contacts and a "Qualification Status" column.
  - "Contacts" sheet for saving enriched contact info.
- ✅ Configure credentials:
  - Create and link Google Sheets OAuth2 credential in n8n for triggering and updating sheets.
  - Insert Apollo API key securely in the HTTP Request node headers.
- ✅ Adjust polling frequency or sheet IDs as required.
- ✅ Deploy and activate the workflow in `n8n`.

---

### 📅 Payload
| Key             | Definition                                   |
|-----------------|----------------------------------------------|
| Company Name    | The organization name extracted or enriched.|
| Contact Name    | Name of the contact person.                   |
| Job Title      | Job title of the contact, used for ranking.   |
| Email Address  | Contact’s email address.                       |
| Phone Number   | Primary phone number from the contact’s org. |
| LinkedIn URL   | LinkedIn profile URL of the contact.          |
| Qualification Status | Status flag indicating contact qualification state.|
| Campaign Status | Status of the contact in campaign workflow.  |
| person_titles  | Array of job titles extracted for API queries.|

**Example JSON Payload (for Apollo Search):**
```json
{
  "person_titles": ["CEO", "CTO"],
  "q_organization_name": "Example Company",
  "q_organization_location": "New York",
  "page": 1,
  "per_page": 5
}
```

---

### 🔨 Tools/Node Used
- **Google Sheets Trigger**: Monitors Google Sheets for updates every minute.
- **Filter**: Passes only rows with "Qualified" status for further processing.
- **Code Node (Initialize Job Titles)**: Parses and prepares job titles array for API requests.
- **HTTP Request (Search People in Apollo)**: Calls Apollo API with company and titles.
- **Code Node (Rank Job Titles)**: Sorts returned contacts by seniority for prioritization.
- **Set (Edit Fields)**: Maps selected contact fields into structured outputs.
- **Google Sheets (Save Contact Info)**: Adds or updates contact records.
- **Google Sheets (Update Status)**: Updates original qualification status to mark successful enrichment.

---

### ⚙️ Reactive & Proactive Behavior
- Reactive: Triggered by changes in Google Sheets, ensuring near real-time updates.
- Proactive: Queries Apollo API to enrich data actively rather than passively waiting for manual input.

### 🐞 Error Handling
- The workflow includes implicit validation by filtering only qualified contacts.
- API errors or empty results may cause downstream nodes to have no data—ensure monitoring and logging in n8n designed for error alerts.
- OAuth re-authentication required periodically for Google Sheets.

---

### 🧩 Requirements
- n8n Platform (self-hosted or cloud) with execution permission.
- Valid Apollo.io API key with permissions to access people search endpoints.
- Google account with access to two Google Sheets (source and target).
- Proper OAuth credentials configured in n8n for Google Sheets usage.
- Network access to Apollo API endpoints.

---

### 📚 Resources
- [n8n Documentation - Google Sheets Node](https://docs.n8n.io/integrations/n8n-nodes-base.google-sheets/)
- [Apollo.io API Reference](https://docs.apollo.io/)
- [n8n Documentation - HTTP Request Node](https://docs.n8n.io/nodes/n8n-nodes-base.httprequest/)
- [n8n Documentation - Workflow Automation](https://docs.n8n.io/)

---

### 🐞 Troubleshooting
- If no contacts are processed, verify the "Qualification Status" column exactly matches "Qualified" (case-sensitive).
- Confirm Google Sheets credentials have correct read/write access.
- Test Apollo API key with a direct HTTP client to ensure valid and active.
- Check execution logs for skipped nodes or empty datasets after filter nodes.
- Confirm Google Sheet IDs and sheet names/gids are correctly configured.
- Debug job title parsing if API queries return unexpected empty responses.
- Monitor rate limits and quota restrictions on Apollo API.
- Re-authenticate Google OAuth credentials if access errors occur.
- Use manual workflow execution to validate individual segments.
- Ensure all required fields exist in Google Sheets before running workflow.

---

> ℹ️ This section can be removed once you're fully satisfied with the content

### 📌 Recommended Improvements

- Add explicit error handling nodes for API failures and invalid data.
- Implement alerting or notification steps on failed workflow executions.
- Add configuration parameters for polling intervals and API pagination.
- Add comments or descriptions to code nodes for easier maintenance.
- Include field validation steps for malformed emails or phone numbers.
- Enable logging of API responses for audit and troubleshooting.
- Implement data deduplication checks before updating Google Sheets.

# END