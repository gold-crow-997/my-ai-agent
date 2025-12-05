
## Company Locator - AI Agent
![Latest-0.0.1-blue](https://img.shields.io/badge/Latest-0.0.1-blue)

This workflow automates the process of locating companies around given addresses by integrating Google Sheets, OpenStreetMap data scraping, and an AI agent to extract, format, and classify business information. It transforms raw geolocation input into enriched, structured business data in a spreadsheet—ready for use in data analysis or CRM.

---

### 💡 Why Use Company Locator - AI Agent?
- Automatically fetch business details near specified locations using OpenStreetMap data.
- Uses AI to parse and clean incoming JSON data into structured, clean fields.
- Enrich raw address input from Google Sheets with longitude/latitude coordinates.
- Append refined company data back into Google Sheets for easy access and tracking.
- Updates input status fields to track progress and handle errors gracefully.

---

### ⚡ Who Is This For?
- Data analysts working with geographic business data.
- Sales and marketing teams needing automated lead enrichment.
- Organizations managing local business directories or location-based services.
- Anyone looking to automate geographic data extraction and processing without coding.

---

### ❓ What Problem Does It Solve?
It eliminates manual work for gathering nearby business data based on address inputs. Instead of manually searching and copying business details, the workflow automates geocoding, scrapes relevant business information from OpenStreetMap, and uses AI to parse complex JSON data into clean fields that integrate seamlessly with Google Sheets.

---

### 🔧 How This Workflow Works
1. **Trigger:** Watches for new rows added in a Google Sheets sheet ("INPUT").
2. **Geocode:** Uses Nominatim API to transform address text into latitude and longitude.
3. **Scrape:** Sends a query to Overpass API, querying for nodes/ways of specified categories around the coordinates, including contact details.
4. **Conditional Check:** Verifies if the Overpass API returns any elements.
   - If no data, update the input row with a "Stopped: No available data" status.
5. **Split Data:** Splits received business elements for processing individually.
6. **Loop & Transform:** For each element, set and format fields, then pass raw JSON fields to an AI agent.
7. **AI Agent:** Processes JSON objects with a system prompt instructing extraction of company name, business type, address, and contacts. It normalizes phone numbers to a mobile-friendly format.
8. **Parse AI Output:** Parses AI-generated string output back into JSON.
9. **Append Data:** Appends the cleaned, structured company data into a separate "OUTPUT" Google Sheets sheet.
10. **Update Status:** Updates original input row to "DONE Scraping" to mark processing complete.

---

### 🔐 Setup Instructions
- ✅ Create Google Sheets with sheets "INPUT" and "OUTPUT" set up.
- ✅ Provide Google Sheets OAuth2 credentials (for triggers and edits).
- ✅ Configure OpenRouter API credentials for the AI agent integration.
- ✅ Confirm Overpass API and Nominatim API endpoints are reachable.
- ✅ Adjust Google Sheet document ID and sheet GIDs in node parameters to match your own sheets.
- ✅ Define input rows with these columns: ADDRESS, RADIUS (METERS), CATEGORY, ID.
- ✅ Set system message in AI Agent node for extracting and formatting business data correctly.
- ✅ Deploy workflow in n8n and activate.

---

### 📅 Payload
| Key            | Definition                                           |
|----------------|-----------------------------------------------------|
| ADDRESS        | Address to geocode and search nearby businesses     |
| RADIUS (METERS)| Search radius for nearby businesses                  |
| CATEGORY       | Business type category tag for Overpass API query   |
| ID             | Unique row identifier from Google Sheets             |
| fields         | JSON object passed to AI agent containing element data|

**Example JSON Payload Sent to AI Agent:**
```json
{
  "tags": {
    "name": "Sample Company",
    "shop": "bakery",
    "office": "headquarters"
  },
  "addr": {
    "city": "Example City",
    "country": "Example Country"
  },
  "contacts": {
    "email": "contact@example.com",
    "phone": "639123456789"
  },
  "lat": 14.5995,
  "lon": 120.9842
}
```

**Example cURL Test for Geocode Node:**
```bash
curl -G "https://nominatim.openstreetmap.org/search" --data-urlencode "q=1600 Amphitheatre Parkway, Mountain View, CA" -d format=json -d addressdetails=1 -d limit=1 -H "User-Agent: curl-geocoding-demo"
```

---

### 🔨 Tools/Node Used
- **Google Sheets Trigger:** Listens for new rows added in the "INPUT" sheet.
- **HTTP Request - Get Longitude and Latitude:** Calls Nominatim OpenStreetMap API to geocode addresses.
- **HTTP Request - Scrape data from Maps:** Queries Overpass API for business nodes and ways based on location, category, radius.
- **If Node - Is not Empty?:** Checks if Overpass API returned data.
- **Split Out & Split In Batches:** Splits the business data array to process individual entries.
- **Set Node - Edit Fields:** Maps and prepares raw element JSON for AI input.
- **AI Agent (Langchain Agent) Node:** Uses OpenRouter Chat Model to parse JSON and extract structured info.
- **Code Node - Parse into JSON:** Converts AI response string output back into JSON object.
- **Google Sheets (Append and Update rows):** Writes processed business data to "OUTPUT" and updates status in "INPUT".

---

### ⚙️ Reactive & Proactive Behavior
- Triggered reactively on new Google Sheets "INPUT" rows.
- Proactively polls every minute to catch frequent adds.
- Uses conditional branching to gracefully handle empty data results.
- Loops and processes each business element sequentially for robust handling.
- Updates workflow status states for transparency.

---

### 🐞 Error Handling
- If Overpass API returns empty dataset, status updates to "Stopped: No available data" in input sheet.
- HTTP request error in geocoding uses "continueErrorOutput" to prevent workflow halt.
- AI parsing errors are trapped and raw output is preserved for manual review.

---

### 🧩 Requirements
- n8n instance with Google Sheets API credentials.
- OpenRouter API account for AI language model.
- Internet access to OpenStreetMap Nominatim and Overpass APIs.
- Pre-configured Google Sheets document with appropriate sheet names and access.
- Correct row formatting in the Sheets as per input data requirements.

---

### 📚 Resources
- [OpenStreetMap Nominatim API Docs](https://nominatim.org/release-docs/latest/api/Search/)
- [Overpass API Documentation](https://overpass-api.de/api/interpreter)
- [Google Sheets API Documentation](https://developers.google.com/sheets/api)
- [OpenRouter API info (Langchain)](https://openrouter.ai/)
- [n8n Documentation](https://docs.n8n.io/)

---

### 🐞 Troubleshooting
- Ensure Google Sheets API credentials have sufficient permission (read/write).
- Verify API endpoints for geocoding and Overpass are not rate-limited or blocked.
- If AI Agent outputs parsing errors, check system prompt for clear instructions and phone formatting.
- Confirm sheet GIDs and document IDs are accurate.
- Validate input rows contain valid ADDRESS and CATEGORY fields.
- Use n8n’s "Execution Data" panel to inspect payloads at each node.
- Adjust polling frequency if workflow triggers too slowly or too often.
- Check network connectivity and n8n server logs for errors.

---

### 📌 Recommended Improvements
- Split complex HTTP Request bodies into reusable templates to allow easier customization.
- Add node descriptions and comments inside n8n for maintainability.
- Implement error notification (email or Slack) if API calls fail repeatedly.
- Allow configuration parameters (sheet IDs, radius, category) to be environment variables.
- Add caching for geocode results to reduce API calls.
- Enhance AI Agent prompt to include fallback for missing phone or email fields.
- Include validation node after AI Agent parsing to ensure required fields exist before appending.
- Add a manual trigger node for testing workflow runs without sheet input.
- Include retries on HTTP requests for robust network failure handling.

# END