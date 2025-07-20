# 📘 FinBuddy Workflow: PMMY Loan Assistance via n8n

This document outlines the purpose, flow, and future enhancements of the FinBuddy workflow developed using n8n. It is designed to guide users through configuring and extending the flow.

---

## 🎯 Objective

To help small-town and rural Indian users apply for PMMY (Pradhan Mantri Mudra Yojana) loans through a multilingual chatbot. It collects basic user details, identifies loan eligibility (Shishu, Kishore, Tarun), and can generate a loan application summary.

---

## 🧩 Current Workflow Nodes

### 1. When Chat Message Received

* Type: `ChatTrigger`
* Role: Starts workflow when a user sends a message.

### 2. Decide Route

* Type: `Function`
* Role: Classifies message as "mudra" (loan) or general "content"

### 3. If

* Type: `IF`
* Role: Branches flow into:

  * Loan (→ Prepare Prompt1)
  * Financial Education (→ Prepare Prompt)

### 4. Prepare Prompt1 (Loan)

* Type: `Function`
* Role: Constructs a detailed multilingual system prompt for PMMY support

### 5. Prepare Prompt (Content)

* Type: `Function`
* Role: Builds content-related prompt for FinSME modules

### 6. AI Agent

* Type: `LangChain Agent`
* Role: Responds intelligently based on prompt, chat history, and tools

### 7. Google Gemini Chat Model

* Type: `LLM`
* Role: Language model used by the AI Agent

### 8. Calculator (Tool)

* Type: `LangChain Tool`
* Role: Provides math/calculation ability to the agent

### 9. Postgres Chat Memory

* Type: `Memory`
* Role: Stores/retrieves chat memory for session persistence using PostgreSQL

### 10. Respond to Chat

* Type: `Webhook Response`
* Role: Returns the AI response to user

---

## 📄 Suggested Next Node: HTML → PDF Generation

To generate a summary of the PMMY application, add the following:

### 11. Generate Loan PDF

* Type: `HTML to PDF`
* Input HTML template:

```html
<h2>Loan Application Summary</h2>
<p><strong>Name:</strong> {{$json.name}}</p>
<p><strong>Location:</strong> {{$json.location}}</p>
<p><strong>Business:</strong> {{$json.business_type}}</p>
<p><strong>Income:</strong> {{$json.income}}</p>
<p><strong>Loan Category:</strong> {{$json.loan_category}}</p>
<p><strong>Requested:</strong> {{$json.loan_amount_requested}}</p>
<p><strong>Mobile:</strong> {{$json.mobile}}</p>
<p><strong>Aadhaar:</strong> {{$json.aadhaar_number}}</p>
```

* Connect this node after AI Agent
* You can later use this output to attach to an email

---

## 📌 Algorithm Overview

1. Trigger when a chat message is received
2. Route it
