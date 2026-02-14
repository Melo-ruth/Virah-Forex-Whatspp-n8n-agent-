Virah Forex Bureau – WhatsApp AI Agent

An intelligent WhatsApp automation system built with n8n that handles customer inquiries for Virah Forex Bureau using AI, memory, knowledge base logic, and Google Sheets integration.

🚀 System Overview

This WhatsApp AI Agent automatically processes incoming messages and determines whether to:
 Answer from internal knowledge base
 Retrieve working hours from Google Sheets
 Retrieve branch locations from Google Sheets
 Maintain conversational memory per user

The system uses AI tool routing to dynamically decide which data source to call.

🧠 Current Workflow Architecture

WhatsApp Trigger  
↓  
Extract & Clean Message  
↓  
Virah Forex Knowledge Base (Context Injection)  
↓  
AI Agent  
   ├── OpenRouter Chat Model  
   ├── Simple Memory  
   ├── Google Sheets Tool: Working Hours  
   └── Google Sheets Tool: Branch Locations  
↓  
Send Text Message (WhatsApp Response)

🔄 Workflow Logic Explained

1️⃣ WhatsApp Trigger
Captures incoming customer messages.

2️⃣ Extract & Clean Message
Prepares user message for processing (removes metadata, formats text).

3️⃣ Knowledge Base Node
Injects structured business information such as:
 Public holiday policy
 Forex policies
 Business rules

This ensures the AI does NOT hallucinate incorrect data.

4️⃣ AI Agent (Core Brain)
The AI Agent:
Uses OpenRouter Chat Model
Uses Simple Memory (session per WhatsApp number)
Has access to two Google Sheets tools:
  Working Hours at Virah
  Branch Locations

The model decides automatically whether to:
 Answer directly
 Call Working Hours sheet
 Call Branch Locations sheet

🧠 Simple Memory Configuration

Memory Type: Simple Memory  
Session ID: WhatsApp sender number  

Example:
{{$json["from"]}}

This allows:
- Follow-up questions
- Context awareness
- Natural conversations

Example:

User: What are your working hours?  
User: What about on Sundays?  

The agent remembers the topic.


📊 Google Sheets Tools

Working Hours at Virah
Used when user asks:
- Working hours
- Public holidays
- Sunday hours
- Closing time

Branch Locations
Used when user asks:
- How many branches?
- Where are you located?
- Branch address

🔒 Security

This repository does NOT contain:

 API keys
 WhatsApp tokens
 OpenRouter API keys
 Google credentials
 Environment secrets

All credentials are configured inside n8n.

🧪 Example Test Cases

Branch Count
User: How many branches do you have?  
→ AI calls Branch Locations sheet  
→ Returns correct count

Working Hours
User: What time do you close?  
→ AI calls Working Hours sheet  
→ Returns closing time

Public Holidays
User: Do you work on public holidays?  
→ AI answers using Knowledge Base context

🛠 Tech Stack

  n8n
  WhatsApp API
  OpenRouter Chat Model
  Google Sheets API
  Simple Memory



⚙️ Setup Instructions

1. Import workflow JSON into n8n.
2. Configure credentials:
   - WhatsApp API
   - OpenRouter
   - Google Sheets
3. Configure Simple Memory session ID.
4. Activate workflow.



 📈 Future Improvements

Multi-language support
  Branch-specific forex rate queries
  CRM integration
  Admin dashboard
  Rate auto-updates
  Logging and analytics



