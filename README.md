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
---

# 🧠 AI Agent with Memory and Tool Calling

## 12. Building a Context-Aware AI Agent

After building the basic Salon AI Assistant using a Basic LLM Chain, I extended my n8n learning by working with an **AI Agent** capable of maintaining conversation context and interacting with external tools.

This exercise introduced three important components of agentic AI workflows:

- AI Agent
- Conversation Memory
- Tool Calling

Unlike a basic LLM workflow that follows a direct input-to-output pattern, an AI Agent can decide whether to answer directly using its available context or invoke an external tool to complete an action.

---

## 🏗️ Workflow Architecture

The workflow was built using:

```text
Chat Trigger
     ↓
 AI Agent
   /  |  \
  /   |   \
 ↓    ↓    ↓
Groq  Simple  HTTP Request
Chat  Memory     Tool
Model
```

### Components

| Component | Purpose |
|---|---|
| Chat Trigger | Provides a built-in interface for testing multi-turn conversations |
| AI Agent | Processes user requests and decides how to respond |
| Groq Chat Model | Provides the Large Language Model used by the agent |
| Simple Memory | Maintains conversation context within the current session |
| HTTP Request Tool | Allows the AI Agent to interact with an external API |

---

# 💬 Multi-Turn Conversation with Memory

The first objective was to understand how an AI Agent maintains context across multiple messages.

The AI Agent was configured as a salon assistant with the following business information:

```text
You are a WhatsApp assistant for a salon.

The salon is open Monday through Saturday from 10:00 AM to 8:00 PM.
The salon is closed on Sundays.

Keep responses short, clear, and friendly.
```

The agent was connected to **Simple Memory**.

### Test Conversation

First message:

```text
What time do you open on Saturday?
```

The assistant correctly responded using the provided business information.

The next message was sent within the same conversation session:

```text
And Sunday?
```

The AI Agent understood that the second message referred to the previously discussed topic of opening hours.

Conceptually:

```text
Message 1
"What time do you open on Saturday?"
            ↓
      Simple Memory
            ↓
Conversation context retained
            ↓
Message 2
"And Sunday?"
            ↓
AI understands the context
            ↓
Question interpreted as:
"What are your opening hours on Sunday?"
```

This demonstrated how conversation memory enables an AI assistant to handle natural follow-up questions.

---

# 🧠 Understanding Session Memory

I also learned that memory must be associated with the correct conversation session.

Within the same session:

```text
Previous Messages
       +
Current Message
       ↓
Simple Memory
       ↓
Context-Aware Response
```

In a new session:

```text
New Conversation
       ↓
No Previous Conversation History
       ↓
Fresh Context
```

This is an important concept for future production chatbot systems.

Incorrect session management could cause:

- Conversation history to be lost
- Context from unrelated conversations to be mixed
- One customer's conversation to interfere with another customer's session

Proper session management will therefore be essential when integrating the AI Agent with real messaging platforms.

---

# 🛠️ Giving the AI Agent Tools

The next exercise introduced **Tool Calling**.

An **HTTP Request Tool** was connected to the AI Agent.

For testing purposes, the tool called a free public joke API.

The tool was configured with a clear description explaining when the AI Agent should use it.

Example tool description:

```text
Use this tool to fetch a random joke when the user asks for a joke.
Always use this tool when a joke is requested instead of creating a joke yourself.
```

The AI Agent system instructions were updated so that joke requests should use the available tool.

---

# 🔧 Understanding Tool Calling

When the user sends:

```text
Tell me a joke.
```

The workflow conceptually operates as:

```text
User Request
     ↓
AI Agent
     ↓
Analyzes User Intent
     ↓
Determines Tool Is Required
     ↓
HTTP Request Tool
     ↓
External API
     ↓
API Response
     ↓
AI Agent
     ↓
Final User-Friendly Response
```

The AI Agent successfully invoked the HTTP Request Tool before generating its final response.

This demonstrated an important difference between a normal chatbot and an AI Agent.

A normal LLM primarily generates text:

```text
User
  ↓
LLM
  ↓
Generated Response
```

An AI Agent can decide to perform actions:

```text
User
  ↓
AI Agent
  ↓
Decision
 /      \
↓        ↓
Answer   Use Tool
Directly    ↓
         External System
             ↓
           Result
             ↓
       Final Response
```

---

# 📝 Importance of Tool Descriptions

One important lesson from this exercise was that an AI Agent relies on the tool description to understand when and why a tool should be used.

A vague description such as:

```text
Gets data
```

does not provide enough information.

A better description clearly defines:

1. What the tool does
2. When the agent should use it

Example:

