
## Scrapper to Outreacher Agent
![Latest-0.0.1](https://img.shields.io/badge/Latest-0.0.1-blue)

This `n8n AI agent workflow` automates the process of identifying qualified companies from scrapped map data, enriching company information and contacts via Apollo API, and managing outreach campaigns through Woodpecker. It integrates Google Sheets for data storage and status tracking, ensuring seamless data flow and campaign management.


---

### 💡 Why Use Scrapper to Outreacher Agent?
- Automate qualification of companies listed in map scrapped data based on size and industry parameters.
- Enrich company and contact data leveraging Apollo API.
- Automatically log companies as qualified or disqualified in Google Sheets.
- Identify and save key contacts associated with qualified companies.
- Integrate with Woodpecker to automate sending emails through campaigns.
- Maintain campaign prospect lists in sync with contact data.
- Update campaign and contact statuses dynamically based on interactions.
- Handle error logging transparently into a dedicated Google Sheet.
- Support batch processing to scale with large datasets.
- Provide wait steps to control workflow pace and rate-limit external API calls.

---

### ⚡ Who Is This For?
- Sales and marketing teams seeking streamlined lead qualification and outreach automation.
- Business development professionals aiming to scale outreach campaigns effortlessly.
- Data analysts who manage and enrich marketing contact lists.
- Automation engineers building AI-enhanced workflows for CRM and marketing tools.
- Small to medium enterprises wanting to integrate data scraping, qualification, and outreach without manual effort.

---

### ❓ What Problem Does It Solve?
Manually qualifying companies, enriching contact data, logging results, and managing outreach campaigns is labor-intensive and error-prone. This workflow automates these entire steps, ensuring accurate qualification, efficient contact enrichment, and smooth campaign management with minimal manual intervention.

---

### 🔧 How This Workflow Works

1. **Trigger:** Initiates when a new row is added to the "Maps Scrapped Data" Google Sheet.
2. **Filter for Qualification:** Filters rows with status "For Company Qualifying" to focus the workflow.
3. **Batch Processing:** Loops over items in batches for scalability.
4. **Apollo Company Search:** For each company, uses Apollo API to search and get company details.
5. **Qualification Check:** Evaluates company size and other details to determine qualification status.
6. **Log Results:** Updates either "Apollo Scraped Data" or "Error Log" sheets depending on qualification.
7. **Contact Discovery:** For qualified companies, retrieves job titles, searches people on Apollo.
8. **Contact Sorting:** Sorts contacts by seniority ranking to prioritize outreach.
9. **Save Contact Info:** Cleans and stores contact details in the "Contacts" Google Sheet and updates company status.
10. **Campaign Control:** Stops any running Woodpecker campaign.
11. **Prospect Management:** Fetches unassigned contacts, adds them as prospects to the Woodpecker campaign.
12. **Update Contact Status:** Marks contacts as "Sent An Email" post outreach.
13. **Campaign Restart:** Automatically runs the Woodpecker campaign again to start outreach.
14. **Error Handling:** Logs companies that cannot be found or not qualified in an error sheet.

---

### 🔐 Setup Instructions

- ✅ **Google Sheets Setup:**
  - Prepare a Google Sheet titled `Company_Locator_Template` with sheets:
    - "Maps Scrapped Data" (trigger source)
    - "Apollo Scraped Data" (enriched company data)
    - "Contacts" (contact info)
    - "Error Log" (logging companies with issues)
    - "INPUT" (for user inputs like Job Titles)
  - Connect and authenticate Google Sheets in n8n via OAuth2 credentials (Google Sheets account 3).
- ✅ **Apollo API:**
  - Acquire an Apollo API key and assign it in the HTTP Request nodes (`x-api-key` header).
- ✅ **Woodpecker API:**
  - Get Woodpecker.io API key and configure in HTTP Request nodes for campaign actions.
  - Adjust `campaign_id` in Woodpecker requests to your campaign.
- ✅ **n8n Workflow:**
  - Import this workflow JSON.
  - Authenticate and link external API credentials for Google Sheets, Apollo, and Woodpecker.
- ✅ **User Inputs:**
  - Define job titles and other conditions manually in the "INPUT" sheet for qualification criteria.
- ✅ **Permissions:**
  - Ensure the Google Sheets and API services have the necessary permissions to read/write.

---

### 📅 Payload
| Key                 | Definition                                                   |
|---------------------|--------------------------------------------------------------|
| COMPANY NAME        | Name of the company from scrapped maps or Apollo API search  |
| STATUS              | Qualification status: For Company Qualifying, Qualified, etc.|
| Industry            | Industry category of the company                              |
| Company Size        | Number of employees                                           |
| Revenue             | Estimated or reported revenue                                 |
| Key Contacts        | Phone numbers or emails of key contacts                       |
| Job Title           | List of desired job titles to find contacts for              |
| Contact Name        | Name of the contact person                                    |
| Email Address       | Contact's email address                                       |
| Phone Number        | Contact phone number                                          |
| LinkedIn URL        | Contact's LinkedIn profile link                               |
| Campaign Status     | Current outreach status of the contact (Unassigned, Sent An Email, etc.)|

**Example JSON Payload for Apollo Company Search:**
```json
{
  "q_organization_name": "Example Company",
  "organization_num_employees_ranges": [
    "50-100",
    "101-250",
    "251-500",
    "501-1000",
    "1001-5000",
    "5001-10000",
    "10000+"
  ],
  "industry_tag_names": ["SaaS"],
  "page": 1,
  "per_page": 1,
  "enrich_persons": true
}
```

**Example cURL Test for Apollo Company Search:**
```bash
curl -X POST "https://api.apollo.io/api/v1/mixed_companies/search" \
  -H "Accept: application/json" \
  -H "Cache-Control: no-cache" \
  -H "x-api-key: YOUR_API_KEY" \
  -d '{
        "q_organization_name": "Example Company",
        "organization_num_employees_ranges": ["50-100","101-250"],
        "industry_tag_names": ["SaaS"],
        "page": 1,
        "per_page": 1,
        "enrich_persons": true
      }'
```

---

### 🔨 Tools/Node Used

- **Google Sheets Trigger:** Triggers workflow on new rows in scrapped data sheet.
- **Filter & If Nodes:** Conditional routing based on status and qualification.
- **SplitInBatches:** Allows efficient processing of multiple records in batches.
- **HTTP Request Nodes:** Connects to Apollo and Woodpecker APIs for data retrieval and campaign management.
- **Code Nodes:** Format and transform data (e.g., parse job titles, sort contacts by seniority).
- **Set Node:** Cleans and prepares fields for output.
- **Google Sheets Node:** Append or update rows in various sheets for logging and data storage.
- **Wait Nodes:** Provide delay to control pacing and avoid rate-limiting.
- **Sticky Notes:** Annotate workflow zones for clarity.

---

### ⚙️ Reactive & Proactive Behavior

- *Reactive:* Trigger responds to new rows added in the maps scraped data sheet.
- *Proactive:* Periodically polls Google Sheets or API endpoints.
- Dynamic status updates drive flow changes and campaign execution.
- Batching allows volume management and timely updates.

---

### 🐞 Error Handling

- Companies not found trigger logging into "Error Log" sheet with description.
- Qualification status controls pathway to disqualify or qualify companies.
- Campaign and contact status updates ensure consistent workflow state management.
- Wait nodes reduce risk of API throttling and transient failures.

---

### 🧩 Requirements

- Google Sheets document with named sheets as per workflow setup.
- Apollo API key with access to company and people search endpoints.
- Woodpecker API key with campaign management permissions.
- n8n installed and configured for OAuth2 Google Sheets credentials.
- Proper network access for n8n to reach external APIs.

---

### 📚 Resources

- [Apollo API Docs](https://apolloio.docs.apiary.io/)
- [Woodpecker API Documentation](https://docs.woodpecker.co/api/#introduction)
- [n8n Documentation](https://docs.n8n.io/)
- [Google Sheets API](https://developers.google.com/sheets/api)
- [Google OAuth2 Setup for n8n](https://docs.n8n.io/credentials/n8n-nodes-base.googleSheetsOAuth2Api/)

---

### 🐞 Troubleshooting

- Ensure API keys are valid and not expired.
- Check Google Sheet IDs and sheet names match the workflow configuration.
- Verify all OAuth2 credential authentications in n8n are successful.
- Monitor workflow execution logs for HTTP errors or timeouts.
- Confirm that job titles and qualification criteria in the "INPUT" sheet are populated.
- Watch out for quotation or formatting errors in JSON body requests.
- API rate limits can cause failures — adjust wait times or batch sizes as needed.
- Make sure Woodpecker campaign IDs are correct and campaigns exist.
- Google Sheets permissions must allow edit, append, and update operations by the connected account.
- The seniority ranking in the code node can be customized to better fit your industry contacts.

---

> ℹ️ This section can be removed once you're fully satisfied with the content

### 📌 Recommended Improvements

- Add comments in code nodes for better maintainability and explanation of logic.
- Create a node grouping or use tags to cluster related nodes (e.g., "Company Qualification", "Contact Enrichment", "Campaign Management").
- Provide input validation node after Google Sheets trigger to check data integrity early.
- Optimize batch sizes configurable via a parameter node for scaling performance.
- Add retry logic or alerting on API failures for robustness.
- Centralize API key management in environment variables or credentials manager.
- Include a node to notify users via email or Slack in case of critical errors or completion.
- Enable detailed logging and monitoring via n8n's built-in execution logs for easier troubleshooting.

# END