# 🤖 n8n AI Automation Learning Journey

A hands-on learning repository documenting my journey of building **AI-powered automation workflows using n8n, Docker, APIs, Webhooks, and Groq AI**.

The goal of this repository is to develop practical skills in workflow automation and AI integration by progressing from basic n8n concepts to real-world AI automation projects.

---

## 📌 About This Repository

This repository documents my practical learning and experimentation with **n8n**, an open-source workflow automation platform.

Rather than focusing only on theoretical concepts, I am building workflows to understand how modern automation systems connect:

- APIs
- Webhooks
- Conditional logic
- JSON data
- Large Language Models (LLMs)
- AI APIs
- Business automation workflows

My long-term goal is to apply these skills to build **AI-powered automation microservices for small and local businesses**, including:

- WhatsApp customer support assistants
- Automated FAQ systems
- Lead capture workflows
- Lead qualification systems
- AI-powered customer support
- Automated lead routing
- Business process automation

---

# 🛠️ Local Development Environment

I configured n8n locally on **Windows 11** using:

- Docker Desktop
- Windows Subsystem for Linux 2 (WSL2)
- n8n
- Groq API
- PowerShell
- Git
- GitHub

My local n8n instance runs at:

```text
http://localhost:5678
```

> Note: API keys and credentials are never committed to this repository.

---

# 📚 Learning Progress

## 1. n8n Local Setup with Docker and WSL2

Configured a local development environment for running n8n using Docker Desktop and WSL2 on Windows 11.

Key concepts learned:

- Docker containers
- Running n8n locally
- Starting and stopping the n8n environment
- Accessing the n8n workflow editor
- Understanding local workflow development

---

## 2. Manual Trigger

Learned how to use the **Manual Trigger** node to start workflows manually during development and testing.

Basic workflow:

```text
Manual Trigger
      ↓
Workflow Logic
```

This is useful for testing workflows before connecting them to real-world triggers such as APIs, webhooks, or messaging platforms.

---

## 3. Edit Fields (Set)

Learned how to use the **Edit Fields (Set)** node to create and manipulate data inside an n8n workflow.

Example fields:

```text
name = Nagendra
city = Chennai
```

Also practiced using n8n expressions such as:

```text
{{ $now }}
```

to dynamically generate values such as the current timestamp.

---

## 4. Conditional Logic with the IF Node

Built workflows using the **IF** node to create conditional branches.

Example:

```text
Manual Trigger
      ↓
Edit Fields
      ↓
IF city == "Chennai"
     /       \
   TRUE      FALSE
    ↓          ↓
 "Local!"   "Remote!"
```

This introduced conditional workflow execution and decision-making logic.

---

## 5. Working with Public APIs

Learned how to call external APIs using the **HTTP Request** node.

Practiced:

- HTTP requests
- GET requests
- Query parameters
- API responses
- JSON data
- Inspecting API output

This helped build an understanding of how n8n communicates with external services.

---

## 6. Webhook-Based Workflow

Built a webhook workflow that receives incoming HTTP POST requests and processes JSON data.

Workflow:

```text
Webhook (POST)
      ↓
IF body.name exists
     /          \
   TRUE         FALSE
    ↓              ↓
Success        Missing Name
Response         Response
```

The workflow was tested using PowerShell HTTP requests.

Example request:

```powershell
Invoke-RestMethod -Uri "http://localhost:5678/webhook-test/name-checker" -Method POST -ContentType "application/json" -Body '{"name":"Nagendra"}'
```

Successful response:

```json
{
  "message": "Success"
}
```

Missing-name test:

```powershell
Invoke-RestMethod -Uri "http://localhost:5678/webhook-test/name-checker" -Method POST -ContentType "application/json" -Body '{}'
```

Response:

```json
{
  "message": "Missing name"
}
```

This exercise introduced the fundamentals of **event-driven automation using webhooks**.

---

# 🧠 AI Integration with Groq

## 7. Connecting Groq API to n8n

Connected the **Groq API** to n8n using API authentication.

