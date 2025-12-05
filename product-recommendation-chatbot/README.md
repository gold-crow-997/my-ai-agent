> ⚠️ Kindly review the docs once more before saving.

## Product Recommendation Chatbot
![Latest-0.0.1](https://img.shields.io/badge/Latest-0.0.1-blue)

An intelligent n8n AI agent workflow designed to interact with Telegram users, understand their product needs using Google Gemini large language models, reason about missing or needed information, search a product catalog via Google Sheets, and provide personalized product recommendations directly in chat.

![Product Recommendation Chatbot screenshot](./workflow.png)

> ⚠️ Note: This image is a sample and does not represent the actual workflow

---

### 💡 Why Use Product Recommendation Chatbot?
- Automates personalized product recommendations on Telegram
- Provides intelligent conversational understanding using Google Gemini LLM
- Uses logical reasoning to check if more user info is required
- Queries real product data from a Google Sheets catalog dynamically
- Sends friendly, formatted replies with product details and links
- Secure access enforced via API key on webhook endpoint
- Easy to extend and maintain within n8n environment

---

### ⚡ Who Is This For?
- E-commerce operators wanting conversational shopping bots
- Developers building AI-powered chat agents in n8n
- Businesses needing Telegram bots for product help & sales
- Automation enthusiasts looking to combine AI and external data
- Teams requiring structured error notifications for workflow issues

---

### ❓ What Problem Does It Solve?
Users often struggle to find the right product by filtering hundreds of options manually. This chatbot automates the process by understanding user preferences (category, price, color, brand, features) through conversation, reasoning about missing info, searching an up-to-date spreadsheet catalog, and returning tailored product recommendations immediately. It enhances user experience and streamlines sales assistance inside popular chat apps.

---

### 🔧 How This Workflow Works
1. **Telegram Trigger**: Listens for Telegram messages to start conversation.
2. **Conversation Understanding**: Uses Google Gemini LLM with custom prompt and structured output parsing to extract user intent and filters.
3. **Think Tool ("Think")**: Internal reasoning engine assesses if enough data exists or what is missing.
4. **Memory Node ("Simple Memory")**: Stores chat context keyed by Telegram chat ID, enabling conversation continuity.
5. **Intent Check (IF Node)**: Routes flow based on intent — if "recommend", proceed; else ask for more info.
6. **Ask More Info Node**: Sends Telegram messages asking users for missing information.
7. **Product Finder Agent**:
   - Utilizes Gemini LLM and a reasoning tool to interpret filters.
   - Queries Google Sheets product catalog, filtering products by category, price, color, brand, and features.
   - Ranks and selects the best matches with persuasive justifications.
8. **Format Recommendation Message**: Formats product list into user-friendly Telegram messages with names, prices, reasons, and product links.
9. **Send Recommendation**: Delivers final product suggestions back to user’s Telegram chat.
10. **Error Handling**: Monitors workflow errors, triggers error notifications via Gmail to admins.
11. **Webhook + Validation**: Provides standardized API entry with API key validation, but does not start main flow (Telegram is primary trigger).

---

### 🔐 Setup Instructions
- ✅ **Telegram Bot Credentials**: Create a Telegram bot and configure its Token & Username inside n8n credentials.
- ✅ **Google Gemini (PaLM) API Key**: Obtain API key and configure in n8n for Gemini language model nodes.
- ✅ **Google Sheets OAuth2 Credentials**: Setup access to the Google Sheets catalog containing product data.
- ✅ **Gmail OAuth2 Credentials**: Setup OAuth for n8n to send error alert emails.
- ✅ **Environment Variables**:  
  - `PRODUCT_RECOMMENDATION_API_KEY=product_recommendation_test_key` (used for webhook validation)
- ✅ **Google Sheets Product Catalog**: Maintain a Google Sheet with appropriate columns (Category, Price, Color, Brand, Features, Link).
- ✅ **Webhook URL**: Use webhook URL for testing or external API integration:  
  `https://n8n.popapps.ai/webhook-test/product-recommentation-chatbot`

---

### 📅 Payload

| Key       | Definition                                              |
|-----------|---------------------------------------------------------|
| intent    | User intent: "ask_more" to gather info or "recommend"  |
| missing   | Array of missing attributes (e.g., color, price)        |
| filters   | Object containing category, price_max, color, features, brand for product search |
| message   | Text message for Telegram, either ask or recommendations |
| products  | (Output) List of products with detailed info and reasons|

**Example JSON Payload for Conversation Understanding Output:**
```json
{
  "intent": "recommend",
  "missing": [],
  "filters": {
    "category": "headphones",
    "price_max": "150",
    "color": "black",
    "features": ["noise cancelling", "wireless"],
    "brand": "Sony"
  },
  "message": "I found some great wireless noise cancelling Sony headphones under $150."
}
```

**Example cURL Test Webhook Call:**
```bash
curl -X POST https://n8n.popapps.ai/webhook-test/product-recommentation-chatbot \
 -H "Content-Type: application/json" \
 -d '{"apiKey": "product_recommendation_test_key"}'
```

---

### 🔨 Tools/Node Used
- **Telegram Trigger**: Receives user messages to start conversations.
- **Google Gemini Chat Model**: Google’s PaLM language model for natural language understanding.
- **Structured Output Parser**: Parses AI JSON responses for structured data extraction.
- **Think Tool**: Dedicated logical reasoning node to analyze input and decide next actions.
- **Simple Memory**: Memory buffer to retain chat history keyed by Telegram chat ID.
- **If Node (Intent Check)**: Branches logic based on AI intent output.
- **Product Finder Agent**:
  - Uses Gemini LLM + reasoning tool + Google Sheets tool for product search.
- **Search Product (Google Sheets Tool)**: Queries Google Sheets catalog by user filters.
- **Format Recommendation Message (Code Node)**: Formats product data into Telegram message text.
- **Send Recommendation (Telegram Node)**: Sends final messages to user.
- **Error Trigger + Send Error Alert**: Captures errors and emails admin.
- **Webhook + Validation + Respond to Webhook**: API entry point with API key validation and response.

---

### ⚙️ Reactive & Proactive Behavior
- **Reactive**: Responds to Telegram user messages and input dynamically.
- **Proactive**: If filtering info is incomplete, asks follow-up questions to fill missing fields before product search.
- **Error Handling**: Immediately alerts administrators on workflow errors for prompt corrective action.

---

### 🐞 Error Handling
- Workflow includes an Error Trigger node connected to a Gmail node that emails system admins with detailed error and execution info.
- Validation node at webhook checks for correct API key; if invalid, workflow halts with unauthorized error.
- Common errors like invalid input formatting or API failures are captured and notified.

---

### 🧩 Requirements
- n8n instance with access to external APIs
- Telegram Bot with active token & username
- Google Cloud account with Gemini (PaLM) API access enabled
- Google credentials for accessing shared Google Sheets product catalog
- Gmail OAuth2 credentials configured in n8n
- Defined environment variable `PRODUCT_RECOMMENDATION_API_KEY`
- Google Sheet formatted for product data (columns: category, price, color, features, brand, link, description)

---

### 📚 Resources
- [Telegram Bots API](https://core.telegram.org/bots/api)
- [Google Gemini / PaLM API Documentation](https://developers.generativeai.google/)
- [n8n Documentation - Telegram Trigger](https://docs.n8n.io/integrations/n8n-nodes-base.telegramTrigger/)
- [Google Sheets API & OAuth Setup](https://developers.google.com/sheets/api/guides/authorizing)
- [n8n Official](https://n8n.io)
- [JavaScript Function Nodes in n8n](https://docs.n8n.io/nodes/nodes-library/n8n-nodes-base.code/)

---

### 🐞 Troubleshooting
- **Telegram messages not triggering flow:** Verify bot token and webhook setup in Telegram, ensure Telegram Trigger node is active.
- **No product results returned:** Confirm Google Sheet contains product data matching filters, check Product Finder reasoning logic.
- **Errors on Google Gemini nodes:** Verify Gemini API credentials, quota, and network connectivity.
- **API Key validation fails:** Ensure `PRODUCT_RECOMMENDATION_API_KEY` environment variable matches key sent in webhook calls.
- **Email alerts not received:** Check Gmail OAuth credentials and ensure Send Error Alert node is correctly configured.
- **Memory not persisting user context:** Confirm sessionKey formula uses Telegram chat ID correctly.
- **Workflow stalls at Intent Check:** Inspect if AI output JSON is malformed or parser nodes are misconfigured.
- **Malformed user prompts or unexpected AI outputs:** Tune LangChain AI prompts for clearer instruction and output format adherence.
- **Webhook response delayed:** Ensure minimum execution time in nodes is optimized and n8n server resources are adequate.
- **Multiple product matches exceeding message length:** Adjust Format Recommendation node to limit number of products displayed.

---

> ℹ️ This section can be removed once you're fully satisfied with the content

### 📌 Recommended Improvements
- Rename **Telegram Trigger** node to **User Message Listener** for clarity.
- Rename **Conversation Understanding** node to **Intent & Filter Extractor**.
- Rename **Think** node to **Logical Reasoner** for clearer purpose.
- Rename **Simple Memory** node to **User Memory Buffer**.
- Rename **Intent Check** to **Decision Router**.
- Rename **Ask More Info** to **Missing Data Prompt**.
- Rename **Product Finder** to **Product Search Agent**.
- Rename **Product Finder Language Model** to **Product Search LLM**.
- Rename **Product Search Reasoning** to **Product Search Planner**.
- Rename **Format Recommendation Message** to **Recommendation Formatter**.
- Add input validation on Telegram messages for empty or malformed text.
- Include rate limiting for Telegram requests to avoid API throttling.
- Allow configuration of max products displayed in recommendation message.
- Implement fallback message for Gemini API downtime or quota exceeded.
- Log more detailed error info in Gmail alerts (e.g., input payload context).
- Consider adding sentiment analysis to better tailor responses.
- Support multi-language product recommendations by expanding GPT prompts.
- Add unit tests or manual test cases to validate each node’s output.
- Provide a dashboard or monitoring to track usage and errors visually.
- Expand webhook validation to support multiple API keys or client identifiers.

# END