

## Podcast Show Notes Generator  
![Latest-0.0.1-blue](https://img.shields.io/badge/Latest-0.0.1-blue)  

An automated **n8n AI agent workflow** designed to generate professional, SEO-optimized podcast show notes directly from uploaded podcast audio files. It orchestrates Google Sheets triggers, Google Drive interactions, AssemblyAI’s transcription services, and Google Docs for output delivery, enhanced with an AI agent to transform raw transcripts into polished, structured notes.

![Podcast Show Notes Generator screenshot](./workflow.png)  


---

### 💡 Why Use Podcast Show Notes Generator?  
- Automates creation of high-quality, timestamped, and formatted podcast show notes.  
- Seamlessly integrates transcription, speaker recognition, chaptering, and SEO keyword generation.  
- Saves manual effort and speeds up content publishing workflows.  
- Supports ongoing podcast production efficiency, perfect for creators and marketers.  
- Provides a polished Markdown output for episode summaries, takeaways, chapters, and SEO optimization.  

---

### ⚡ Who Is This For?  
- Podcast producers and content creators looking to automate show notes generation.  
- Marketing teams aiming to enhance podcast discoverability with SEO-friendly notes.  
- Audio content distributors requiring fast, accurate transcription and summarization.  
- Automation enthusiasts investigating n8n AI agent workflows for multimedia tasks.  
- Developers integrating AssemblyAI and Google Workspace services.  

---

### ❓ What Problem Does It Solve?  
Manually crafting podcast show notes that are detailed, structured, and SEO-optimized can be time-consuming and inconsistent. This workflow eliminates manual transcription and labor-intensive content formatting by:  

- Automatically downloading podcast audio linked in public or internal Google Sheets.  
- Uploading the audio to AssemblyAI for transcription with speaker labeling and chapter segmentation.  
- Checking transcript completion status repeatedly to avoid premature processing.  
- Formatting transcripts into clean, timestamped text segments.  
- Leveraging Google Gemini (PaLM API) AI agent to generate rich show notes from transcript data.  
- Creating and updating Google Docs with finalized notes for immediate publishing or editing.  

---

### 🔧 How This Workflow Works  
1. **Trigger on new podcast submission in Google Sheets**: Periodically polls a Google Sheet storing podcast episode details.  
2. **Edit Fields**: Parses data fields including episode titles, speaker names, audio file links, and notes from the Sheets entry.  
3. **Download file**: Fetches the podcast audio file from Google Drive using the extracted Audio ID.  
4. **Upload Audio to AssemblyAI**: Posts the audio binary to AssemblyAI’s upload endpoint for transcription.  
5. **Start Transcribing**: Sends a request to AssemblyAI’s transcription API with advanced features enabled (speaker labels, automatic highlights, chapters, IAB categories).  
6. **Polling transcription status**: Regularly checks the transcription status until the transcript is marked `"completed"`.  
7. **Format Transcript (Code Node)**: Formats the raw transcription utterances into human-readable, timestamp-annotated text blocks, organizing speaker contributions.  
8. **Podcast Agent (AI Agent Node)**: Uses Google Gemini Chat Model and LangChain to generate structured show notes in Markdown, comprising episode summary, key takeaways, chapters, and SEO keywords based on the transcript and episode metadata.  
9. **Create Google Docs**: Creates a new Google Docs document titled with the episode name to store the generated notes.  
10. **Update Google Docs**: Inserts the AI-generated show notes Markdown text into the created document for review or publishing.  

---

### 🔐 Setup Instructions  
- ✅ Have an **n8n** instance with API access enabled.  
- ✅ Create and link OAuth2 credentials for:  
   - Google Sheets (read access to your podcast response sheet).  
   - Google Drive (file download permissions).  
   - Google Docs (file creation/editing).  
- ✅ Acquire AssemblyAI API key for audio transcription services.  
- ✅ Set up Google Gemini (PaLM API) credentials for AI natural language generation.  
- ✅ Prepare a Google Sheet with podcast episode data including audio file links identified by Google Drive file IDs.  
- ✅ Replace all hardcoded API keys with environment variables or n8n credentials for security.  
- ✅ Configure the Google Sheets Trigger node with your spreadsheet and sheet ID.  

---

### 📅 Payload  

| Key               | Definition                                                |  
|-------------------|-----------------------------------------------------------|  
| Timestamp         | Uploaded podcast episode submission timestamp             |  
| Episode Title     | Title of the podcast episode as submitted in Google Sheet |  
| Audio File        | Google Drive sharable URL for audio file                   |  
| Speaker Names     | Comma-separated list of podcast participants               |  
| Notes             | Style notes or instructions for AI content tone            |  
| Audio ID          | Extracted Google Drive file ID for audio lookup            |  
| upload_url        | Temporary URL where the audio is uploaded for transcription |  
| transcript status | Status of transcription process (queued, processing, completed) |  
| formatted_transcript | Transformed readable transcript with timestamps and speakers|  
| output            | Final AI-generated Markdown show notes                      |  

**Example JSON Payload:**  
```json
{
  "Timestamp": 45959.402595949075,
  "Episode Title": "Joe Rogan - Jordan Peterson's Philosophy on Self Improvement",
  "Audio File": "https://drive.google.com/open?id=13J_AmwGA9s55WMZVfQOg4vfYvR1wZ7fT",
  "Speaker Names": "Joe Rogan, Jordan Peterson",
  "Notes": "Maintain a conversational tone. Add disclaimer about opinions expressed.",
  "Audio ID": "13J_AmwGA9s55WMZVfQOg4vfYvR1wZ7fT"
}
```

**Example cURL Test for AssemblyAI Upload:**  
```bash
curl -X POST "https://api.assemblyai.com/v2/upload" \
-H "authorization: YOUR_ASSEMBLYAI_API_KEY" \
--data-binary "@path/to/audiofile.mp3"
```

---

### 🔨 Tools/Node Used  

| Node Name                 | Purpose                                                      |  
|---------------------------|--------------------------------------------------------------|  
| Google Sheets Trigger     | Triggers workflow on new row submissions in podcast sheet   |  
| Set (Edit Fields)         | Extracts and prepares episode metadata                      |  
| Google Drive (Download file) | Retrieves audio files from the drive                        |  
| HTTP Request (Upload Audio in AssemblyAI) | Uploads audio to AssemblyAI for transcription          |  
| HTTP Request (Start Transcribing) | Initiates transcription with advanced features          |  
| HTTP Request (Get Status) | Polls to check transcription progress                        |  
| If (Is Done?)             | Decision node checking whether transcription is complete    |  
| Code (Code in JavaScript) | Formats transcript into readable timestamped text           |  
| LangChain Agent (Podcast Agent) | Processes transcript to generate finalized show notes     |  
| Google Docs (Create a document) | Creates new Google Doc for episode notes                   |  
| Google Docs (Update a document) | Inserts AI-generated notes into the document              |  
| Google Gemini Chat Model  | AI language model used optionally for natural language tasks|  

---

### ⚙️ Reactive & Proactive Behavior  
- **Reactive**: Starts processing upon new spreadsheet entries (podcast uploads).  
- **Proactive**: Polls AssemblyAI API for transcription completion, avoiding user intervention.  

### 🐞 Error Handling  
- Transcription node includes retry by feeding incomplete transcriptions back until completion.  
- API authorization headers must be valid; invalid keys will fail HTTP requests.  
- Google Drive file ID extraction guarded by regex — malformed URLs may cause failures.  
- Node sequence prevents downstream execution until transcription completes successfully.  

---

### 🧩 Requirements  
- n8n instance with internet access.  
- AssemblyAI account and valid transcription API key.  
- Google Cloud IAM setup with OAuth2 credentials for Sheets, Drive, and Docs.  
- Podcasts uploaded into Google Drive accessible by the workflow.  
- Google Sheet structured like the example with audio file IDs and metadata.  

---

### 📚 Resources  
- [n8n Official Documentation](https://docs.n8n.io/)  
- [AssemblyAI API Docs](https://www.assemblyai.com/docs/)  
- [Google Drive API](https://developers.google.com/drive/api)  
- [Google Sheets API](https://developers.google.com/sheets/api)  
- [Google Docs API](https://developers.google.com/docs/api)  
- [Google PaLM (Gemini) API](https://developers.generativeai.google/products/palm-api)  

---

### 🐞 Troubleshooting  
- Check AssemblyAI API key validity and quotas if uploads or transcriptions fail.  
- Confirm Google Drive file sharing settings and IDs match the expected format.  
- Ensure Google Sheets row data are not missing critical fields (Episode Title, Audio File).  
- Validate OAuth2 credentials scopes and refresh tokens in n8n.  
- Monitor API request limits to avoid rate limiting.  
- Debug JavaScript code node for formatting based on transcript structure changes.  
- Confirm AI agent prompt formatting aligns with updated template if LangChain behavior changes.  
- Adjust polling intervals in Google Sheets Trigger or Get Status nodes based on throughput.  
- Review error logs in n8n for HTTP 400/401/403 response codes from APIs for permission issues.  
- Avoid hardcoding secrets in nodes; use n8n credential store securely.  

---

> ℹ️ This section can be removed once you're fully satisfied with the content  

### 📌 Recommended Improvements  
- Rename "Edit Fields" node to **Prepare Podcast Metadata** for clarity.  
- Rename "Download file" node to **Fetch Podcast Audio from Drive**.  
- Parameterize AssemblyAI API key via environment variables instead of hardcoded string.  
- Add configurable polling interval settings on "Get Status" for API efficiency.  
- Enhance error handling with failure branches to notify via Slack or email on API failures.  
- Replace fixed Google Sheet and Doc IDs with workflow parameters for reusability.  
- Add comments or descriptions in code node explaining timestamp formatting logic for maintainability.  
- Introduce a node to clean and validate Google Sheets input data to reduce malformed entries.  
- Provide fallback transcript processors if AssemblyAI fails or is unavailable.  
- Enable dynamic episode naming conventions in Google Docs based on date or category.  
- Separate AI agent system prompts and user prompts as configurable templates outside the workflow.  
- Implement versioning for generated docs to keep historical notes archived.  
- Support additional AI language models or multi-lingual processing options for diverse podcasts.  
- Integrate author or podcast host info into the show notes metadata for branding.  
- Add a test trigger node to simulate new podcast upload events for workflow testing.  

# END