```text
Use this tool to fetch a random joke when the user asks for a joke.
```

Clear tool descriptions improve the reliability of agent tool selection.

---

# 💇 Salon AI Agent with Memory and Tool Calling

The concepts of AI Agents, Memory, and Tool Calling were then combined with the previous Salon AI Assistant project.

The resulting architecture represents a more advanced version of the original assistant.

```text
Customer Message
       ↓
Chat Trigger
       ↓
AI Agent
   /    |     \
  ↓     ↓      ↓
Groq  Simple  Availability
Model  Memory    Tool
```

The AI Agent was grounded with salon business information:

```text
Salon Hours:
Monday-Saturday: 10:00 AM-8:00 PM
Sunday: Closed
```

The agent could:

- Answer questions about salon opening hours
- Understand follow-up questions using conversation memory
- Maintain context during a conversation
- Recognize requests that require an external action
- Decide when to invoke an available tool

---

# 📅 Simulated Appointment Availability Tool

For learning purposes, an HTTP Request Tool was configured as a simulated appointment availability checker.

The tool description instructed the AI Agent to use it when a customer asked about appointment availability or attempted to book an appointment.

Example:

```text
Checks appointment slot availability.

Use this tool whenever a customer asks to book an appointment or asks whether an appointment slot is available.

This is a simulated tool for learning purposes.
```

> Note: The API used during this exercise does not provide real salon appointment availability. It was used only to understand and test the AI Agent's tool-calling behavior.

A real production system would replace this simulated tool with an actual appointment system, database, or calendar API.

---

# 🧪 Multi-Turn Agent Test

The combined workflow was tested using a multi-turn conversation.

### Message 1 — Business Information

```text
What time do you open on Saturday?
```

Expected behavior:

```text
AI Agent answers directly using its business knowledge.
```

---

### Message 2 — Contextual Follow-Up

```text
And Sunday?
```

Expected behavior:

```text
AI Agent uses conversation context and Simple Memory to understand that the customer is still asking about opening hours.
```

---

### Message 3 — Appointment Request

```text
I'd like to book an appointment for Saturday.
```

Expected behavior:

```text
AI Agent recognizes that the request requires checking appointment availability.
        ↓
AI Agent invokes the availability tool.
```

This demonstrated how a single AI Agent can combine:

```text
Business Knowledge
       +
Conversation Memory
       +
Intent Recognition
       +
Tool Calling
       ↓
Context-Aware AI Automation
```
---

# 🌐 Production-Style Webhook AI Assistant with Google Sheets Logging

## 13. Moving from Chat Trigger to Webhook

After building and testing the Salon AI Agent using n8n's Chat Trigger, I rebuilt the workflow using a **Webhook Trigger**.

The Chat Trigger is useful for testing AI conversations inside n8n, but real messaging platforms communicate with automation systems through APIs and webhooks.

This workflow represents a more production-oriented architecture that can later be connected to services such as WhatsApp Cloud API.

The new workflow can:

- Receive customer messages through an HTTP POST webhook
- Extract customer phone numbers and messages
- Validate incoming messages
- Process valid messages using an AI Agent
- Maintain separate conversation memory for each customer
- Return AI-generated responses through the webhook
- Log customer conversations in Google Sheets
- Detect and handle empty messages without calling the AI model

---

# 🏗️ Workflow Architecture

The completed workflow follows this architecture:

```text
Webhook (POST)
      ↓
Extract Customer Message
(Edit Fields)
      ↓
Validate Message
(IF)
   /       \
TRUE       FALSE
 ↓           ↓
AI Agent   Empty Message Reply
 /   \          ↓
↓     ↓     Respond to Webhook
Groq  Simple       ↓
Model Memory   Google Sheets Log
 ↓
Respond to Webhook
 ↓
Google Sheets Log
```

This workflow combines:

```text
Webhook APIs
      +
Input Validation
      +
AI Agent
      +
Conversation Memory
      +
Session Isolation
      +
Google Sheets Logging
```

---

# 📩 Receiving Customer Messages with a Webhook

The workflow begins with an n8n **Webhook** node configured to accept:

```text
POST
```

For local testing, requests were sent to the n8n test webhook endpoint.

A simulated incoming customer payload followed this structure:

```json
{
  "from": "919800000001",
  "message": "What time do you open Saturday?"
}
```

This structure represents a simplified version of the type of payload that could eventually be received from a messaging platform.

---

# 🧹 Extracting Customer Data

An **Edit Fields (Set)** node was used to extract the required information from the incoming webhook payload.

Two fields were created:

```text
phone
customer_message
```

The phone number was extracted using:

