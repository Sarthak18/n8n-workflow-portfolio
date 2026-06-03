**AI Idea Validator**


An intelligent webhook-powered workflow that evaluates product ideas using AI and provides instant feedback with ratings.

## Overview
This n8n workflow receives product ideas via webhook, analyzes them using Claude Sonnet 4.6 (Anthropic), and returns a structured evaluation with a numerical score (1-10) and actionable feedback.

**Features**
- **Instant Evaluation:** Real-time idea assessment via HTTP POST endpoint
- **AI-Powered Analysis:** Leverages Claude Sonnet 4.6 for intelligent product idea evaluation
- **Structured Output:** Returns JSON response with score and feedback
- **Secure Access:** Protected with header authentication
- **Scalable:** Handles multiple concurrent requests

## How It Works
- **Webhook Trigger:** Receives POST requests at /idea-rater endpoint
- **AI Agent Processing:** Claude Sonnet 4.6 evaluates the idea based on:
- **Score:** Numerical rating (1-10)
- **feedback:** Two-sentence evaluation summary

This workflow was created and refined using Lovable, an AI-powered development platform that helps you build applications and automations faster.
