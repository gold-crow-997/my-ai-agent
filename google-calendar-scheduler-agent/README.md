## Google Calendar Scheduler
![Agent Version](https://img.shields.io/badge/Latest-0.0.1-blue)

Agentix Google Calendar Scheduler is an automated workflow designed to enable effortless scheduling, updating, retrieval, and deletion of calendar events within Google Calendar through natural language chat messages or structured webhook inputs. Utilizing advanced AI agents, it converts user chat or webhook requests into validated calendar commands and manages events with Google Calendar API integrations. It also sends timely notifications via email.

![Agentix Google Calendar Scheduler screenshot](https://bin.nmscreative.com/bnabp7.png)

---

### 💡 Why Use Google Calendar Scheduler?
- Automates calendar event creation and management from natural language or webhook inputs
- Leverages AI (OpenAI GPT-4.1-mini) to parse unstructured inputs into structured data
- Validates scheduling conflicts automatically before creating or updating events
- Supports event creation, retrieval, updates (via deletion + new create), and deletion
- Sends personalized event notifications via Gmail to organizers and attendees
- Allows inclusion of Google Meet links for virtual meeting integration
- Simplifies scheduling through chat or programmatic API calls (webhooks)
- Maintains attendee and host info with timezone awareness and conflict rules
- Provides detailed, JSON-compliant outputs for downstream processing or UI display
- Integrates tightly with Google OAuth2 credentials for seamless API access

---

### ⚡ Who Is This For?
- Professionals and teams seeking hands-free calendar scheduling
- Developers aiming to build AI-powered calendar workflows in n8n
- Organizations wanting automated event notification systems
- Automation enthusiasts exploring n8n AI agent integrations
- Anyone needing easy event scheduling from chat or webhooks without manual data entry

---

### ❓ What Problem Does It Solve?
Manual calendar entry can be slow, error-prone, and inefficient. This workflow:
- Automatically extracts event details from conversational inputs or APIs
- Enforces scheduling rules and prevents double bookings or conflicts
- Generates confirmations including alternate available slots on conflicts
- Sends formatted calendar invites and reminders via Gmail
- Offers consistent, validated event JSON outputs usable for other systems
- Saves time and reduces cognitive load linked with managing multiple calendars

---

### 🔧 How This Workflow Works
1. **Reception of Input:**
   - Via a public chatbot webhook triggered by new chat messages or incoming webhook requests.
2. **Input Parsing & Normalization:**
   - AI Agents extract sessionId, event title, host, attendees, start and end times, timezone, and action type.
   - Event data is normalized to standardized date/time formats and validated.
3. **Conflict & Event Checks:**
   - Google Calendar node retrieves existing events within specified time ranges to ensure no overlaps.
   - Enforces rules such as no overlapping events per participant.
4. **Event Operation Execution:**
   - Depending on the action (`create`, `delete`, etc.), events are inserted, updated, or removed accordingly.
   - For updates, previous event deletion occurs before creating the new event.
5. **Output Preparation:**
   - Constructed outputs include event IDs, titles, hosts, attendees, Google Meet links, and suggested next available times.
6. **Notification Dispatch:**
   - Sends personalized email notifications containing event summary and meeting links to participants.
7. **Response & Confirmation:**
   - Webhook responds with formatted JSON object indicating status, scheduling success, and messages.

---

### 🔐 Setup Instructions
- **Step 1: Import the Workflow**
  - Import the provided JSON workflow into your n8n environment.
- **Step 2: Connect Google Calendar Credentials**
  - Add Google Calendar OAuth2 credential.
  - Configure `Get Events`, `Create Event`, and `Delete Event` nodes with this credential.
- **Step 3: Connect Gmail Node**
  - Add Gmail OAuth2 credential for sending email notifications.
- **Step 4: Configure Chat Trigger**
  - Enable chat webhook node to receive external user requests.
- **Step 5: Set Webhook Node**
  - Configure webhook node with your preferred path to receive API calls.
- **Step 6: Set AI Credentials**
  - Provide OpenAI or OpenRouter API credentials for natural language processing agents.
- **Step 7: Test Event Scheduling**
  - Send sample chat message or webhook JSON per the specified JSON payload format.
  - Verify events get created without conflicts and emails are sent.
- **Step 8: (Optional) Edit Email Templates**
  - Customize the email formatting agent or Google Docs template for personalized notification appearance.

---

### 📅 Payload
| Key         | Definition                                               |
| ----------- | --------------------------------------------------------|
| sessionId   | Unique identifier for the event session                  |
| eventTitle  | Descriptive title of the event                            |
| host        | Email and optionally name of the organizer/host          |
| attendees   | Array of attendee emails                                  |
| startDate   | Start datetime in "YYYY-MM-DD HH:MM AM/PM" format       |
| endDate     | End datetime in "YYYY-MM-DD HH:MM AM/PM" format         |
| timezone    | IANA timezone string e.g. "Asia/Manila"                  |
| action      | Event action: `create`, `delete`, or `update`            |
| withGoogleMeet | Boolean, include Google Meet link if true               |
| templateId  | Optional Google Docs template ID for email formatting     |

**Example JSON Payload:**
```json
{
  "sessionId": "session-aug27-001",
  "eventTitle": "Interview for Jon Rey",
  "host": { "email": "jonreygalera@gmail.com" },
  "attendees": ["jrg@newmediastaff.com"],
  "startDate": "2025-08-27 04:00 PM",
  "endDate": "2025-08-27 05:00 PM",
  "timezone": "Asia/Manila",
  "action": "create",
  "withGoogleMeet": true,
  "templateId": null
}
```

**Example cURL Test:**
```bash
curl --request POST \
  --url https://YOUR_WORKSPACE.n8n.cloud/webhook/76b4545f-dac4-4e20-a365-8585ab1e67e2 \
  --header 'Content-Type: application/json' \
  --data '{
    "sessionId": "session-aug27-001",
    "eventTitle": "Interview for Jon Rey",
    "host": {"email": "jonreygalera@gmail.com"},
    "attendees": ["jrg@newmediastaff.com"],
    "startDate": "2025-08-27 04:00 PM",
    "endDate": "2025-08-27 05:00 PM",
    "timezone": "Asia/Manila",
    "action": "create",
    "withGoogleMeet": true
  }'
```

---

### 🔨 Tools/Node Used
- **@n8n/n8n-nodes-langchain.agent**: Core AI agents to parse, validate, and format event data
- **Google Calendar Tool Nodes** (`Get Events`, `Create Event`, `Delete Event`): Manage actual Google Calendar events
- **Webhook Node**: Accept external API or chat input requests
- **Gmail Node**: Send email notifications about events
- **Set Nodes**: Prepare and map required parameters between nodes
- **If Node**: Conditional logic to control workflow branches
- **OpenAI / OpenRouter Chat Models**: Language models for natural language understanding and generation
- **Email Transformation Agent**: Formats notification emails with styling and structure
- **HTML Node**: Outputs formatted email HTML
- **Sticky Note Node**: Documentation embed inside workflow for sample user prompts

---

### ⚙️ Reactive & Proactive Behavior
- **Reactive:** Listens for incoming chat messages or webhook calls to trigger scheduling automation
- **Proactive:** Can be extended with scheduled triggers (e.g., reminder emails 1 hour before event)

---

### 🧩 Requirements
- n8n instance with API access and webhook internet exposure
- Google Calendar API access with OAuth2 credentials
- Gmail API OAuth2 credentials for email notifications
- OpenAI or OpenRouter API key for AI natural language processing
- Properly configured webhooks with valid URLs accessible externally

---

### 📚 Resources
- n8n Official Documentation  
- Google Calendar API Documentation  
- Gmail API Documentation  
- OpenAI / OpenRouter API docs  
- Telegram Bot API (optional for messaging integration)

---

### 🐞 Troubleshooting
- Ensure Google credentials have required scopes for calendar event modification
- Verify webhook URL is publicly accessible and properly configured in n8n
- Check OpenAI/OpenRouter API keys are valid and not rate-limited
- Confirm timezone strings are valid IANA identifiers (e.g., "Asia/Manila")
- Debug failures by inspecting node execution logs and AI agent outputs
- Confirm email node OAuth2 tokens are refreshed and active
- Validate JSON payloads sent to webhook are complete and correctly formatted
- If conflicts arise repeatedly, check the calendar events for overlapping times
- Make sure chat or webhook triggers are enabled and active in workflow settings
