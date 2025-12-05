# Landing Page Generator v2

Landing Page Generator v2 is an AI-powered workflow that automatically creates professional, mobile-responsive landing pages based on user input from Google Forms. It leverages market research, content generation, and advanced AI models to produce complete HTML landing pages with embedded CSS, optional logo generation, and comprehensive A/B testing recommendations.

## 💡 Why Use Landing Page Generator v2?

- **Fully Automated**: Creates complete landing pages from form submission to email delivery
- **Market Research Integration**: Uses SerpAPI to analyze competitors and target audience insights
- **AI-Powered Content**: Generates headlines, copy, CTAs, and A/B test variations using Google Gemini
- **Professional Design**: Produces responsive HTML5 pages with embedded CSS, accessibility features, and SEO optimization
- **Logo Generation**: Creates custom logos using AI image generation when needed
- **Complete Package Delivery**: Emails HTML file, content documentation, and A/B test reports automatically

## ⚡ Who Is This For?

- **Marketing Agencies** needing rapid landing page prototypes for clients
- **Small Business Owners** wanting professional landing pages without design skills
- **Freelancers** offering landing page services with automated workflows
- **Startups** requiring quick MVP landing pages for product launches
- **Developers** building automated marketing solutions for clients

## ❓ What Problem Does It Solve?

Creating professional landing pages typically requires design skills, market research, copywriting expertise, and technical implementation. This workflow automates the entire process by:

- Conducting market research to understand target audience and competitors
- Generating compelling headlines, copy, and call-to-action variations
- Creating professional HTML/CSS code with responsive design
- Generating custom logos when needed
- Providing A/B testing recommendations for optimization
- Delivering complete packages via email for immediate use

## 🔧 How This Workflow Works

1. **Google Sheets Trigger** monitors a Google Form for new landing page requests
2. **Content Generation Agent** uses SerpAPI to research the market and generate compelling copy including headlines, subheadings, body content, and CTAs
3. **Conditional Logo Generation** creates custom logos using Google Gemini image generation if no existing logo is provided
4. **Search Landing Page** researches competitor landing pages for design inspiration and UX patterns
5. **Initial Code Generation** creates the base HTML landing page with embedded CSS using Google Gemini
6. **Refinement Process** optimizes the HTML for performance, accessibility, SEO, and mobile responsiveness
7. **Image Processing** converts generated logos to proper format and embeds them in the HTML
8. **File Conversion** packages the HTML, content documentation, and A/B test reports into downloadable files
9. **Gmail Delivery** automatically emails all files to the requester

## 🔐 Setup Instructions

1. **Import Workflow**: Import the provided workflow JSON into your n8n instance
2. **Configure Credentials**:
   - Google Sheets API (for form monitoring)
   - Google Gemini API Key (for AI content and image generation)
   - Gmail OAuth2 (for email delivery)
   - SerpAPI Key (for market research)
3. **Set Up Google Form**: Create a Google Form with the required fields (see payload section)
4. **Connect Google Sheets**: Link the form responses to the Google Sheets trigger
5. **Configure Webhook**: Set up proper webhook URLs if needed
6. **Test Workflow**: Submit a test form entry to verify all connections work
7. **Customize Prompts**: Adjust AI prompts in the agent nodes if needed for your specific use case

## 📅 Payload

### Required Google Form Fields:

| Field | Definition |
|-------|------------|
| Page Title | The main topic/service for the landing page |
| Goal/Purpose | Primary objective (e.g., "Convert visitors to trial users") |
| Target Audience | Description of ideal customers |
| Product/Service Name | Business/product name |
| Testimonials or Social Proof | Customer testimonials and reviews |
| Video Embed Link | Optional video URL for embedding |
| Font Styles | Preferred typography (e.g., "Inter") |
| First Name / Last Name | Contact information |
| Phone Number / Email | Contact details for delivery |
| Logo Description | Description for AI logo generation (if no existing logo) |
| Logo | Existing logo URL (optional) |