The first AI integration was built manually using the HTTP Request node.

Workflow:

```text
Manual Trigger
      ↓
HTTP Request
      ↓
Groq API
      ↓
AI Response
```

The workflow used the model:

```text
llama-3.3-70b-versatile
```

Example API request structure:

```json
{
  "model": "llama-3.3-70b-versatile",
  "messages": [
    {
      "role": "user",
      "content": "Write a one-line WhatsApp reply for a customer asking about salon hours."
    }
  ]
}
```

---

## 8. Extracting AI Responses

Learned how API responses contain nested JSON structures.

The generated AI response was located at:

```text
choices[0].message.content
```

Using an n8n expression:

```text
{{ $json.choices[0].message.content }}
```

I extracted the generated response and stored it as a clean field using an **Edit Fields (Set)** node.

Workflow:

```text
Manual Trigger
      ↓
HTTP Request
      ↓
Groq API
      ↓
Edit Fields
      ↓
Clean AI Reply
```

This demonstrated how to transform complex API responses into usable workflow data.

---

# 🤖 Dedicated n8n AI Nodes

## 9. Basic LLM Chain + Groq Chat Model

After understanding how AI APIs work through raw HTTP requests, I rebuilt the AI workflow using n8n's dedicated AI components.

Workflow:

```text
Manual Trigger
      ↓
Basic LLM Chain
      │
      └── Groq Chat Model
```

This demonstrated how dedicated AI nodes simplify:

- API communication
- Model configuration
- Prompt handling
- Response processing

Compared with the raw HTTP Request approach, dedicated AI nodes reduce the amount of manual JSON configuration required.

---

# 📦 Structured AI Output

## 10. JSON-Formatted LLM Responses

Practiced instructing an LLM to generate structured JSON-formatted responses.

Example prompt requirement:

```json
{
  "reply": "...",
  "tone": "friendly"
}
```

Example AI response:

```json
{
  "reply": "Our salon is open from Monday to Saturday.",
  "tone": "friendly"
}
```
---

# 💇 Salon AI Assistant Mini-Project

## 11. Building a Business-Focused AI Assistant

Built a simple **AI-powered Salon Assistant** using n8n and the Groq Chat Model.

The objective of this mini-project was to simulate the core AI logic that could eventually power a WhatsApp customer support assistant for a local business.

The assistant receives a customer question, processes it using an LLM with predefined business information, and returns a clean customer-facing response.

### Workflow Architecture

```text
Manual Trigger
      ↓
Set Customer Question
(Edit Fields)
      ↓
Generate Salon Reply
(Basic LLM Chain)
      │
      └── Groq Chat Model
            ↓
Extract Clean Reply
(Edit Fields)
```

---

## 🧠 Business Knowledge

The AI assistant was given a limited set of business information:

```text
Business Type: Salon

Opening Hours:
Monday-Saturday: 10:00 AM-8:00 PM
Sunday: Closed
```

The system instruction was designed to ensure that the assistant answers customer questions using only the provided business information.

Example system instruction:

```text
You are a WhatsApp assistant for a salon.

The salon is open Monday through Saturday from 10:00 AM to 8:00 PM.
The salon is closed on Sundays.

Answer customer questions using ONLY the business information provided above.

Do not invent or assume any information that is not provided.

Keep your responses short, clear, friendly, and suitable for WhatsApp.
```

This introduced the concept of **grounding an LLM with business-specific information** and reducing hallucinations.

---

## 📨 Simulating Customer Questions

The **Edit Fields (Set)** node was used to simulate incoming customer messages.

A field called:

```text
customer_question
```

was created.

Example:

```text
What time do you open on Sunday?
```

This value was dynamically passed to the Basic LLM Chain using the n8n expression:

```text
{{ $json.customer_question }}
```

This demonstrated how data from one node can be dynamically passed into an AI workflow.

---

## 🤖 AI Response Generation

The customer question was processed using:

- Basic LLM Chain
- Groq Chat Model
- Llama 3.3 70B Versatile

