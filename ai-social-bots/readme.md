# AI-Social-Bots

An n8n workflow collection that automates **multi-platform social media content creation and posting** using AI-generated captions, hashtags, and images with real-time Telegram approval and scheduled automation.

---

## Features

- **Telegram Bot Integration:** Dedicated bots for each platform (X, Facebook, Instagram, LinkedIn, Reddit) to receive content requests and approvals.  
- **AI-Powered Content Generation:** Creates optimized captions, hashtags, and image prompts via Google Gemini.  
- **Image Creation & Hosting:** Generates and stores platform-ready images using Cloudinary.  
- **Dual Trigger System:**  
  - On-demand posting via Telegram  
  - Scheduled posting via Google Sheets or Google Calendar  
- **Approval Workflow:** Inline Telegram buttons (Approve / Reject / Regenerate) for content moderation.  
- **Platform-Specific Optimization:** Captions, hashtags, and images formatted for each platform’s best practices.  
- **Permalink Sharing:** Sends post URLs back to Telegram after publishing.  

---

## Workflow Components

### Trigger Systems
- **Telegram Bot Trigger:** Real-time user requests for social content.  
- **Schedule Triggers:** Automated posting via Google Sheets or Google Calendar events.  

### Content Generation Pipeline
- **Message Processing:** Extracts topics and context from Telegram.  
- **AI Content Generation:** Produces captions, hashtags, and image prompts optimized per platform.  
- **Trend Research (Optional):** Pulls trending topics from SerpAPI.  
- **Image Generation:** Creates aspect-ratio–specific images.  
- **Approval Flow:** Sends previews to Telegram for decision-making.  
- **Posting:** Publishes to platform APIs upon approval.  
- **Permalink Return:** Returns live post URL to Telegram.  

### Supported Platforms
1. **X (Twitter):** Media + caption posting via OAuth API.  
2. **Facebook:** Direct photo and caption posting via Graph API.  
3. **Instagram:** Container → publish workflow with 4:5 aspect ratio images.  
4. **LinkedIn:** Caption + image posting via LinkedIn API.  
5. **Reddit:** Link submission with hosted images via Reddit API.  

---

## Setup Requirements

- **n8n Installation:** Running n8n instance with LangChain + social API nodes.  
- **API Credentials:**  
  - X API keys (OAuth1 + OAuth2)  
  - Facebook/Instagram Graph API tokens  
  - LinkedIn developer access  
  - Reddit OAuth2 credentials  
  - Google Gemini API key  
  - Telegram bot tokens (per platform)  
  - Google Sheets & Calendar API credentials  
  - Cloudinary API with social-post preset  

---

## Usage

### Manual Posting
1. Send a topic to the Telegram bot for the target platform.  
2. Workflow generates caption, hashtags, and image.  
3. Telegram preview sent with inline Approve/Reject/Regenerate buttons.  
4. On approval, workflow posts directly to the platform.  
5. Telegram bot replies with permalink to the published post.  

### Automated Posting
1. Topics stored in Google Sheets or Google Calendar events.  
2. Workflow triggers daily or at scheduled time.  
3. AI generates captions, hashtags, and new images.  
4. Posts automatically (optional approval flow).  
5. Returns permalink to Telegram.  

---

## Configuration Notes

- **AI Prompts:** Customize tone, length, and hashtag rules per platform.  
- **Image Ratios:**  
  - Instagram → 4:5 (1080x1350)  
  - X → 16:9  
  - Facebook → 1:1 or flexible  
- **Posting Schedule:** Adjust triggers for engagement windows.  
- **Rate Limiting:** Add delays to avoid hitting platform API limits.  
- **User Preferences:** Daily posting opt-in stored in Google Sheets.  

---

## Dependencies

- n8n with LangChain and platform API nodes  
- Google Gemini (content + image generation)  
- Cloudinary for image hosting  
- Telegram API for approvals  
- Google Sheets / Google Calendar  
- SerpAPI for trend-based topics  

---

## Advanced Features

- **Cross-Platform Automation:** Unified workflow logic adapted to each platform.  
- **Structured Output Parsing:** Ensures consistent JSON output from AI.  
- **Error Handling:** Automatic retries for failed posts.  
- **User-Specific Preferences:** Stores and applies posting rules per user.  
- **Scalable Framework:** Extendable to additional platforms or AI services.  