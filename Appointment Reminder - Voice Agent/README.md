Appointment Reminder - Voice Agent
An automated n8n workflow that sends AI-powered voice call reminders for upcoming appointments by integrating Google Calendar with Retell AI's voice calling platform.

Overview
This workflow automatically:

Checks Google Calendar daily for appointments in the next 24 hours
Extracts appointment details using AI (GPT-4.1-mini) from event descriptions
Initiates voice calls via Retell AI to remind attendees about their appointments
Workflow Architecture
Schedule Trigger (Daily at midnight)
    ↓
Get Google Calendar Events (Next 24 hours)
    ↓
AI Extraction (GPT-4.1-mini with JSON schema)
    ↓
Format Data (Edit Fields)
    ↓
Trigger Voice Call (Retell AI API)
Features
Automated Daily Checks: Runs every day at midnight to scan for upcoming appointments
AI-Powered Data Extraction: Uses GPT-4.1-mini to intelligently parse event descriptions and extract:
Name
Email
Phone number
Appointment reason
Start time
End time
Description
Voice Call Reminders: Automatically initiates personalized voice calls through Retell AI
Structured Data Handling: Enforces JSON schema validation for consistent data extraction
Prerequisites
Required Accounts & Credentials
n8n Instance (self-hosted or cloud)
Google Calendar API
OAuth2 credentials configured in n8n
Access to the calendar: sarthkgpta@gmail.com
OpenAI API
API key for GPT-4.1-mini access
Retell AI Account
API key for authentication
Configured agent ID: agent_ee12183d6672dd534b57a2a094
Phone number: +17255255621
Setup Instructions
1. Import the Workflow
# Import workflow.js into your n8n instance
# File location: /workflow.js
2. Configure Credentials
Set up the following credentials in n8n:

Google Calendar OAuth2 API
Credential name: Google Calendar OAuth2 API
Type: OAuth2
Follow n8n's Google Calendar authentication guide
OpenAI API
Credential name: n8n free OpenAI API credits (or your own OpenAI API key)
Type: API Key
Add your OpenAI API key
Retell AI Custom Auth
Credential name: Custom Auth account
Type: HTTP Custom Auth
Add your Retell AI API key as a header
3. Customize Configuration
Update the following values in workflow.js:

// Line 12: Update calendar email
calendar: { __rl: true, value: 'YOUR_CALENDAR_EMAIL@gmail.com', ... }

// Line 30: Update Retell AI configuration
"from_number": "YOUR_RETELL_PHONE_NUMBER",
"override_agent_id": "YOUR_RETELL_AGENT_ID"
4. Adjust Schedule (Optional)
The workflow runs daily at midnight by default. To change:

// Line 6: Modify schedule trigger
rule: { interval: [{ 
  field: 'days', 
  daysInterval: 1,  // Run every N days
  triggerAtHour: 0,  // Hour (0-23)
  triggerAtMinute: 0  // Minute (0-59)
}] }
How It Works
Step 1: Schedule Trigger
Executes daily at midnight (00:00) to check for upcoming appointments.

Step 2: Fetch Calendar Events
Retrieves up to 50 events from Google Calendar within the next 24 hours using:

timeMin: Current time ($now)
timeMax: 24 hours from now ($now.plus(24,'hours'))
Step 3: AI Data Extraction
Sends event details to GPT-4.1-mini with a structured JSON schema to extract:

Contact information (name, email, phone)
Appointment details (reason, start/end times, description)
System Prompt:

Extract structure fields from the event.
If a field is missing, return empty string "".
Return any JSON that matches the required schema
Step 4: Format Data
Transforms AI output into clean fields:

Name
Email
Phone Number
Reason
Start Time
End Time
Step 5: Initiate Voice Call
Makes a POST request to Retell AI API with:

Recipient phone number
Dynamic variables (name, reason, times)
Agent configuration
Calendar Event Format
For best results, structure your Google Calendar event descriptions like this:

Name: John Doe
Email: john.doe@example.com
Phone: +1234567890
Reason: Annual checkup
The AI will extract these fields even if the format varies slightly.

API Integration Details
Retell AI API Call
Endpoint: https://api.retellai.com/v2/create-phone-call

Payload:

{
  "from_number": "+17255255621",
  "to_number": "{{ extracted_phone }}",
  "retell_llm_dynamic_variables": {
    "name": "{{ extracted_name }}",
    "phone_number": "{{ extracted_phone }}",
    "Reason": "{{ extracted_reason }}",
    "start_time": "{{ extracted_start }}",
    "end_time": "{{ extracted_end }}"
  },
  "override_agent_id": "agent_ee12183d6672dd534b57a2a094"
}
Troubleshooting
No calls being made?
Check that calendar events exist in the next 24 hours
Verify Google Calendar credentials are valid
Ensure event descriptions contain phone numbers
AI extraction failing?
Verify OpenAI API key is valid and has credits
Check that event descriptions contain the required fields
Review the JSON schema in the "Message a model" node
Voice calls not connecting?
Confirm Retell AI API key is correct
Verify phone numbers are in E.164 format (+1234567890)
Check Retell AI agent ID is active
Customization Ideas
Multi-language support: Modify AI prompts for different languages
SMS fallback: Add Twilio/SMS node if voice call fails
Email reminders: Add Gmail node for email notifications
Custom reminder timing: Adjust schedule to run multiple times per day
Confirmation tracking: Store call results in a database
Security Notes
Never commit credentials to version control
Use n8n's credential management system
Rotate API keys regularly
Limit calendar access to specific calendars only
License
This workflow is provided as-is for educational and commercial use.

Support
For issues or questions:

n8n Documentation: https://docs.n8n.io
Retell AI Docs: https://docs.retellai.com
Google Calendar API: https://developers.google.com/calendar
