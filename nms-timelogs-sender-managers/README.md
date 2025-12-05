## NMS Timelogs Sender - Managers
![Latest-0.0.1](https://img.shields.io/badge/Latest-0.0.1-blue)

This `n8n` workflow is designed to automatically send summaries of invalid employee time logs grouped by their respective managers. It extracts invalid time attendance records from a Google Sheet, groups employees by manager email, composes detailed HTML email reports, and sends these notifications to respective managers daily at a scheduled time.

![](https://bin.nmscreative.com/1bhm7g.png)

---

### 💡 Why Use NMS Timelogs Sender - Managers?
- Automate daily reporting of employee invalid time logs to managers.
- Reduce manual effort in gathering and communicating attendance discrepancies.
- Provide detailed, well-organized reports including time logs and reasons for invalid entries.
- Ensure timely communication with managers for prompt follow-up actions.
- Group multiple employees per manager email to minimize email clutter.

---

### ⚡ Who Is This For?
- HR teams and attendance administrators managing employee time logs.
- Managers who need regular updates on their team's attendance irregularities.
- Automation specialists integrating time-tracking systems with communication tools.
- Organizations using Google Sheets for timesheet management.
- Users of `n8n` looking to automate attendance notifications via Gmail.

---

### ❓ What Problem Does It Solve?
Manual review and reporting of invalid attendance logs can be time-consuming and prone to delays. This workflow automates extraction, grouping, and notification, ensuring managers receive accurate and timely updates about their team’s attendance issues for improved oversight and corrective action.

---

### 🔧 How This Workflow Works
1. **Schedule Trigger:** Executes daily at 9:15 AM to start the process automatically.
2. **Get Invalid Time Logs:** Retrieves invalid attendance records from a specified Google Sheet.
3. **Group by Managers:** Processes and groups employee records by their manager's email(s); defaults to HR email if no manager email found.
4. **Loop Over Items:** Iterates through each manager group.
5. **Clear Time Logs:** Clears the source time logs in the Google Sheet to prepare for new data.
6. **Compose Email:** Constructs a detailed HTML email report for each manager, showing employees’ invalid logs with rich formatting.
7. **Send Invalid Logs to Managers:** Emails the composed summaries to each manager using Gmail.
8. **Wait:** Pauses for 2 seconds before repeating the loop as a pacing mechanism.

---

### 🔐 Setup Instructions
- Import this JSON workflow into your `n8n` instance.
- Configure Google Sheets credentials with read/write access to the specified time logs spreadsheet.
- Set up Gmail credentials for email sending capabilities.
- Verify the Google Sheet document ID and sheet name points correctly to your invalid logs data.
- Adjust "Schedule Trigger" timing as needed.
- Confirm the default HR email address in the "Group by Managers" node code or customize it.
- Test by running the workflow manually before enabling the scheduled trigger.
- Ensure all code nodes’ JavaScript is suitable for your data schema.

---

### 📅 Payload
 
- Collection of invalid timelogs from google sheet.

---

### 🔨 Tools/Nodes Used
- **Schedule Trigger:** Initiates workflow at 9:15 AM daily.
- **Google Sheets (Get Invalid Time Logs):** Reads invalid attendance data.
- **Code (Group by Managers):** Groups employees by their manager emails.
- **SplitInBatches (Loop Over Items):** Iterates through each manager-group for individual processing.
- **Google Sheets (Clear Time Logs):** Clears invalid logs after processing.
- **Code (Compose Email):** Builds the HTML formatted email content with employee invalid logs.
- **Gmail (Send Invalid Logs to Managers):** Sends the composed emails securely via Gmail.
- **Wait:** Adds small delays between sending batches to avoid API throttling.

---

### ⚙️ Reactive & Proactive Behavior
- **Proactive Scheduled Trigger:** Automatically runs every workday at a fixed time without user interaction.
- **Batch Processing:** Handles multiple managers and employees efficiently by batch splitting iteration.
- **Dynamic Email Generation:** Emails are auto-generated dynamically based on current data grouping and content.

---

### 🧩 Requirements
- An active `n8n` instance with internet access.
- Google Sheets API credentials configured with OAuth2 and proper spreadsheet access.
- Gmail API credentials configured with OAuth2 for email sending.
- Access to accurate and up-to-date time log data in the configured Google Sheet.
- Working `n8n` Gmail integration node with proper permissions.
- JavaScript enabled and allowed in `n8n` for code nodes.

---

### 📚 Resources
- [n8n Documentation](https://docs.n8n.io/)
- [Google Sheets API Documentation](https://developers.google.com/sheets/api)
- [Gmail API Documentation](https://developers.google.com/gmail/api)
- [JavaScript Array and Object Documentation](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array)
- [HTML Email Formatting Tips](https://www.campaignmonitor.com/dev-resources/guides/coding-html-email/)

---

### 🐞 Troubleshooting
- Ensure all Google Sheets IDs and Sheet names are correct and accessible.
- Validate OAuth2 credentials for Google Sheets and Gmail nodes are current and authorized.
- Check the default HR email fallback address in the grouping code for accuracy.
- Confirm that time logs data structure matches the expectations of the code nodes (especially timeLogs JSON parsing).
- Monitor for API rate limits or throttling in Gmail node; use Delay/Wait nodes if needed.
- Review error messages in `n8n` executions logs for failed nodes.
- Make sure the scheduled trigger is enabled to run on time.
- Verify employee data contains proper manager email(s); missing or malformed data may cause grouping errors.
- Test send email node separately with a sample payload to isolate issues.
- Validate the HTML email formatting in popular email clients; tweak styles if needed.

---