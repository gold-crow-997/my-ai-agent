> ⚠️ Kindly review the docs once more before saving.

## Sales Proposal Drafter
![Latest-0.0.1](https://img.shields.io/badge/Latest-0.0.1-blue)

The **Sales Proposal Drafter** workflow automates the creation of customized sales proposals by integrating CRM data, service catalogs, and AI-generated proposal narratives. It validates input data from new sales deals, retrieves client and deal details, fetches service descriptions, and utilizes a Google Gemini AI model to draft key proposal sections. The result is a professional, client-tailored proposal document uploaded back to the CRM system, streamlining the sales process.

![Sales Proposal Drafter screenshot](./workflow.png)

> ⚠️ Note: This image is a sample and does not represent the actual workflow

---

### 💡 Why Use Sales Proposal Drafter?
- Automates sales proposal generation, saving time and effort
- Integrates multiple data sources (CRM client & deal data, service catalog)
- Uses AI (Google Gemini) for tailored, professional narratives
- Outputs a final well-structured markdown proposal and uploads it to CRM
- Sends Slack notifications for progress and errors to keep teams informed
- Ensures input data validation and error handling for robustness

---

### ⚡ Who Is This For?
- Sales teams aiming to accelerate the proposal drafting process
- Proposal managers seeking consistency in proposal sections
- Businesses with complex service catalogs wanting dynamic proposals
- Users of n8n who want to integrate AI into sales workflows
- Organizations using Google Gemini (PaLM) AI for content generation

---

### ❓ What Problem Does It Solve?
Sales teams often spend excessive time manually combining CRM data, service details, and customized narratives to draft proposals. This workflow automates data retrieval and validation, leverages AI to generate persuasive content tailored to client pain points, and assembles a structured document. It reduces manual effort, improves proposal quality, and enhances sales productivity.

---

### 🔧 How This Workflow Works
1. **Webhook Trigger**: Listens for HTTP POST requests at `/proposal_draft` containing `deal_id`, `client_id`, and `service_id`.
2. **Validate Input**: Normalizes and validates incoming data ensuring required `client_id` and service IDs are present and formatted properly.
3. **Fetch CRM Data**: Retrieves client and deal information from CRM endpoints using the validated IDs.
4. **Iterate Services**: Loops over service IDs to fetch detailed service descriptions from the Service Catalog API.
5. **Combine Scope of Work (SOW)**: Aggregates the standard SOW sections for each selected service into a single document block.
6. **Load Proposal Templates**: Loads reusable introduction, terms, and company overview template sections.
7. **Generate AI Proposal Sections**: Calls Google Gemini AI model to produce:
   - Executive Summary
   - Client Challenge
   - Proposed Solution Narrative  
   The AI adheres to strict rules: no pricing/legal terms, uses provided SOW, and tailors content to client profile.
8. **Merge Document**: Combines AI outputs, SOW, pricing, and templates into a full markdown proposal.
9. **Convert and Upload**: Converts markdown to a file, uploads as attachment to CRM deal record.
10. **Slack Notifications**: Sends status updates on proposal generation start and alerts on failures.
11. **Error Handling**: Captures errors during any step and notifies via Slack with detailed error messages.

---

### 🔐 Setup Instructions
- ✅ Create `Webhook` node at `/proposal_draft` to receive POST requests.
- ✅ Configure HTTP Request nodes with CRM API base URLs and OAuth2 credentials.
- ✅ Add and configure `Service Catalog API` endpoint access.
- ✅ Set up Google Gemini (PaLM) AI API credentials and link with the AI model node.
- ✅ Configure Slack OAuth2 credentials and select the Slack channel for notifications.
- ✅ Adjust template text inside the "Load Template" node as needed to reflect your company.
- ✅ Ensure the CRM allows attachment uploads via API for proposals.
- ✅ Verify node error workflows to ensure notifications are sent correctly.

---

### 📅 Payload
| Key | Definition |
| --- | ---------- |
| `deal_id` | Unique identifier for the sales deal |
| `client_id` | Unique identifier for the client |
| `service_id` | Array or single string of service IDs selected for the deal |

**Example JSON Payload:**
```json
{
  "deal_id": "D123456",
  "client_id": "C78910",
  "service_id": ["Srv003", "Srv005"]
}
```

**Example cURL Test:**
```bash
curl -X POST https://your-n8n-instance.com/webhook/proposal_draft \
 -H 'Content-Type: application/json' \
 -d '{"deal_id":"D123456","client_id":"C78910","service_id":["Srv003","Srv005"]}'
```

---

### 🔨 Tools/Node Used
- **Webhook**: Entry point for triggering workflow via HTTP POST.
- **Code Node ("Validate Input")**: Ensures input data correctness and normalizes service IDs to array.
- **HTTP Request Nodes**: Fetch client data, deal data, and service catalog info from external APIs.
- **SplitInBatches ("Loop Over Items")**: Processes multiple services one by one.
- **Slack Node**: Sends progress and error messages to a specified Slack channel.
- **Google Gemini Node**: Uses Google’s PaLM 2.5 AI model to generate tailored proposal sections.
- **Code Nodes**: Assemble and format final markdown proposal document.
- **Convert to File**: Converts final markdown text into downloadable/uploadable file format.
- **HTTP Request Upload**: Uploads the proposal file to CRM linked with the deal.
- **Error Trigger**: Catches workflow errors and activates Slack error notification.

---

### ⚙️ Reactive & Proactive Behavior
- **Reactive**: Workflow triggers when a new proposal draft request is received via webhook.
- **Proactive**: Sends Slack notifications as workflow progresses or if it encounters errors.
- **Looping**: Services processed sequentially within split batches to enable granular control and error handling.
- **Error Continuation**: LLM node configured to continue even if minor errors occur, ensuring workflow resilience.

### 🐞 Error Handling
- Input validation errors throw and stop processing early with descriptive error messages.
- A dedicated error trigger node captures uncaught exceptions.
- Slack notifications post error details including failed node name, error messages, and related deal ID.
- Workflow continues only when critical inputs and API responses are valid.

---

### 🧩 Requirements
- Working n8n environment (v2.x or higher recommended) with access to the nodes used.
- Valid CRM API credentials for accessing Client, Deal, and Attachment upload endpoints.
- Google Gemini (PaLM) API access and configured credentials.
- Slack OAuth2 credentials configured with permission to post messages to a designated channel.
- Internet access for n8n to reach external APIs (CRM, Service Catalog, Google Gemini, Slack).
- Proper authorization from CRM to accept file uploads tied to deals.

---

### 📚 Resources
- [n8n Official Documentation](https://docs.n8n.io/)
- [Google PaLM API (Gemini) Documentation](https://developers.generativeai.google/)
- [Slack API & OAuth2 Setup](https://api.slack.com/authentication/oauth-v2)
- [CRM API Documentation - (Replace with your CRM’s docs)]
- [Service Catalog API Documentation - (Replace with your specific service catalog API)]

---

### 🐞 Troubleshooting
- **Missing Input Keys**: Validate input JSON contains `client_id` and `service_id`.
- **Service ID Format Issues**: Service IDs must be strings or arrays; form-data objects converted appropriately.
- **API Call Failures**: Check credentials and endpoint URLs for CRM and Service Catalog.
- **AI Generation Errors**: Verify Google Gemini API quota and credential validity.
- **Slack Notifications Missing**: Ensure OAuth2 token covers the selected channel.
- **File Upload Failure**: Confirm CRM permissions to attach files and correct deal ID usage.
- **Workflow Loops Unexpectedly**: Verify batch splitting and looping logic is consistent.
- **Webhook Not Triggering**: Confirm URL path is correct and reachable.
- **Markdown Formatting Issues**: Adjust template or code node concatenation if layout breaks.
- **Error Messages Not Clear**: Inspect error trigger node and Slack message template for debug info.

---

> ℹ️ This section can be removed once you're fully satisfied with the content

### 📌 Recommended Improvements
- Consolidate loop and Slack notification logic to avoid duplicate Slack posts.
- Move hardcoded template texts from "Load Template" node to external files or environment variables for easier edits.
- Add more detailed logging for API response errors to aid debugging.
- Implement retries with exponential backoff on HTTP Request nodes to improve resiliency.
- Provide option to select different AI models dynamically based on proposal type.
- Add input schema checks in the webhook node to reject malformed requests immediately.
- Enhance Slack messages with links to relevant deals or proposal drafts in CRM.
- Include unit test examples or dry run mode for validating workflow changes.
- Modularize code nodes into reusable sub-workflows or external scripts to improve maintainability.
- Expand error handling with custom alerts (e.g., email, SMS) beyond Slack.
- Incorporate version control tags directly into the output proposal documents.
- Allow dynamic pricing block fetching and generation instead of static text fallback.
- Provide example input JSON schema and Swagger/OpenAPI doc for external integrators.

# END