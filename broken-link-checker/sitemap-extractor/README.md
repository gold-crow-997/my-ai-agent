## Sitemap XML AI Crawler
![AGENT_VERSION](https://img.shields.io/badge/Latest-0.0.1-blue)

A robust AI-powered `n8n` workflow designed to recursively crawl, parse, normalize, and filter XML sitemaps. This workflow intelligently handles sitemap indexes and URL sets, extracts detailed metadata such as images, videos, and news data, and applies robots.txt-based filtering to ensure only allowed URLs are processed and returned.

![Sitemap XML AI Crawler screenshot](./screenshot/workflow.png)

---

### 💡 Why Use Sitemap XML AI Crawler?
- Automatically processes complex sitemap structures, including nested sitemap indexes.
- Extracts enriched URL metadata (lastmod, priority, changefreq, images, videos, news).
- Prevents infinite loops with configurable maximum recursion depth.
- Applies robots.txt rules to respect site crawling policies.
- Outputs clean, aggregated URL lists for downstream automation or analysis.
- Handles HTTP errors gracefully without stopping the workflow.
- Supports flexible input with user-configurable starting sitemap URL and crawl depth.
- Modular design enables easy integration into larger workflows.
- Includes detailed filtering to exclude disallowed URLs efficiently.
- Proactively logs important events and statuses for traceability.

---

### ⚡ Who Is This For?
- SEO specialists automating sitemap analysis and URL gathering.
- Webmasters needing automated URL extraction respecting robots.txt.
- Developers building web crawlers or data pipelines using `n8n`.
- Automation engineers integrating multi-level sitemap parsing.
- Anyone interested in extending sitemap processing to media-rich content.
- Beginners looking for a comprehensive example of recursion and filtering in `n8n`.

---

### ❓ What Problem Does It Solve?
Sitemaps often include indexes referencing multiple other sitemaps, creating a nested structure that is complicated to parse manually or with simple tools. Moreover, filtering URLs based on robots.txt rules and extracting detailed metadata requires custom logic. This workflow automates these tasks within `n8n`, enabling scalable, repeatable, and respectful URL extraction that prevents infinite recursions and respects site crawling policies.

---

### 🔧 How This Workflow Works
1. **Entry Point:** Receives initial sitemap URL, maximum crawl depth, and robots.txt rules.
2. **Fetch Sitemap:** Downloads sitemap XML content from the given URL.
3. **Parse XML:** Parses XML to identify sitemap type: either a sitemap index or URL set.
   - Extracts URLs, sitemaps, metadata (lastmod, priority, changefreq), images, videos, news data.
4. **Check Recursion:** Determines if the sitemap is an index requiring recursion.
5. **Recursion Handling:** 
   - If sitemap index, splits child sitemaps and normalizes data.
   - Loops back to fetch each child sitemap, incrementing crawl depth.
   - Stops recursion if max depth is exceeded.
6. **URL Extraction:**
   - If URL set, splits URLs array into single records.
   - Filters only items with type `url` (excludes sitemap references).
   - Renames `loc` field to `url` and retains all metadata fields.
7. **Aggregation:**
   - Combines all filtered URLs into a single array.
8. **Filtering:**
   - Applies robots.txt rules to each URL.
   - Flags URLs as allowed or blocked, with reasons.
   - Filters out blocked URLs and returns only allowed URLs.
9. **Output:** Outputs a clean list of URLs respecting crawl policies and enriched with metadata.

---

### 🔐 Setup Instructions
- ✅ **Install and run `n8n`** in your environment with internet access.
- ✅ **Create credentials** if HTTP requests require authentication (optional depending on sitemap source).
- ✅ **Prepare robots.txt rules** in JSON format, if filtering by robots is needed.
- ✅ **Import the workflow JSON** into `n8n` via the import feature.
- ✅ **Configure the "Receive Sitemap Input" node**:
  - Set the initial sitemap URL in the `sitemaps` field.
  - Define `maxCrawlDepth` to limit recursion depth (e.g., 5).
  - Provide `robotsRules` parsed from robots.txt in expected structure.
- ✅ **Execute the workflow** in `n8n` and monitor the progress.
- ✅ **Inspect output** from the "Keep Only Allowed URLs" node for final allowed URLs.
  
---

### 📅 Payload
| Key              | Definition                                   |
|------------------|----------------------------------------------|
| sitemaps         | URL of sitemap or sitemap index to fetch     |
| sitemapDepth     | Current recursion depth (integer)             |
| maxCrawlDepth    | Maximum allowed recursion depth (integer)     |
| urls             | Array of extracted URLs or child sitemaps     |
| loc              | URL location inside sitemap                    |
| lastmod          | Last modification date (string)                |
| priority         | Sitemap priority value (string)                 |
| changefreq       | Expected change frequency (string)              |
| parentSitemap    | URL of parent sitemap (string)                  |
| type             | Type of item: 'url' or 'sitemap'               |
| images           | Array of image metadata related to URL         |
| videos           | Array of video metadata related to URL         |
| news             | News metadata if present (object)               |
| status           | Filtering status: 'allowed' or 'blocked_by_robots' |
| filterReason     | Reason URL was blocked (string)                  |

**Example JSON Payload:**
```json
{
  "maxCrawlDepth": 5,
  "sitemaps": "https://example.com/sitemap.xml",
  "sitemapDepth": 0,
  "robotsRules": {}
}
```

**Example cURL Test:**
```bash
curl -X POST https://your-n8n-instance/webhook/your-webhook-url \
-H 'Content-Type: application/json' \
-d '{"sitemaps":"https://example.com/sitemap.xml","maxCrawlDepth":5,"sitemapDepth":0,"robotsRules":{}}'
```

---

### 🔨 Tools/Node Used
- **Execute Workflow Trigger** ("Receive Sitemap Input"): Entry point accepting input parameters.
- **HTTP Request** ("Fetch Sitemap XML"): Downloads sitemap XML content.
- **Code Node** ("Parse XML Sitemap"): Parses XML data, extracting URLs and metadata, recognizing sitemap type.
- **Code Node** ("Check Recursion Needed"): Determines if recursion is needed based on sitemap type.
- **If Node** ("Is Sitemap Index?"): Routes processing based on sitemap type.
- **Split Out Node** ("Split Child Sitemaps" / "Split URLs from Final Sitemap"): Splits array fields to individual items.
- **Code Node** ("Normalize for Recursion"): Formats child sitemap entries for recursive fetching.
- **Set Node** ("Normalize URL Fields"): Renames XML fields to standard keys.
- **Aggregate Node** ("Aggregate All URLs"): Combines items into arrays.
- **Code Node** ("Apply Robots Filters"): Filters URLs against robots.txt rules.
- **Filter Node** ("Keep Only Allowed URLs"): Provides final filtering to only allowed URLs.

---

### ⚙️ Reactive & Proactive Behavior
- **Reactive:** The workflow reacts to the initial sitemap input and dynamically downloads and parses sitemaps.
- **Proactive:** Uses recursion with depth checks to proactively traverse nested sitemap indices.
- **Error Handling:** HTTP errors continue with error output without stopping the workflow. Code nodes safely catch and report parsing errors.
- **Filtering:** Proactively applies robots.txt rules to avoid processing disallowed URLs.

### 🐞 Error Handling
- **HTTP Request Node:** Retries on failure; continues workflow on errors.
- **Parse XML Code Node:** Catches XML parsing exceptions; outputs error details in JSON.
- **Recursion Limiting:** Workflow stops processing if recursion depth exceeds `maxCrawlDepth`.
- **Robot Filter:** Logs and flags URLs blocked by robots.txt rather than failing.
  
---

### 🧩 Requirements
- Access to the internet to fetch sitemap URLs.
- `n8n` version supporting the following nodes and versioning:
  - HTTP Request (v4.2)
  - Code (v2)
  - Execute Workflow Trigger (v1.1)
  - Split Out (v1)
  - Set (v3.4)
- JSON-formatted robots.txt rules for filtering (optional but recommended).
- Properly formed XML sitemaps compliant with standard sitemap schema.
- Permissions to run recursive workflows if used in limited environments.

---

### 📚 Resources
- [Sitemap XML Protocol](https://www.sitemaps.org/protocol.html)
- [Robots.txt Specification](https://developers.google.com/search/docs/advanced/robots/intro)
- [n8n Documentation](https://docs.n8n.io/)
- [JavaScript RegExp Guide](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions)
- [n8n HTTP Request Node](https://docs.n8n.io/nodes/n8n-nodes-base.httprequest/)
- [n8n Code Node](https://docs.n8n.io/nodes/n8n-nodes-base.function/)

---

### 🐞 Troubleshooting
- **Infinite Loop:** Ensure `maxCrawlDepth` is correctly set to prevent endless recursion.
- **Empty Output:** Verify input sitemap URL is reachable and returning valid XML.
- **Blocked URLs Unexpectedly:** Confirm your robots.txt `robotsRules` JSON structure matches expected format.
- **HTTP Failures:** Check network access and permissions; inspect HTTP Request node logs.
- **Parsing Errors:** Look for malformed XML or unexpected sitemap markup.
- **No URLs After Filtering:** Confirm robots.txt rules are not over-restrictive.
- **Performance Issues:** Large sitemaps with deep recursion may require longer execution times.
- **Credential Issues:** If protected sitemaps require credentials, configure HTTP Request node accordingly.
- **Version Compatibility:** Ensure node versions align with this workflow’s nodeVersion specifications.
- **Output Mapping:** Make sure your downstream workflows expect fields normalized (e.g., `url` instead of `loc`).

---