The Basic LLM Chain dynamically received the customer question and combined it with the predefined business instructions.

Conceptually:

```text
Customer Question
        +
Business Information
        ↓
Basic LLM Chain
        ↓
Groq Chat Model
        ↓
AI-Generated Reply
```

---

## 🧹 Extracting the Clean AI Reply

The response generated by the Basic LLM Chain was passed to another **Edit Fields (Set)** node.

A new field was created:

```text
clean_reply
```

The generated AI response was extracted using:

```text
{{ $json.text }}
```

This produced a clean output that could eventually be sent back to a customer through a messaging platform such as WhatsApp.

Example final output:

```json
{
  "clean_reply": "We're closed on Sundays."
}
```

---

# 🧪 Testing the AI Assistant

The workflow was tested with multiple customer questions to verify that the model followed the provided business rules.

## Test 1 — Sunday Opening Hours

Customer:

```text
What time do you open on Sunday?
```

Expected behavior:

```text
The assistant should explain that the salon is closed on Sundays.
```

Result:

```text
Correct response generated.
```

✅ Passed

---

## Test 2 — Saturday Availability

Customer:

```text
Are you open at 5 PM on Saturday?
```

Business rule:

```text
Saturday: 10:00 AM-8:00 PM
```

Expected behavior:

```text
The assistant should confirm that the salon is open at 5 PM on Saturday.
```

Result:

```text
Correct response generated.
```

✅ Passed

---

## Test 3 — Sunday Appointment Request

Customer:

```text
Can I come at 11 AM on Sunday?
```

Expected behavior:

```text
The assistant should explain that the salon is closed on Sundays.
```

Result:

```text
Correct response generated.
```

✅ Passed

---

## Test 4 — Hallucination Test

The assistant was also tested with a question for which no information was provided.

Customer:

```text
How much does a haircut cost?
```

No pricing information was included in the system instructions.

The expected behavior was for the AI to avoid inventing a price and indicate that pricing information was unavailable.

This test helped demonstrate an important concept in production AI systems:

> An AI assistant should not invent business information when the required data is unavailable.

This introduced practical experience with **LLM grounding and hallucination control**.

---

# 💡 Key Concepts Learned

Through this mini-project, I practiced:

- Building multi-node AI workflows in n8n
- Passing dynamic data between nodes
- Using n8n expressions
- Creating system instructions for LLMs
- Connecting a Basic LLM Chain to Groq
- Using the Llama 3.3 70B Versatile model
- Extracting clean LLM responses
- Grounding an AI assistant with business information
- Testing AI responses against predefined business rules
- Understanding basic hallucination control
- Designing the AI logic layer of a business chatbot

---

# 🔮 Future Evolution

Currently, the workflow uses a manually entered customer question:

```text
Manual Trigger
      ↓
Hardcoded Customer Question
      ↓
AI Assistant
      ↓
Clean Reply
```

The long-term goal is to evolve this architecture into:

```text
Customer
      ↓
WhatsApp Message
      ↓
WhatsApp Cloud API
      ↓
Webhook
      ↓
n8n
      ↓
AI Assistant
      ↓
Business Knowledge
      ↓
AI-Generated Response
      ↓
WhatsApp Cloud API
      ↓
Customer
```

Future improvements may include:

- WhatsApp Cloud API integration
- Real-time webhook triggers
- Customer conversation history
- FAQ knowledge base
- Lead capture
- Lead qualification
- Appointment booking
- Google Calendar integration
- Customer data storage
- Human agent handoff
- Error handling
- Logging and monitoring
- Production deployment

This mini-project represents the first step toward building a complete **AI-powered WhatsApp automation system for local businesses**.

An important observation from this exercise was that an LLM can generate text that **looks like JSON**, while n8n may still treat the entire response as a text string.

This introduced the distinction between:

```text
JSON-formatted text
```

and:

```text
Actual structured JSON data
```

Future workflows will explore parsing and validating structured LLM responses.

---