```text
{{ $json.body.from }}
```

The customer message was extracted using:

```text
{{ $json.body.message }}
```

The resulting data structure was:

```json
{
  "phone": "919800000001",
  "customer_message": "What time do you open Saturday?"
}
```

This step separates the required business data from the complete incoming webhook payload.

---

# ✅ Validating Incoming Messages

An **IF node** was added before the AI Agent.

The purpose of this node is to check whether:

```text
customer_message
```

is empty.

The workflow branches into two paths:

```text
Validate Message
      |
   ┌──┴──┐
   ↓     ↓
 TRUE   FALSE
```

### TRUE Branch

If a customer message exists:

```text
AI Agent
   ↓
Generate Response
   ↓
Respond to Webhook
   ↓
Log Conversation
```

### FALSE Branch

If the customer message is empty:

```text
Empty Message Reply
   ↓
Respond to Webhook
   ↓
Log Empty Message
```

This prevents unnecessary AI API calls when no valid message has been provided.

---

# 🤖 Processing Messages with the Salon AI Agent

Valid customer messages are passed to the **Salon AI Agent**.

The AI Agent uses:

- Groq Chat Model
- Llama 3.3 70B Versatile
- Simple Memory
- Business-specific system instructions

The incoming customer message is dynamically provided to the Agent.

Conceptually:

```text
Webhook Request
      ↓
Extract Message
      ↓
Validate Message
      ↓
AI Agent
      ↓
Groq Chat Model
      ↓
AI Response
```

The AI Agent is grounded with the salon's business information.

```text
The salon is open Monday through Saturday from 10:00 AM to 8:00 PM.

The salon is closed on Sundays.
```

This allows the assistant to answer questions such as:

```text
What time do you open Saturday?
```

with a response similar to:

```text
We're open from 10 AM to 8 PM on Saturday.
```

---

# 🧠 Phone-Number-Based Conversation Memory

One of the most important improvements in this workflow was configuring conversation memory using the customer's phone number.

Instead of using one shared memory session, each customer receives an independent conversation session.

Conceptually:

```text
Customer A
Phone: 919800000001
       ↓
Memory Session A


Customer B
Phone: 919800000002
       ↓
Memory Session B
```

The phone number extracted from the webhook is used as the session identifier for **Simple Memory**.

This ensures that:

- Each customer has separate conversation history
- Follow-up questions retain context
- Conversations from different customers do not mix
- Customer sessions remain isolated

---

# 🧪 Testing Conversation Memory

Session memory was tested using multiple webhook requests.

### Customer A — First Message

```text
My name is Nagendra.
```

The message was sent using:

```text
phone = 919800000001
```

### Customer A — Follow-Up

Using the same phone number:

```text
What is my name?
```

The AI Agent correctly remembered:

```text
Nagendra
```

This confirmed that memory persisted for the same session identifier.

---

# 🔒 Testing Session Isolation

A second simulated customer was created using:

```text
phone = 919800000002
```

The second customer asked:

```text
What is my name?
```

The AI Agent did not inherit the conversation history of Customer A.

This demonstrated proper session isolation.

Conceptually:

```text
919800000001
      ↓
Memory A
      ↓
Knows "Nagendra"


919800000002
      ↓
Memory B
      ↓
Separate Conversation
```

This is an important requirement for production chatbot systems where multiple customers may interact with the assistant simultaneously.

---

# 📤 Responding to Webhook Requests

After generating the AI response, the workflow uses the **Respond to Webhook** node.

The AI Agent's generated output is returned as a structured response.

Example:

```json
{
  "reply": "We're open from 10 AM to 8 PM on Saturday."
}
```

This architecture allows an external application to:

```text
Send HTTP Request
      ↓
n8n Webhook
      ↓
AI Processing
      ↓
HTTP Response
```

In the future, the external application can be replaced with a real messaging service such as WhatsApp Cloud API.

---

# 📊 Google Sheets Conversation Logging

Google Sheets was integrated into the workflow as a lightweight conversation logging system.

A spreadsheet named:

```text
Salon AI Assistant Logs
```

was created.

The logging structure contains the following columns:

```text
timestamp
phone
customer_message
bot_reply
note
```

Example:

| timestamp | phone | customer_message | bot_reply | note |
|---|---|---|---|---|
| Current timestamp | 919800000001 | What time do you open Saturday? | We're open from 10 AM to 8 PM on Saturday. | normal message |

Each successful customer interaction creates a new row in Google Sheets.

---

# 🔐 Google OAuth Integration

Because n8n is running locally using Docker, Google OAuth credentials were configured through Google Cloud.

The setup involved:

