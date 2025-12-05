## HR FAQ Telegram
![Latest-0.0.1](https://img.shields.io/badge/Latest-0.0.1-blue)

HR FAQ Telegram is an n8n AI agent workflow designed to automate HR-related FAQ handling via Telegram chat. Leveraging AI-driven natural language understanding, it interprets user questions sent as Telegram messages and responds using a carefully curated company FAQ knowledgebase and chatbot personality tailored for NMS Philippines.

![HR FAQ Telegram screenshot](https://bin.nmscreative.com/amqmnm.png)

---

### 💡 Why Use HR FAQ Telegram?
- Automates frequent HR queries through Telegram, reducing manual response effort.
- Provides contextual answers based on a rich company knowledgebase and current HR FAQ data.
- Supports dynamic tone switching (professional, informal, humorous, etc.) adjustable per chat session.
- Enables seamless integration of AI language models and external FAQ tools for accurate recall.
- Keeps a friendly chatbot persona ("Vee") consistent with company branding.
- Handles off-topic queries gracefully by redirecting conversation back to HR topics.
- Provides real-time Telegram notifications and conversational interface.
- Caches and manages FAQ updates efficiently via Google Sheets integration.
- Encapsulates complex logic into a maintainable n8n workflow.
- Supports multi-language tone translation and polite fallback messaging.

---

### ⚡ Who Is This For?
- HR departments seeking automated FAQ support on Telegram.
- n8n automation developers integrating AI with chatbots.
- Support teams reducing repetitive query handling overhead.
- Companies wanting consistent, branded internal HR communication.
- AI enthusiasts leveraging Langchain agents in real-world workflows.
- Telegram bot developers looking for AI conversational integrations.
- Organizations aiming for multi-office timezone aware chatbot assistants.
- Data teams maintaining FAQ knowledgebases synchronized with Google Sheets.
- Companies using proactive tone switching to tailor user experience.
- Enterprises benefiting from AI-augmented employee support.

---

### ❓ What Problem Does It Solve?
Answering repetitive HR questions is time-consuming, prone to inconsistent messaging, and burdens HR staff. This workflow provides an AI-powered assistant that interprets employee queries from Telegram and replies using an up-to-date FAQ knowledgebase with tone adaptability. This reduces response time, improves user experience, and ensures consistent HR information delivery across global offices.

---

### 🔧 How This Workflow Works
1. **Telegram Trigger**: Listens for incoming Telegram messages containing employee questions.
2. **Tone Save Node (Code Node)**: Detects if the user wants to switch conversational tone (e.g., formal, humorous) based on keywords and maintains tone state per chat.
3. **NMS HR Assistant Agent (Langchain Agent Node)**: Receives user message and current tone; generates AI responses using system context including company data and chatbot rules. Queries FAQ tool if needed.
4. **FAQ_Query_Tool (HTTP Request Tool)**: Fetches relevant HR FAQs from a dedicated vector store or knowledgebase for AI reference.
5. **OpenAI Chat Model**: Utilizes GPT-4.1-mini to interpret and generate human-like replies incorporating company style and tone.
6. **Send Telegram Response**: Sends the AI-generated reply back to the user via Telegram bot API.
7. **HR FAQ Sheet (Google Sheets Trigger)**: Monitors FAQ spreadsheet updates to refresh or add entries in the knowledgebase.
8. **Code in JavaScript, Aggregate, HTTP Request Nodes**: Process and upload FAQ data into the knowledgebase vector store to keep responses topical and fresh.
9. Chatbot maintains polite fallback messages if no FAQ matches and gently steer off-topic messages back to company HR context.

---

### 🔐 Setup Instructions
- Import the provided JSON workflow into your n8n instance.
- Configure the following credentials:
  - Telegram Bot API credentials to enable Telegram Trigger and send responses.
  - OpenAI API key for the Langchain agent's chat model nodes.
  - HTTP Header Auth credential for accessing RAG API endpoints (FAQ Query and archival).
  - Google Sheets OAuth2 for HR FAQ spreadsheet trigger and access.
- Update the Google Sheet ID and Worksheet name linked to your HR FAQ data source.
- Ensure your n8n webhook URLs are accessible for Telegram and other external calls.
- Confirm timezone is set to Manila (GMT+8) for company context.
- Run initial synchronization of FAQ data to populate vector knowledgebase.
- Test Telegram bot by sending sample HR questions and confirming tone switching works.
- Maintain the FAQ sheet to keep company HR data current.

---

### 🔨 Tools/Node Used
- **Telegram Trigger**: Initiates workflow on incoming Telegram messages.
- **Tone Save (Code Node)**: Parses and saves conversational tone states by chat.
- **NMS HR Assistant Agent (Langchain Agent)**: Core AI conversational node implementing chatbot personality with Langchain.
- **FAQ_Query_Tool (HTTP Request Tool)**: Interfaces with vector database RAG API for FAQ retrieval.
- **OpenAI Chat Model (GPT-4.1-mini)**: Provides advanced conversational AI completions.
- **Send Telegram Response (Telegram Node)**: Sends generated textual answers back to user.
- **HR FAQ Sheet (Google Sheets Trigger)**: Updates FAQ knowledge base from spreadsheet changes.
- **Code / Aggregate / HTTP Request Nodes**: Transform and synchronize FAQ data to vector store.

---

### ⚙️ Reactive & Proactive Behavior
- **Reactive**: Responds instantly when an employee sends an HR query via Telegram.
- **Proactive**: Updates the knowledgebase when FAQ Spreadsheet changes detected, keeping data fresh.
- **Tone switching**: Dynamically adapts chatbot tone per user preference in conversation.
- **Fallback**: If no FAQ found, politely redirects conversation to company information.

---

### 🧩 Requirements
- n8n instance version supporting Langchain agent nodes.
- Telegram bot creation and valid API credentials.
- OpenAI API key with GPT access.
- Valid RAG API token for FAQ retrieval services.
- Google Sheets API OAuth2 credentials.
- Google Sheet containing current HR FAQs, with question and answer columns.
- A publicly accessible webhook endpoint for Telegram.
- Manila timezone setting for relevant date/time context.

---

### 📚 Resources
- [n8n Official Documentation](https://docs.n8n.io/)
- [Telegram Bot API Documentation](https://core.telegram.org/bots/api)
- [OpenAI API](https://platform.openai.com/docs/)
- [Langchain Documentation](https://python.langchain.com/en/latest/)
- [Google Sheets API](https://developers.google.com/sheets/api/)
- RAG (Retrieval-Augmented Generation) API for FAQ querying

---

### 🐞 Troubleshooting
- Ensure Telegram webhook URL is properly set and reachable by Telegram servers.
- Verify credentials for Telegram, OpenAI, and RAG API are valid and unexpired.
- Confirm Google Sheets API access and correct spreadsheet ID and sheet name.
- If no responses or empty replies, check the Langchain agent prompt and FAQ integrations.
- Tone switching may fail if unrecognized tone keywords are input—check regex in Tone Save node.
- Ensure RAG API endpoints for FAQ querying and storing are accessible and receiving correct data format.
- Validate timezones and date formats align with Manila timezone settings.
- Monitor n8n execution logs for node-specific errors or timeouts.
- The workflow requires stable internet access to all APIs.
- When updating FAQ sheet, allow a few minutes for vector store sync before testing new entries.