# 🧩 Workflows Built

| # | Workflow | Concepts |
|---|---|---|
| 1 | Manual Trigger Workflow | Workflow execution |
| 2 | Set/Edit Fields Workflow | Data creation and manipulation |
| 3 | City IF Branch | Conditional logic |
| 4 | Public API Request | HTTP requests and query parameters |
| 5 | Webhook Name Checker | Webhooks and conditional responses |
| 6 | Raw Groq AI API Call | AI API integration |
| 7 | AI Response Extraction | Nested JSON and n8n expressions |
| 8 | Groq Chat Model Workflow | n8n AI nodes and LLM chains |
| 9 | Structured Output Experiment | JSON-formatted LLM output |

---

# 🧰 Technologies & Tools

| Technology | Purpose |
|---|---|
| n8n | Workflow automation |
| Docker Desktop | Running n8n locally |
| WSL2 | Linux environment on Windows |
| Groq API | LLM inference |
| Llama 3.3 | Language model |
| REST APIs | External service integration |
| Webhooks | Event-driven workflow triggers |
| JSON | API data exchange |
| PowerShell | API and webhook testing |
| Git | Version control |
| GitHub | Project documentation and source control |

---

# 🚀 Upcoming Projects

The next phase of this learning journey will focus on building practical AI automation systems.

### Planned Project: Salon WhatsApp Assistant

```text
Customer Question
      ↓
Workflow Trigger
      ↓
AI Assistant
      ↓
Business Knowledge
      ↓
AI-Generated Response
      ↓
Clean Customer Reply
```

The assistant will answer customer questions based only on predefined business information.

Example business rules:

```text
Salon Hours
Monday-Saturday: 10 AM-8 PM
Sunday: Closed
```

Future improvements may include:

- WhatsApp integration
- Automated FAQ responses
- Lead capture
- Lead qualification
- Google Sheets integration
- Customer information storage
- Human handoff
- AI-powered lead routing

---

# 🎯 Long-Term Goal

The long-term objective of this learning journey is to develop the skills required to build **AI-powered automation microservices for small and local businesses**.

Potential applications include:

```text
WhatsApp Bots
      +
AI Customer Support
      +
Lead Capture
      +
Lead Qualification
      +
Automated Lead Routing
      +
Business Process Automation
```

The focus is on building practical, deployable automation systems that solve real business problems.

---

# 📈 Progress

```text
n8n Fundamentals             ✅
Docker + WSL2 Setup          ✅
Edit Fields / Set            ✅
Conditional Logic            ✅
HTTP Requests                ✅
REST API Integration         ✅
Webhooks                     ✅
Groq API Integration         ✅
LLM API Calls                ✅
AI Response Extraction       ✅
Groq Chat Model              ✅
Basic LLM Chain              ✅
JSON-Formatted AI Output     ✅

Salon AI Assistant           ✅ Completed
WhatsApp Integration         ⏳ Planned
Lead Capture System          ⏳ Planned
Lead Qualification           ⏳ Planned
Production Deployment        ⏳ Planned
```

---

# 🔐 Security

API keys and credentials should never be committed to GitHub.

Sensitive information such as:

```text
API keys
Access tokens
Authentication credentials
Environment variables
```

should be stored securely using n8n credentials or environment variables.

---

# 📖 What I Learned

Through these exercises, I developed a practical understanding of how automation workflows connect different systems.

The key learning progression was:

```text
Trigger
   ↓
Data
   ↓
Logic
   ↓
API
   ↓
Webhook
   ↓
AI Model
   ↓
Structured Response
   ↓
Automation
```

This repository will continue to evolve as I build increasingly advanced AI automation workflows and real-world projects.

---

## 👤 Author

**Nagendra Prasad**

B.Tech Artificial Intelligence & Machine Learning Student

Currently exploring:

- Artificial Intelligence
- Machine Learning
- AI Automation
- n8n
- APIs
- LLM Integration

---

⭐ This repository documents my continuous learning journey in AI-powered workflow automation.