- Creating a Google Cloud project
- Enabling Google Sheets API
- Enabling Google Drive API
- Configuring the OAuth consent screen
- Creating OAuth 2.0 credentials
- Configuring the n8n OAuth redirect URI
- Adding a test user
- Connecting the Google account to n8n

This allowed the self-hosted n8n instance to securely interact with Google Sheets.

> OAuth Client Secrets and API credentials are never stored in this GitHub repository.

---

# 📝 Logging AI Conversations

For normal customer messages, the Google Sheets node appends:

```text
timestamp          → Current timestamp
phone              → Customer phone number
customer_message   → Incoming customer message
bot_reply          → AI-generated response
note               → normal message
```

This creates a simple conversation history that can later be used for:

- Monitoring chatbot interactions
- Debugging workflows
- Understanding customer questions
- Identifying common customer requests
- Basic analytics

---

# ⚠️ Empty Message Handling

The workflow also handles invalid requests where the customer message is empty.

Example payload:

```json
{
  "from": "919800000001",
  "message": ""
}
```

Instead of sending this request to the AI Agent, the IF node routes it through the FALSE branch.

```text
Empty Customer Message
       ↓
IF Validation
       ↓ FALSE
Empty Message Reply
       ↓
Respond to Webhook
```

The response is:

```json
{
  "reply": "Please send your question"
}
```

The AI Agent is completely skipped.

This avoids:

- Unnecessary AI API calls
- Wasted tokens
- Invalid LLM requests
- Unnecessary processing

---

# 📊 Logging Empty Messages

Empty messages are also logged in Google Sheets.

Example:

| timestamp | phone | customer_message | bot_reply | note |
|---|---|---|---|---|
| Current timestamp | 919800000001 | | Please send your question | empty message received |

The note:

```text
empty message received
```

makes it easy to distinguish invalid requests from normal customer conversations.

---

# 🧪 Workflow Testing

The completed workflow was tested with multiple scenarios.

## Test 1 — Normal Customer Message

Input:

```json
{
  "from": "919800000001",
  "message": "What time do you open Saturday?"
}
```

Expected behavior:

```text
Webhook
   ↓
Extract Message
   ↓
IF → TRUE
   ↓
AI Agent
   ↓
Respond to Webhook
   ↓
Google Sheets Log
```

Result:

```text
Passed ✅
```

---

## Test 2 — Conversation Memory

First message:

```text
My name is Nagendra.
```

Follow-up:

```text
What is my name?
```

Both requests used the same phone number.

Result:

```text
AI Agent remembered the conversation context.
```

```text
Passed ✅
```

---

## Test 3 — Session Isolation

A different phone number was used to start another conversation.

The second session did not inherit the first customer's conversation history.

Result:

```text
Customer conversations remained isolated.
```

```text
Passed ✅
```

---

## Test 4 — Google Sheets Logging

A normal webhook request was executed.

The following information was successfully added to Google Sheets:

```text
timestamp
phone
customer_message
bot_reply
note
```

Result:

```text
Passed ✅
```

---

## Test 5 — Empty Message Validation

Input:

```json
{
  "from": "919800000001",
  "message": ""
}
```

The workflow:

- Detected the empty message
- Skipped the AI Agent
- Returned "Please send your question"
- Logged the request in Google Sheets
- Tagged the log as "empty message received"

Result:

```text
Passed ✅
```

---

# 💡 Key Concepts Learned

Through this project, I gained practical experience with:

- Production-style webhook workflows
- HTTP POST requests
- JSON webhook payloads
- Dynamic field extraction
- n8n expressions
- IF-based input validation
- AI Agent integration
- Groq Chat Model integration
- Session-based conversation memory
- Phone-number-based session identifiers
- Customer conversation isolation
- Respond to Webhook nodes
- Structured JSON responses
- Google Sheets integration
- Google OAuth 2.0 configuration
- Google Sheets API
- Google Drive API
- Conversation logging
- Error handling
- Defensive workflow design
- Avoiding unnecessary LLM API calls

---

# 🚀 Project Evolution

The project has evolved through several stages:

### Stage 1 — Raw AI API Call

```text
Manual Trigger
      ↓
HTTP Request
      ↓
Groq API
      ↓
AI Response
```

### Stage 2 — Basic Salon AI Assistant

```text
Manual Trigger
      ↓
Customer Question
      ↓
Basic LLM Chain
      ↓
Groq Chat Model
      ↓
Clean Reply
```

### Stage 3 — AI Agent with Memory and Tools

```text
Chat Trigger
      ↓
AI Agent
   /    |    \
Groq  Memory  Tools
```

