**Appointment Reminder - Voice Agent**


An automated n8n workflow that sends AI-powered voice call reminders for upcoming appointments by integrating Google Calendar with Retell AI's voice calling platform.

## Overview
This workflow automatically:

- Checks Google Calendar daily for appointments in the next 24 hours
- Extracts appointment details using AI (GPT-4.1-mini) from event descriptions
- Initiates voice calls via Retell AI to remind attendees about their appointments
- Workflow Architecture
- Schedule Trigger (Daily at midnight)
    ↓
- Get Google Calendar Events (Next 24 hours)
    ↓
- AI Extraction (GPT-4.1-mini with JSON schema)
    ↓
- Format Data (Edit Fields)
    ↓
- Trigger Voice Call (Retell AI API)

## Features
**Automated Daily Checks:** Runs every day at midnight to scan for upcoming appointments
**AI-Powered Data Extraction:** Uses GPT-4.1-mini to intelligently parse event descriptions and extract:
**Voice Call Reminders:** Automatically initiates personalized voice calls through Retell AI
**Structured Data Handling:** Enforces JSON schema validation for consistent data extraction
**Extract structure fields from the event.**
If a field is missing, return empty string "".
Return any JSON that matches the required schema

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
