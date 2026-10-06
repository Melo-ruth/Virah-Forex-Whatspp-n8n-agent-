# Virah Forex Bureau – WhatsApp AI Agent ("Valerie")

An intelligent WhatsApp automation system built with n8n that handles customer inquiries for Virah Forex Bureau, a licensed multi-branch forex bureau in Uganda. The agent is live in production and answers customer questions about working hours, branch locations, forex policies and business rules, escalating to staff when it cannot help.

---

## 📌 Version History

| Version | Status | Data & Memory | Key Features |
|---|---|---|---|
| **v1** (prototype) | Superseded | Google Sheets + n8n Simple Memory | AI tool routing, knowledge base context injection |
| **v2** (production) | **Live** | Supabase (PostgreSQL) | Persistent conversation storage, three-layer error handling and escalation, Meta-verified production deployment |

The workflow documented in detail below is **v1**, which established the core design. The production upgrades in v2 are summarised in the next section.

---

## 🚀 Production System (v2)

The production version builds on the v1 architecture with the changes needed to run reliably for real customers:

- **Supabase (PostgreSQL) backend** replacing Google Sheets and in-memory session storage, with a purpose-designed schema for conversations and business data.
- **GPT-4o mini via OpenRouter** as the chat model.
- **Three-layer error handling and escalation architecture**, with unresolved queries escalated to staff by Gmail.
- **Duplicate-message protection** to handle Meta's webhook retry behaviour, which previously caused crash loops and repeated replies.
- **Production deployment on the Meta WhatsApp Business Cloud API**, including Meta app verification and live webhook configuration in n8n.
- **Meta-compliant privacy policy** covering data storage, third-party processing and a 12-month data retention period.

### Production challenges solved
- Webhook verification failures during Meta setup
- Crash loops and duplicate replies caused by Meta webhook retries
- n8n workflow auto-deactivation
- A flagged phone number (WhatsApp error 131031)

---

## 🧠 v1 Workflow Architecture


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


The system uses AI tool routing to dynamically decide which data source to call.

---

## 🔄 Workflow Logic Explained

**1️⃣ WhatsApp Trigger**
Captures incoming customer messages.

**2️⃣ Extract & Clean Message**
Prepares the user message for processing (removes metadata, formats text).

**3️⃣ Knowledge Base Node**
Injects structured business information such as:
- Public holiday policy
- Forex policies
- Business rules

This ensures the AI does not hallucinate incorrect data.

**4️⃣ AI Agent (Core Brain)**
The AI Agent:
- Uses the OpenRouter Chat Model
- Uses Simple Memory (one session per WhatsApp number)
- Has access to two Google Sheets tools: Working Hours at Virah, and Branch Locations

The model decides automatically whether to:
- Answer directly
- Call the Working Hours sheet
- Call the Branch Locations sheet

---

## 🧠 Memory Configuration (v1)

Memory Type: Simple Memory
Session ID:  WhatsApp sender number
Example:     {{$json["from"]}}


This allows follow-up questions, context awareness and natural conversations.


User: What are your working hours?
User: What about on Sundays?


The agent remembers the topic.

---

## 📊 Google Sheets Tools (v1)

**Working Hours at Virah**, used when the user asks about:
- Working hours
- Public holidays
- Sunday hours
- Closing time

**Branch Locations**, used when the user asks:
- How many branches?
- Where are you located?
- Branch address

---

## 🧪 Example Test Cases

| User message | Expected behaviour |
|---|---|
| How many branches do you have? | Calls Branch Locations → returns correct count |
| What time do you close? | Calls Working Hours → returns closing time |
| Do you work on public holidays? | Answers from Knowledge Base context |

---

## 🛠 Tech Stack

| | v1 | v2 (production) |
|---|---|---|
| Orchestration | n8n | n8n |
| Messaging | WhatsApp API | Meta WhatsApp Business Cloud API |
| LLM | OpenRouter Chat Model | GPT-4o mini via OpenRouter |
| Data | Google Sheets API | Supabase (PostgreSQL) |
| Memory | n8n Simple Memory | Supabase |
| Escalation | — | Gmail |

---

## 🔒 Security

This repository does **not** contain:
- API keys
- WhatsApp tokens
- OpenRouter API keys
- Google or Supabase credentials
- Environment secrets

All credentials are configured inside n8n.

---

## ⚙️ Setup Instructions (v1)

1. Import the workflow JSON into n8n.
2. Configure credentials:
   - WhatsApp API
   - OpenRouter
   - Google Sheets
3. Configure the Simple Memory session ID.
4. Activate the workflow.

---

## 📈 Future Improvements

- Multi-language support
- Branch-specific forex rate queries
- Automatic rate updates
- CRM integration
- Admin dashboard
- Logging and analytics