### Stage 4 — Production-Style Webhook AI Assistant

```text
Webhook
   ↓
Extract Data
   ↓
Validate Input
   ↓
AI Agent
   ↓
Phone-Based Memory
   ↓
Respond to Webhook
   ↓
Google Sheets Logging
```

The project is gradually moving toward a complete real-world architecture:

```text
Customer
   ↓
WhatsApp
   ↓
WhatsApp Cloud API
   ↓
Webhook
   ↓
n8n
   ↓
Input Validation
   ↓
AI Agent
   ├── Business Knowledge
   ├── Customer Memory
   └── Business Tools
   ↓
Response
   ↓
WhatsApp Cloud API
   ↓
Customer

      +
      
Google Sheets / Database
Conversation Logging
```

---

# 🔮 Future Improvements

Future development will focus on:

- WhatsApp Cloud API integration
- Real incoming WhatsApp messages
- Sending AI replies through WhatsApp
- Persistent production-grade memory
- Real appointment availability checking
- Google Calendar integration
- Automated appointment booking
- Customer database integration
- Lead capture
- Lead qualification
- Human agent handoff
- Better error handling
- Retry mechanisms
- Production deployment
- Monitoring and analytics

This milestone represents the transition from a simple AI chatbot experiment toward a more structured **AI-powered business automation system** built with n8n.


---

# 💡 Key Concepts Learned

Through this exercise, I gained practical experience with:

- n8n AI Agent workflows
- Chat Trigger
- Groq Chat Model integration
- Simple Memory
- Multi-turn conversations
- Conversation context
- Session-based memory
- AI Agent decision-making
- HTTP Request Tools
- Tool descriptions
- External API integration
- Agentic tool calling
- Intent-based tool selection
- Simulated appointment availability workflows

---

# 🚀 Evolution of the Project

The project has progressed through multiple stages.

### Stage 1 — Basic API Integration

```text
Manual Trigger
      ↓
HTTP Request
      ↓
Groq API
      ↓
AI Response
```

### Stage 2 — Basic AI Assistant

```text
Manual Trigger
      ↓
Customer Question
      ↓
Basic LLM Chain
      ↓
Groq Chat Model
      ↓
Clean Reply
```

### Stage 3 — Context-Aware AI Agent

```text
Chat Trigger
      ↓
AI Agent
   /    |    \
Groq  Memory  Tools
```

The next stages will focus on replacing simulated components with real business integrations.

Potential future improvements include:

- WhatsApp Cloud API integration
- Real appointment availability checking
- Google Calendar integration
- Appointment booking
- Customer-specific session management
- Customer database integration
- Lead capture
- Lead qualification
- Human agent handoff
- Persistent conversation memory
- Production deployment

This represents another step toward building a complete **AI-powered automation system for local and small businesses**.

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

# 📈 Learning Progress

### 🟢 Core n8n & Automation

```text
n8n Fundamentals                  ✅
Docker + WSL2 Local Setup         ✅
Edit Fields / Data Transformation ✅
Conditional Logic (IF)            ✅
HTTP Requests                     ✅
REST API Integration              ✅
Webhook Workflows                 ✅
Input Validation                  ✅
```

### 🤖 AI & LLM Integration

```text
Groq API Integration              ✅
LLM API Calls                     ✅
AI Response Extraction            ✅
JSON-Formatted AI Output          ✅
Groq Chat Model                   ✅
Basic LLM Chain                   ✅
Salon AI Assistant                ✅
```

### 🧠 AI Agents & Memory

```text
AI Agent Fundamentals             ✅
Simple Memory                     ✅
Multi-Turn Conversations          ✅
Session-Based Memory              ✅
AI Agent Tool Calling             ✅
HTTP Request Tools                ✅
Salon AI Agent                    ✅
```

### 🌐 Production-Style Automation

```text
Webhook-Based AI Agent            ✅
Phone-Based Session Memory        ✅
Customer Session Isolation        ✅
Empty Message Validation          ✅
Structured Webhook Responses      ✅
```

### 📊 Google Integration & Logging

```text
Google OAuth 2.0 Integration      ✅
Google Sheets Integration         ✅
Conversation Logging              ✅
Invalid Request Logging           ✅
```

### 🚀 Next Steps

```text
WhatsApp Cloud API Integration    ⏳ Planned
Real Appointment Integration      ⏳ Planned
Google Calendar Integration       ⏳ Planned
Lead Capture System               ⏳ Planned
Lead Qualification                ⏳ Planned
Persistent Conversation Memory    ⏳ Planned
Human Agent Handoff               ⏳ Planned
Production Deployment             ⏳ Planned
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
