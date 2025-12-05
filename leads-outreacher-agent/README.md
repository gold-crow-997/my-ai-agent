
## Leads Outreacher  
![Leads Outreacher](https://img.shields.io/badge/Latest-0.0.1-blue)  

An automated outreach workflow designed to manage and run email campaigns by integrating Google Sheets with the Woodpecker campaign management API. It periodically detects new leads added to a spreadsheet, filters those contacts based on campaign status, adds them to an email campaign, updates their status, and manages campaign execution — all seamlessly automated within n8n.

---

### 💡 Why Use Leads Outreacher?  
- Automatically triggers outreach actions when new contacts are added to a Google Sheet  
- Ensures contacts are only emailed once by filtering out those already contacted  
- Adds new prospects directly to a Woodpecker email campaign programmatically  
- Updates campaign status back to the Google Sheet for record-keeping  
- Automatically manages campaign run state (start/stop) via API calls  
- Batch processes leads for optimized handling  

---

### ⚡ Who Is This For?  
- Sales and marketing teams looking to automate lead outreach  
- Customer success managers managing follow-ups on new contacts  
- Anyone using Woodpecker for cold email campaigns and Google Sheets for contact management  
- n8n users seeking to integrate spreadsheet triggers with HTTP-based campaign APIs  
- Automation enthusiasts wanting to combine spreadsheet data triggers with campaign workflows  

---

### ❓ What Problem Does It Solve?  
It eliminates manual efforts needed to continuously monitor, update, and run email outreach campaigns by automatically syncing contacts from a spreadsheet and managing their status in an active campaign using Woodpecker’s API — guaranteeing no duplicate outreach and timely follow-up.

---

### 🔧 How This Workflow Works  
1. **Google Sheets Trigger1**: Watches a specified Google Sheet ("Leads Outreacher" spreadsheet, "Contacts" sheet) every minute and triggers when a new row is added.  
2. **Stop campaign**: Immediately stops the campaign via Woodpecker API to ensure a clean state before processing new leads.  
3. **Filter**: Checks if the contact’s "Campaign Status" is *not* "Sent An Email" to avoid re-processing already emailed leads.  
4. **Loop Over Items1**: Processes filtered contacts in batches (to efficiently handle multiple leads).  
5. Inside the batch:  
   - **Add Contacts to Campaign**: Adds the prospect’s email, first name, company, and a custom snippet to the Woodpecker campaign through a POST request.  
   - **Update Status : Sent an Email**: Updates the respective row in Google Sheets, marking the Campaign Status as "Sent An Email" to prevent future duplicate emails.  
6. **Run Campaign**: Sends a POST request to Woodpecker to start running the campaign after adding prospects.  
7. This looping and updating repeat ensures continuous processing and campaign operation.  

---

### 🔐 Setup Instructions  
- ✅ Create a Google Sheet named "Leads Outreacher" and add a sheet/tab named "Contacts" with columns such as:  
  - Contact Name  
  - Company Name  
  - Email Address  
  - Campaign Status  
- ✅ Obtain Google Sheets OAuth2 credentials and connect them to n8n for the "Google Sheets Trigger1" and "Update Status" nodes.  
- ✅ Generate or acquire your Woodpecker API Key and configure HTTP Request nodes with this key as the `x-api-key` header.  
- ✅ Know your Woodpecker campaign ID (here: `2363707`) for API calls to start, stop, and add prospects.  
- ✅ Import this workflow JSON into n8n, configure credential references, and activate your workflow.  
- ✅ Ensure network access allows n8n to call Google Sheets and Woodpecker API endpoints.  

---

### 📅 Payload  
| Key              | Definition                                               |  
| ---------------- | --------------------------------------------------------|  
| Contact Name     | Prospect’s full name extracted from the Google Sheet     |  
| Company Name     | The prospect’s company name                               |  
| Email Address    | The lead’s email used to add them to Woodpecker campaign |  
| Campaign Status  | Workflow-controlled status column tracking email sent    |  

**Example JSON Payload Used to Add a Prospect:**  
```json
{
  "campaign": {
    "campaign_id": 2363707
  },
  "prospects": [
    {
      "email": "{{ $json['Email Address'] }}",
      "first_name": "{{ $json['Contact Name'] }}",
      "company": "{{ $json['Company Name'] }}",
      "snippet1": "Hello World!"
    }
  ]
}
```

**Example cURL Test for Adding Prospect:**  
```bash
curl -X POST https://api.woodpecker.co/rest/v1/add_prospects_campaign \
-H "x-api-key: YOUR_API_KEY" \
-H "Content-Type: application/json" \
-d '{
  "campaign": {
    "campaign_id": 2363707
  },
  "prospects": [{
    "email": "example@example.com",
    "first_name": "John",
    "company": "Example Corp",
    "snippet1": "Hello World!"
  }]
}'
```

---

### 🔨 Tools/Nodes Used  
- **Google Sheets Trigger1 (Trigger Node)**: Monitors "Contacts" sheet for new rows added in real-time to trigger the workflow.  
- **Stop campaign (HTTP Request Node)**: Calls Woodpecker’s API to stop the email campaign before updating contacts.  
- **Filter (Filter Node)**: Checks if contact's "Campaign Status" is not "Sent An Email" to filter out already emailed prospects.  
- **Loop Over Items1 (SplitInBatches Node)**: Processes contacts in batches for scalability.  
- **Add Contacts to Campaign (HTTP Request Node)**: Adds each new prospect programmatically to Woodpecker campaigns via API.  
- **Update Status : Sent an Email (Google Sheets Node)**: Updates the Google Sheet to mark contacts as emailed, preventing duplicates.  
- **Run Campaign (HTTP Request Node)**: Triggers Woodpecker to run the campaign after prospect addition.  

---

### ⚙️ Reactive & Proactive Behavior  
- **Reactive:** The workflow triggers reactively on new row addition in Google Sheets.  
- **Proactive:** Automatically orchestrates campaign management (start/stop, add prospects, update status) programmatically with no manual intervention.  

### 🐞 Error Handling  
- If the Woodpecker API call fails, the workflow may stall or skip that contact—ideal to add retry or error handling in n8n for robust operation.  
- Google Sheets node update errors could leave status outdated; ensure credentials and sheet permissions are correct.  

---

### 🧩 Requirements  
- n8n instance with internet access  
- Google Sheets OAuth2 credentials with edit and read access to target spreadsheet  
- Valid Woodpecker API key and rights for managing the specified campaign  
- Properly named Google Sheet and tabs matching the workflow configuration  
- Campaign ID from Woodpecker to tie HTTP requests appropriately  

---

### 📚 Resources  
- [n8n Documentation - Google Sheets Trigger](https://docs.n8n.io/integrations/builtin/app-nodes/n8n-nodes-base.googlesheetstrigger/)  
- [Woodpecker API Documentation](https://woodpecker.co/docs/api)  
- [Google Sheets API and OAuth Setup](https://developers.google.com/sheets/api/guides/authorizing)  
- [n8n Documentation - HTTP Request Node](https://docs.n8n.io/integrations/builtin/app-nodes/n8n-nodes-base.httprequest/)  

---

### 🐞 Troubleshooting  
- **Trigger not firing?** Ensure Google Sheets OAuth credentials are connected and the spreadsheet ID and sheet name are correct.  
- **Campaign Status not updating?** Verify that Google Sheets node has edit permissions and matching columns defined properly.  
- **API calls failing (401 or 403)?** Confirm Woodpecker API key is valid, correctly scoped, and included in header.  
- **Workflow runs but does not add contacts?** Double-check filtering logic; "Campaign Status" values must be as expected.  
- **Batch processing issues?** Confirm batch sizes and n8n settings fit the expected lead volume.  
- **Node connection errors?** Ensure each node is properly connected and configured within the n8n canvas.  

---

> ℹ️ This section can be removed once you're fully satisfied with the content

### 📌 Recommended Improvements  
- Rename **Google Sheets Trigger1** → "New Lead Trigger" for clarity.  
- Rename **Stop campaign** → "Pause Woodpecker Campaign" to clarify action intent.  
- Rename **Add Contacts to Campaign** → "Add Prospect to Campaign API" for descriptive clarity.  
- Rename **Update Status : Sent an Email** → "Mark Contact as Emailed" for straightforward understanding.  
- Add explicit error handling and retry logic on HTTP Request nodes to handle temporary API downtime.  
- Include webhook trigger alternative for real-time updates instead of polling every minute, improving efficiency.  
- Add parameterization of campaign ID and API key as workflow variables to avoid hardcoding.  
- Use environment variables or n8n credentials management for secure storage of API keys instead of inline values.  
- Extend filtering to accept a whitelist/blacklist of contacts or domain names for better targeting.  
- Incorporate logging node to track successes and failures for audit and debugging.  
- Add additional field mappings dynamically to improve the flexibility of prospect data sent to Woodpecker.  
- Provide a dashboard node or periodic summary email about campaign statuses and newly added contacts.  
- Refactor batch processing to delay between batches to avoid API rate limiting issues.  
- Validate emails format before adding prospects to reduce errors from invalid data entries.  

# END