### Example Form Submission:

```json
{
  "Page Title": "Boost Your Workflow with FlowTrack",
  "Goal/Purpose": "Convert visitors into free trial users of the productivity SaaS",
  "Target Audience": "Remote teams, freelancers, and small businesses",
  "Product/Service Name": "FlowTrack",
  "Testimonials or Social Proof": "FlowTrack cut our project delivery time by 30% in just two months. - Sarah K., Agency Owner",
  "Font Styles": "Inter",
  "First Name": "John",
  "Last Name": "Doe", 
  "Email": "john@example.com",
  "Logo Description": "Logo for tech company that specializes in ai automation"
}
```

## 🔨 Tools/Nodes Used

- **Google Sheets Trigger**: Monitors form responses for new landing page requests
- **SerpAPI Search**: Conducts market research and competitor analysis
- **Google Gemini Chat Models**: Multiple AI agents for content generation, HTML creation, and refinement
- **Google Gemini Image Generation**: Creates custom logos when needed
- **LangChain Agents**: Orchestrates complex AI workflows for content and code generation
- **Simple Memory**: Maintains context across the workflow execution
- **Code Nodes**: Handle data processing, image conversion, and file preparation
- **Conditional Logic (If Nodes)**: Manages optional logo generation and existing logo handling
- **Gmail Integration**: Delivers final packages via email with attachments

## ⚙️ Reactive & Proactive Behavior

- **Reactive**: Automatically triggers when new Google Form submissions are detected
- **Proactive**: Conducts market research and generates comprehensive content packages without manual intervention

## 🧩 Requirements

- **n8n Instance**: Functional n8n installation with internet access
- **Google Workspace**: Access to Google Forms, Sheets, and Gmail
- **API Keys**: 
  - Google Gemini API key for AI generation
  - SerpAPI key for search functionality
  - Configured OAuth2 for Google services
- **Storage**: Adequate space for temporary file processing and email attachments
- **Permissions**: Proper API permissions for all integrated services

## 📊 Output Deliverables

The workflow automatically emails three files:

1. **Landing Page HTML**: Complete, responsive landing page ready for hosting
2. **Content Documentation**: Generated headlines, copy, and key selling points
3. **A/B Test Report**: Alternative headlines, CTAs, and optimization recommendations

## 🎯 Key Features

- **Mobile-Responsive Design**: All pages are optimized for mobile devices
- **SEO Optimized**: Includes proper meta tags, structured data, and semantic HTML
- **Accessibility Compliant**: WCAG AA standards with proper contrast and keyboard navigation
- **Performance Optimized**: Embedded CSS, optimized images, and minimal JavaScript
- **Professional Styling**: Modern design with smooth animations and micro-interactions
- **A/B Testing Ready**: Includes variations and testing recommendations for optimization

## 📚 Resources

- [n8n Documentation](https://docs.n8n.io/)
- [SerpAPI Documentation](https://serpapi.com/search-api)
- [Google Sheets API](https://developers.google.com/workspace/sheets/api/guides/concepts)

## 🐞 Troubleshooting
- If workflow doesn't trigger: Verify Google Sheets Trigger is properly configured and polling every minute. Check that the Google Form is correctly linked to the spreadsheet and new responses are being recorded.
- If content generation fails: Validate SerpAPI key is active and has sufficient credits. Check that the search query from "Page Title" field returns results. Ensure Google Gemini API key has proper permissions and quota.
- If email delivery fails: Confirm Gmail OAuth2 credentials are properly authenticated and not expired. Check that recipient email address is valid. Verify Gmail API has send permissions enabled.
- If file attachments are corrupted: Review JavaScript code in "Converts generated HTML & Contents to file" node for proper Base64 encoding. Check that binary data is being processed correctly. Verify MIME types are set appropriately.
- If HTML is malformed: Validate that refinement prompts are generating proper HTML structure.