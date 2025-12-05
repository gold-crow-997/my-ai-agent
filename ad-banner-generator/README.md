> ⚠️ Kindly review the docs once more before saving.

## Banner Ad Generator
![Latest-0.0.1](https://img.shields.io/badge/Latest-0.0.1-blue)

The **Banner Ad Generator** is an automated workflow that generates banner ad creatives based on product and promotional data entered in a Google Sheet. It utilizes Google's Gemini AI models for ad copy and image generation, blends images and text via image manipulation nodes, and finally sends the finished banner ads via email. This workflow is designed for marketing teams seeking to streamline ad creative production with AI-assisted automation.

![Banner Ad Generator screenshot](./workflow.png)

> ⚠️ Note: This image is a sample and does not represent the actual workflow


---

### 💡 Why Use Banner Ad Generator?
- Automatically create engaging and brand-aligned banner ads without manual design effort.
- Leverage Google Gemini AI for generating targeted ad copy and creative images.
- Integrate with Google Sheets to dynamically generate banners based on live product data.
- Compose and merge multiple image layers (backgrounds, logos, product images, text).
- Send final ad creative directly via email to reviewers or clients.
- Standardize ad generation with webhook support for enterprise-level API compliance.
- Incorporates error alerting and monitoring via Gmail notifications.

---

### ⚡ Who Is This For?
- Digital marketing teams looking to automate banner ad production.
- Agencies managing multiple product campaigns with frequent ad updates.
- eCommerce businesses requiring consistent ad creatives from product data.
- Developers and automation engineers integrating AI-powered design workflows.
- n8n users seeking real-world AI + image compositing examples.

---

### ❓ What Problem Does It Solve?
Manual banner ad design can be time-consuming, inconsistent, and costly—especially when managing many products or frequent updates. This workflow automatically generates visually coherent banner advertisements by reading product info from Google Sheets, creatively generating copy and images via Google Gemini, compositing multiple design elements programmatically, and distributing the ads via email—drastically reducing manual workload and turnaround time.

---

### 🔧 How This Workflow Works

1. **Trigger on New Product Data (Google Sheets Trigger)**  
   Watches for new rows added to a specified Google Sheet representing a product or promotion.

2. **Pre-processing Node**  
   Extracts and restructures raw sheet data into a consistent JSON object containing promotion info, assets (images, colors, fonts), banner format and size.

3. **Banner Contents Generator (Google Gemini Chat Model + LangChain Agent)**  
   Sends promotion JSON data to Gemini AI to generate banner ad copy text (headline, body, CTA) following custom size-based copy rules.

4. **Position Setting Node**  
   Calculates pixel-based layout coordinates for each visual element in the banner based on target banner size.

5. **Prepare Banner Data Node**  
   Combines layout settings with the generated ad copy variations for downstream processing.

6. **Wait Nodes**  
   Pauses workflow to ensure AI-generated images and other async processes complete before continuing.

7. **Background & Headline Image Generators (Google Gemini Image Generation Models)**  
   Generate base banner background and headline text images respectively as PNGs according to AI prompts and brand colors.

8. **Resize Nodes**  
   Resize the generated background and headline images to exact canvas sizes as needed.

9. **Merge Nodes and Composite Nodes**  
   Sequentially layer and composite images: background + headline, then composite with resized logo and product images downloaded from Google Drive.

10. **Download & Resize Logo and Product Images**  
    Fetch external assets from Google Drive, resize them for banner composition.

11. **Final Composition and Extraction Nodes**  
    Composite all image layers into the final banner ad image, extract the binary data for sending.

12. **Send Email Node**  
    Email the composed banner ad as an attachment to the email address specified in the Google Sheet row.

13. **Error Handling Nodes**  
    On error, triggers email notifications to alert workflow owner with detailed error and execution info.

14. **Webhook & Validation Nodes**  
    Receive POST calls for interoperability, validate API key, and return developer notes. The webhook is not the workflow entry point; the real trigger is Google Sheets.

---

### 🔐 Setup Instructions

- ✅ **Google Sheets Credentials**  
  Connect with OAuth2, allowing the workflow to read banner data from your sheet.  
- ✅ **Google Gemini API Credentials**  
  Provide API key for Google Gemini/PaLM for both language and image generation nodes.  
- ✅ **Google Drive Credentials**  
  Allow downloading product and logo images stored on Google Drive.  
- ✅ **Gmail Credentials**  
  For sending emails with finished banner attachments on error or success notifications.  
- ✅ **Environment Variable for API Key**  
  Add `BANNER_GENERATOR_API_KEY` in your environment with a shared secret key (e.g., `banner_generator_test_key`).  
- ✅ **Google Sheet Setup**  
  Format your Google Sheet to include columns for promotion title, description, CTA, landing URL, product logo and image URLs, color palette, font, template, format, banner size, and email for delivery.  
- ✅ **Webhook Setup (Optional)**  
  For standardizing external API calls to initiate or verify workflows complying with NMS-AI standards.

---

### 📅 Payload

| Key             | Definition                                                  |
|-----------------|-------------------------------------------------------------|
| promotion       | Object containing `title`, `description`, `cta`, `landing_url` of the promotion.          |
| assets          | Asset URLs and styling info: `product_image`, `logo`, `colors` (array), `font`.            |
| template        | Banner template name                                         |
| format          | Ad format type                                              |
| target_size     | Banner image size in pixels (e.g., `728x90`)                 |
| copy_variations | AI generated ad copy: headline, body, CTA arrays             |
| layout          | Coordinates x,y for background, logo, headline, and product. |

**Example JSON Payload:**
```json
{
  "promotion": {
    "title": "Sleek Modern Sofa Sale",
    "description": "Upgrade your living room with our ergonomic sofas",
    "cta": "Shop Now",
    "landing_url": "https://example.com/sofas"
  },
  "assets": {
    "product_image": "https://drive.google.com/file/d/abc123",
    "logo": "https://drive.google.com/file/d/xyz789",
    "colors": ["#FF5733", "#333333", "#FFFFFF"],
    "font": "Open Sans"
  },
  "template": "Modern Minimal",
  "format": "Leaderboard",
  "target_size": "728x90"
}
```

**Example cURL Test:**
```bash
curl -X POST https://n8n.popapps.ai/webhook-test/banner-ad-generator \
-H "Content-Type: application/json" \
-d '{"apiKey": "banner_generator_test_key"}'
```

---

### 🔨 Tools/Node Used

- **Google Sheets Trigger**: Watches for new rows in the Ads spreadsheet.  
- **Code Node (Pre-processing)**: Restructures raw Google Sheet data into normalized JSON objects.  
- **LangChain Agent & Google Gemini Chat Model**: AI language model generating banner text content constrained by banner dimensions.  
- **Google Gemini Image Generation Nodes**: Create background and headline images using AI image models.  
- **Edit Image Nodes**: Resize and composite images into final creative.  
- **Google Drive Node**: Downloads logos and product images for final banner.  
- **Gmail Node**: Sends the final banner ad images to specified email addresses.  
- **Webhook & Validation Nodes**: Accept API requests and validate with API keys for standardized access.  
- **Error Trigger & Gmail Node**: Error detection and notification system.

---

### ⚙️ Reactive & Proactive Behavior

- **Reactive**: Triggered automatically when a new product row is added in Google Sheets.  
- **Proactive**: Validates incoming API webhook event keys to ensure only authorized access to docs and workflow information.  
- **Retry Mechanism**: AI image generation nodes are marked with `retryOnFail` to ensure resilience against transient failures.  

---

### 🐞 Error Handling

- Any runtime error triggers the `Error Trigger` node.  
- Sends descriptive error mail including stack trace and execution context to the administrator email `ranilo@popai.agency`.  
- This aids rapid debugging and reliability in production.

---

### 🧩 Requirements

- n8n cloud or self-hosted instance with capability to add integrations and credentials.  
- Proper OAuth2 credentials for Google Sheets, Google Drive, Gmail.  
- Google Gemini API access for AI-driven text and image creation.  
- Permission to write and trigger APIs/webhooks and send emails from the Gmail account.  
- Access to the Google Sheet with the defined columns and data format.  
- Environment variable for secure API key validation handling.

---

### 📚 Resources

- [n8n Documentation](https://docs.n8n.io)  
- [Google Gemini / PaLM API](https://developers.generativeai.google)  
- [Google Sheets API](https://developers.google.com/sheets/api)  
- [Google Drive API](https://developers.google.com/drive)  
- [Gmail API](https://developers.google.com/gmail/api)  
- [LangChain Documentation](https://docs.langchain.com)

---

### 🐞 Troubleshooting

- **Empty or Malformed Sheet Data**: Ensure all required columns exist and contain valid URLs and data.  
- **API Authentication Errors**: Confirm all OAuth2 credentials and API keys are correctly set in n8n.  
- **Image Generation Failures**: Check Google Gemini quota and retry configurations.  
- **Email Delivery Fails**: Verify Gmail credential correctness and appropriate app permissions for sending.  
- **Workflow Does Not Trigger on New Rows**: Confirm trigger node is active and connected to correct sheet and tab.  
- **Webhook Unauthorized Errors**: Confirm API key matches environment variable exactly.  
- **Composite Image Misalignment**: Review layout calculations inside Position Setting node code for x,y accuracy.  
- **Slow Workflow Execution**: Investigate wait nodes or API response delays; increase timeouts if needed.

---

> ℹ️ This section can be removed once you're fully satisfied with the content

### 📌 Recommended Improvements

1. Rename **"Pre-processing"** to **"Data Normalizer"** for clarity on its role.  
2. Rename **"Banner Contents Generator"** to **"AI Copy Generator"** to emphasize text generation.  
3. Rename **"Position Setting"** to **"Layout Calculator"** to better describe function.  
4. Add explicit validation node after "Pre-processing" to check for required fields and format.  
5. Include retry strategies or fallback prompts for Gemini AI nodes in case of repeated failures.  
6. Add a final approval step allowing human review before email dispatch.  
7. Use environment variable for recipient email(s) instead of hard-coding inside Gmail nodes.  
8. Create a separate error dashboard node to accumulate errors for easy monitoring.  
9. Modularize image composition nodes into reusable subworkflows for maintainability.  
10. Include detailed logging and execution timing nodes to monitor performance bottlenecks.  
11. Implement version control tags or comments inside Google Sheet for traceability of data inputs.  
12. Provide multi-language support for banner copy by expanding Gemini prompts.  
13. Add functionality to upload final banner image back to Google Drive or cloud storage.  
14. Expand webhook to trigger workflow start, not only standard response, for direct API-driven use.  
15. Include a node that optimizes output images for web delivery (compression, format).  

# END