> ⚠️ Kindly review the docs once more before saving.

## Product Description Write  
![Latest-0.0.1](https://img.shields.io/badge/Latest-0.0.1-blue)

This n8n AI agent workflow automates the generation of SEO-friendly product descriptions and meta descriptions for e-commerce products stored in Airtable. Using Google Gemini’s advanced language model, it processes new product entries and updates Airtable with rich, optimized text along with SEO performance scoring. The workflow also implements a webhook for standard compliance and access validation.

![Product Description Write screenshot](./workflow.png)

> ⚠️ Note: This image is a sample and does not represent the actual workflow

---

### 💡 Why Use Product Description Write?  
- Automatically create engaging, SEO-optimized product descriptions and concise meta descriptions for e-commerce products.  
- Seamlessly integrates Airtable and Google Gemini for AI-enhanced content generation.  
- Evaluates SEO quality through keyword coverage and length scoring for continuous content improvement.  
- Updates Airtable records with generated content and SEO scores to centralize product data.  
- Includes webhook and validation nodes to ensure secure and standardized API access.

---

### ⚡ Who Is This For?  
- E-commerce store owners wanting automated content generation for product listings.  
- Digital marketers aiming to scale SEO-friendly product descriptions.  
- Content teams looking to streamline product data enrichment.  
- n8n users exploring AI and automation integrations.  
- Developers implementing AI-based workflow automation with Google Gemini and Airtable.

---

### ❓ What Problem Does It Solve?  
Manual writing of SEO-optimized product descriptions is time-consuming and inconsistent. This workflow automates the process, producing high-quality, keyword-rich text tailored to brand tone and sales platforms, ensuring content adheres to SEO best practices and improving product discoverability.

---

### 🔧 How This Workflow Works  
1. **Trigger on New Airtable Product Entry:** The workflow starts when a new product record is created or updated in Airtable's "Products" table (via the **New Product** Airtable Trigger node).  
2. **Batch Processing:** The entries trigger batch processing over new items using the **Loop Over Items** node for efficient execution.  
3. **Data Preparation:** The **Product Data** node extracts relevant fields (product name, specs, features, target keywords, brand tone, platform) from Airtable data.  
4. **Generate AI Descriptions:** The **Generate Description + Meta** node calls the Google Gemini Chat Model to generate a full product description (150–200 words, SEO-friendly) and a short meta description (<160 characters).  
5. **SEO Scoring:** The **SEO Score Check** node runs JavaScript logic to score the description based on keyword inclusion and word count, providing an SEO score out of 100.  
6. **Write Back to Airtable:** The **Write Results** node updates the corresponding Airtable record with the AI-generated description, meta description, and SEO score.  
7. **Loop Continuation and Throttling:** The workflow loops and waits briefly via **Wait** before processing the next item to avoid rate limits.  
8. **Webhook and Validation:** A separate **Webhook** node provides an API entry point requiring an API key validated via a **validation** code node; however, this webhook does not trigger the content creation but complies with workflow standard requirements.  
9. **Respond to Webhook:** On valid webhook calls, the workflow sends back developer notes and usage instructions via the **Respond to Webhook** node.

---

### 🔐 Setup Instructions  
- ✅ **Airtable Setup:**  
  - Create a base (e.g., "Product Catalog") with a table (e.g., "Products").  
  - Fields required: Product Name, Specs, Features, Target Keywords (comma-separated), Brand Tone (option field), Platform (option field), AI Description (text), Meta Description (text), SEO Score (number), Trigger Time (date/time).  
  - Generate an Airtable Personal Access Token with read/write access.  

- ✅ **Google Gemini Model Credentials:**  
  - Set up Google Gemini (PaLM) API access and create credentials in n8n with an API key.  

- ✅ **n8n Environment:**  
  - Import this workflow JSON into n8n.  
  - Add credentials for Airtable and Google Gemini under n8n credentials.  
  - Create an environment variable for webhook API validation key: `AIRTABLE_PRODUCT_WRITER_KEY` (example: `airtable_product_writer_test_key`).  

- ✅ **Trigger Configuration:**  
  - Ensure the Airtable Trigger node monitors the "Trigger Time" field or new entries to start processing.  

- ✅ **Optional Webhook:**  
  - Use the Webhook URL provided for standardized API calls if integration with external services is needed (note this webhook does not initiate generation but validates requests).  

---

### 📅 Payload  
| Key              | Definition                                                             |  
|------------------|------------------------------------------------------------------------|  
| Product Name     | Name of the product for description generation.                        |  
| Specs            | Product specifications, features.                                      |  
| Features         | Highlighted features of the product.                                   |  
| Target Keywords  | Comma-separated list of SEO keywords to include in the description.   |  
| Brand Tone       | Tone of voice (Friendly, Modern, Professional) for text style.        |  
| Platform         | E-commerce platform (Shopify, WooCommerce, Amazon) for tailoring text.|  
| AI Description   | Auto-generated SEO-friendly product description (150–200 words).      |  
| Meta Description | Short meta snippet under 160 characters for SEO purposes.              |  
| SEO Score        | Numerical SEO score evaluating keyword coverage and length.           |  

**Example JSON Payload:**
```json
{
  "name": "Wireless Bluetooth Headphones",
  "specs": "Bluetooth 5.2, 20-hour battery life, noise cancellation",
  "features": "Lightweight design, fast pairing, built-in microphone",
  "keywords": "wireless headphones, bluetooth headphones, noise cancellation",
  "tone": "Friendly",
  "platform": "Shopify"
}
```

**Example cURL Test (Webhook Validation):**  
```bash
curl -X POST "https://n8n.popapps.ai/webhook-test/product-description-write?apikey=airtable_product_writer_test_key" -H "Content-Type: application/json"
```

---

### 🔨 Tools/Nodes Used  
- **Airtable Trigger ("New Product")** – Monitors Airtable for new or updated product records to start the workflow.  
- **Split In Batches ("Loop Over Items")** – Enables efficient batch processing of multiple records sequentially.  
- **Set ("Product Data")** – Extracts and assigns product data fields for AI input.  
- **Langchain Google Gemini Chat Model** – Calls Google Gemini PaLM API for AI text generation.  
- **Langchain Structured Output Parser** – Parses the structured JSON output from AI response.  
- **Langchain Agent ("Generate Description + Meta")** – Defines the AI prompt and triggers generation with retry capability.  
- **Code Node ("SEO Score Check")** – Custom JavaScript calculates SEO score based on keyword coverage and description length.  
- **Airtable Node ("Write Results")** – Updates the product record with AI descriptions and SEO score.  
- **Wait Node** – Adds delay between batches to manage API rate limits.  
- **Webhook Node** – Provides API entry point for external validation calls (non-trigger workflow).  
- **Code Node ("validation")** – Validates webhook API key to secure access.  
- **Respond to Webhook** – Sends usage notes back to API clients upon webhook calls.  
- **Sticky Note** – Contains developer notes and usage instructions inside the workflow.

---

### ⚙️ Reactive & Proactive Behavior  
- **Reactive:** Trigger activates instantly on new Airtable product entries.  
- **Proactive:** Automatically reprocesses batches of product records with controlled waiting to manage rate limits and data updates.  

---

### 🐞 Error Handling  
- **Retry on Fail:** The AI generation node includes retry logic to handle transient API failures.  
- **Validation Node:** Blocks unauthorized webhook access with clear error messaging ("Unauthorized — invalid API key").  
- **Batch Looping:** Wait node prevents API limit breaches by spacing requests.  

---

### 🧩 Requirements  
- Airtable personal access token with write permissions.  
- Airtable base and tables pre-created with required fields.  
- Google Gemini PaLM API key configured in n8n credentials.  
- n8n instance (self-hosted or cloud) with internet access to Google and Airtable APIs.  
- Environment variable setup for webhook API key validation (if webhook used).  

---

### 📚 Resources  
- [n8n Documentation](https://docs.n8n.io/)  
- [Google Gemini (PaLM) API](https://developers.generativeai.google/)  
- [Airtable API](https://airtable.com/api)  
- [LangChain Integration for n8n](https://github.com/n8n-io/n8n)  

---

### 🐞 Troubleshooting  
- Ensure Airtable API keys and base/table IDs are correctly configured in credentials.  
- Confirm Google Gemini API key is valid and has quota available.  
- Check field mapping in Airtable node matches your actual table schema.  
- Review API rate limits; adjust wait timing if hitting limits.  
- Verify environment variable for webhook key matches calls if using webhook.  
- Look at workflow execution logs within n8n to track errors or failed nodes.  
- If AI output is empty or malformed, tweak prompt or check Gemini API status.  
- Confirm retry is enabled for AI generation in case of temporary service issues.  
- Validate that keywords are comma-separated without extra whitespace anomalies.  
- Ensure proper casing and exact field names in Airtable trigger setup.  

---

> ℹ️ This section can be removed once you're fully satisfied with the content

### 📌 Recommended Improvements  
- Rename node **"Google Gemini Chat Model"** to **"AI Model: Google Gemini (PaLM)"** for clarity.  
- Change **"Generate Description + Meta"** to **"AI Content Generator"** to reflect function better.  
- Rename **"SEO Score Check"** to **"SEO Evaluation Script"** to clearly state purpose.  
- Rename **"Write Results"** to **"Update Airtable Records"** for explicit action description.  
- Rename **"New Product"** Airtable Trigger to **"Airtable: New Product Entry Trigger"**.  
- Rename **"Loop Over Items"** to **"Batch Processing Loop"** for better understanding.  
- Add descriptive comments inside Code node "SEO Score Check" for maintainability.  
- Add logging node after Write Results to confirm successful Airtable update (optional).  
- Include explicit error handling and notification node (e.g., email or Slack) for API failures.  
- Add dynamic configuration input for keyword weight and scoring thresholds in SEO evaluation node.  
- Modularize the AI prompt text in an external file or workflow variable for easier edits.  
- Clearly document expected Airtable field data types and validation rules inside notes or separate documentation.  
- Enhance webhook validation to support multiple API keys or user roles if extended.  
- Add manual trigger node option for ad-hoc processing or debugging.  
- Document environment variable setup in an external config file or secrets manager hook.  

# END