> ⚠️ Kindly review the docs once more before saving.

## NMS Time Logs Validator
![Latest-0.0.1](https://img.shields.io/badge/Latest-0.0.1-blue)

NMS Time Logs Validator is an `n8n AI agent workflow` designed to automatically validate employee time log entries against their scheduled shifts. It identifies invalid or missing time logs based on schedule adherence, leave status, and log sequence correctness, helping streamline attendance validation and reporting processes.

![NMS Time Logs Validator screenshot](https://bin.nmscreative.com/x8cjv3.png)

---

### 💡 Why Use NMS Time Logs Validator?
- Automate verification of employee time punch logs versus scheduled shifts.
- Detect invalid logs outside allowable time buffers and mismatches with scheduled leave types.
- Support half-shift and whole-day leave validations with custom logic.
- Automatically generate detailed validation reports containing reasons for invalid entries.
- Integrate with Google Sheets to store invalid log records for audit or further processing.
- Prepare formatted email summaries for employees or managers (nodes included but disabled).
- Improve efficiency and accuracy over manual attendance checking.

---

### ⚡ Who Is This For?
- HR teams and attendance administrators needing automated time log validation.
- Companies with shift schedules requiring log-ins, log-outs, and leave reconciliations.
- Automation engineers integrating attendance systems into digital workflows.
- n8n users seeking automated validation workflows with advanced JavaScript logic.
- Organizations aiming to reduce errors in manual time log reviews.

---

### ❓ What Problem Does It Solve?
Manual checking of employee time logs against scheduled shifts and leave types is tedious, error-prone, and time-consuming. This workflow automates the process by:
- Validating time logs timing against scheduled shifts plus buffer periods.
- Accounting for overnight shifts and special leave scenarios.
- Checking the proper sequence and count of log-in/out pairs depending on leave status.
- Producing clear reasons for validation failure aiding in corrective actions.
- Storing invalid records centrally for management or audit purposes.

---

### 🔧 How This Workflow Works
1. **Webhook Trigger**: Accepts HTTP POST requests with Excel files containing daily attendance time log data.
2. **Extract from File**: Reads the uploaded Excel file and extracts time logs data.
3. **Filter / Remove Break Logs**: Parses and filters active time logs as JSON arrays; fixes date serials from Excel format.
4. **Check logs if Within Sched**: Compares each time log timestamp against scheduled shift time with ±1 hour buffer; flags logs outside this range as invalid.
5. **Check time logs**: Applies complex validation rules:
   - Validates correct number and sequence of In/Out logs.
   - Checks compliance with leave types (whole day, first shift, second shift).
   - Detects missing logs, extra logs, or logs outside allowed periods.
6. **Batch & Store invalid logs**: Splits invalid records in batches for output and appends them to a Google Sheet for record keeping.
7. **Optional Email Notifications** (disabled): Includes nodes for generating formatted email reports per employee or manager summarizing invalid logs.
8. **Loop & Wait Controls**: Manages batch processing and paced sending of messages to avoid API rate limits.

---

### 🔐 Setup Instructions
- Import the workflow JSON into your n8n instance.
- Configure necessary credentials:
  - Google Sheets OAuth2 for storing invalid logs.
  - Gmail OAuth2 (optional) for sending email reports.
- Set up the webhook node with a public accessible URL.
- Prepare and upload Excel files with specific columns: employeeId, name, scheduledTime, timeLogs JSON, onLeave status, etc.
- Verify timezone assumptions (Asia/Manila, +08:00 offset) and adjust if required.
- Enable or disable optional email notification nodes as needed.
- Test the workflow by POSTing Excel files containing example attendance records.

---

### 📅 Payload
| Key             | Definition                                                                                 |
|-----------------|--------------------------------------------------------------------------------------------|
| employeeId      | Unique employee identifier                                                                |
| name            | Employee name                                                                             |
| date            | Date of the attendance record                                                            |
| scheduledTime   | Scheduled shift time range string, e.g. "09:00 - 18:00"                                  |
| timeLogs        | JSON array of time log entries, each with timestamp and type (In/Out) and status          |
| onLeave         | Leave type string ("whole day", "1st shift", "2nd shift", or empty)                      |
| invalidLogs     | List of timestamps identified as outside allowed schedule buffer                         |
| finalStatus     | Overall validation result for the record ("Valid" or "Invalid")                         |
| reason          | Explanation for invalid status                                                           |
| companyEmail    | Employee company email (for messaging nodes)                                             |
| managerEmail    | Manager email (for notifications, grouping)                                             |

**Example JSON Payload Snippet** (within each extracted record):
```json
{
  "employeeId": "E1234",
  "name": "John Doe",
  "date": "2024-06-01",
  "scheduledTime": "09:00 - 18:00",
  "timeLogs": [
    {"timestamp": "2024-06-01T08:45:00+08:00", "type": "in", "status": "active"},
    {"timestamp": "2024-06-01T12:00:00+08:00", "type": "out", "status": "active"},
    {"timestamp": "2024-06-01T13:00:00+08:00", "type": "in", "status": "active"},
    {"timestamp": "2024-06-01T18:30:00+08:00", "type": "out", "status": "active"}
  ],
  "onLeave": "",
  "invalidLogs": ["2024-06-01T08:45:00+08:00"], 
  "finalStatus": "Invalid",
  "reason": "Logs outside allowed range"
}
```

**Example cURL Test to Trigger Webhook**:
```bash
curl --request POST \
  --url https://YOUR_WORKSPACE.n8n.cloud/webhook/2d07e3c3-fe9f-4660-b821-6e16d60ea553 \
  --header 'Content-Type: multipart/form-data' \
  --form 'data=@./attendance_log.xlsx'
```

---

### 🔨 Tools/Node Used
- **Webhook**: Accepts HTTP POST request with Excel file uploads.
- **Extract from File**: Converts Excel sheets into JSON data.
- **Filter / Remove break logs**: Parses and filters relevant active time logs.
- **Check logs if Within Sched**: Script node checking log timestamps against scheduled shift with buffer.
- **Check time logs**: Core script performing comprehensive validation of logs vs shift and leave rules.
- **Loop Over Items1**: Processes records in manageable batches.
- **Store Detected invalid time logs**: Appends invalid records to Google Sheets.
- **Send to Employee / Send a message / Notify Manager**: Email notification nodes (currently disabled).
- **Wait 1 Sec**: Control node to pace notifications.
- **No Operation, do nothing**: Placeholder for branching logic.
- **Collect manager emails and users**: Groups invalid logs by manager (disabled).

---

### ⚙️ Reactive & Proactive Behavior
- **Reactive**: Responds to uploaded Excel file webhook input to immediately process and validate attendance data.
- **Proactive**: Prepared nodes to notify employees or managers with reports based on validation results (disabled, but available to activate).
- Handles overnight shifts and flexible leave scenarios natively in the validation code.

---

### 🧩 Requirements
- n8n instance with Google Sheets and Gmail nodes configured.
- OAuth2 credentials for Google Sheets API (for storing outputs).
- OAuth2 credentials for Gmail (optional, if notifications are enabled).
- Publicly accessible webhook endpoint for file uploads.
- Attendance Excel files formatted following the expected schema.
- Understanding of +08:00 timezone or adjustment to other timezones in validation code.

---

### 📚 Resources
- Official n8n Documentation: https://docs.n8n.io/
- Google Sheets API Reference: https://developers.google.com/sheets/api
- Gmail API Reference: https://developers.google.com/gmail/api/
- JavaScript date and array filtering documentation for custom node scripting.

---

### 🐞 Troubleshooting
- Ensure the Excel file schema matches expected columns (employeeId, timeLogs as valid JSON string, scheduledTime format).
- Watch for timezone discrepancies if logs are in different timezones—adjust code offsets accordingly.
- Check Google Sheets document ID & sheet name correctness in configuration.
- If no invalid logs appear but data is expected, verify the parsing logic and time formats.
- Debug using execution logs and by isolating node outputs for step-by-step data validation.
- For email failures, verify Gmail OAuth2 credentials and sender permissions.
- Handle Excel date serial numbers carefully—code includes fix but data variation may require tweaks.
- Disabled nodes can be enabled for email notifications if desired; ensure required credentials before activation.
---
