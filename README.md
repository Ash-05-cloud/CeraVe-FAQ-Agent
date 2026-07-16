# CeraVe-FAQ-Agent
RAG-powered CeraVe FAQ customer support agent built with n8n, OpenRouter, Hugging Face embeddings, Airtable, and webhook automation.
# CeraVe FAQ Customer Support Agent

An AI-powered FAQ customer support workflow built with **n8n** and **Retrieval-Augmented Generation (RAG)**. The agent uses a CeraVe FAQ PDF as its knowledge base, retrieves relevant information through semantic search, generates grounded answers, stores structured results in Airtable, and returns the response through a webhook.

> **Disclaimer:** This is an independent portfolio project created for educational purposes. It is not affiliated with, endorsed by, or officially connected to CeraVe or L'Oréal.

---

## Project Overview

Customer support teams often answer the same product and brand questions repeatedly. This project demonstrates how an AI agent can use a company FAQ document to provide faster and more consistent responses while reducing unsupported or hallucinated answers.

The workflow has two main parts:

1. **Knowledge Base Ingestion**
   - Downloads the FAQ PDF from Google Drive
   - Extracts the document text
   - Converts the text into vector embeddings
   - Stores the embeddings in a vector store

2. **Customer Question Handling**
   - Receives a customer question through a webhook
   - Cleans and validates the input
   - Searches the FAQ knowledge base
   - Generates a grounded answer using an OpenRouter chat model
   - Converts the result into structured data
   - Saves the interaction in Airtable
   - Returns the response through the webhook

---

## Workflow Architecture

```text
                         KNOWLEDGE BASE INGESTION

Manual Trigger
      |
Google Drive: Download FAQ PDF
      |
Default Data Loader
      |
Hugging Face Embeddings
      |
Simple Vector Store


                         CUSTOMER QUESTION FLOW

Webhook
   |
Clean Input
   |
AI Agent <---- OpenRouter Chat Model
   |
   +---------- CeraVe FAQ Knowledge Base Tool
                  |
             Vector Search
                  |
         Hugging Face Embeddings
   |
Parse AI Output
   |
Airtable: Create Record
   |
Respond to Webhook
```

---

## Key Features

- FAQ-based AI customer support
- Retrieval-Augmented Generation
- Semantic search using vector embeddings
- Grounded responses based on uploaded documents
- Webhook-based API input and output
- Structured response parsing
- Airtable interaction logging
- Unsupported-question testing
- Modular n8n workflow design

---

## Tech Stack

| Technology | Purpose |
|---|---|
| n8n | Workflow automation and orchestration |
| OpenRouter | Access to the chat language model |
| Hugging Face Embeddings | Converts document text and queries into vectors |
| Simple Vector Store | Stores and retrieves FAQ embeddings |
| Google Drive | Stores and provides the FAQ PDF |
| Airtable | Stores customer questions and AI responses |
| Webhook | Receives questions and returns responses |
| JavaScript | Cleans input and parses AI output |
| RAG | Grounds answers in the FAQ knowledge base |

---

## Repository Structure

```text
cerave-faq-customer-support-agent/
│
├── README.md
├── .gitignore
│
├── workflow/
│   └── cerave-faq-agent-workflow.json
│
├── assets/
│   ├── screenshots/
│   │   ├── complete-workflow.png
│   │   ├── knowledge-base-section.png
│   │   └── customer-query-section.png
│   │
│   └── demo/
│       └── cerave-faq-agent-demo.mp4
│
├── docs/
│   ├── setup-guide.md
│   └── architecture.md
│
└── examples/
    ├── test-questions.md
    └── sample-request.json
```

---

## How It Works

### 1. Document Ingestion

The workflow downloads the FAQ PDF from Google Drive. The Default Data Loader extracts the text and prepares it for embedding.

### 2. Embedding Generation

The Hugging Face embedding model converts the FAQ content into numerical vectors that represent the semantic meaning of the text.

### 3. Vector Storage

The generated vectors are stored in the n8n Simple Vector Store. This allows the workflow to search by meaning instead of relying only on exact keyword matches.

### 4. Customer Query

A customer sends a question to the webhook in JSON format.

```json
{
  "message": "What are ceramides?"
}
```

### 5. Retrieval and Answer Generation

The AI agent searches the vector store for the most relevant FAQ content. It then uses the retrieved context to generate an answer through the OpenRouter chat model.

### 6. Response Storage

The generated response is parsed into a structured format and stored in Airtable for tracking and review.

### 7. Webhook Response

The final answer is returned to the customer through the Respond to Webhook node.

---

## Example Questions

Questions supported by the FAQ knowledge base may include:

- What are ceramides?
- What is MVE technology?
- Are CeraVe products tested on animals?
- What is the difference between fragrance-free and unscented?
- How can I identify an authentic CeraVe product?
- Where can I purchase CeraVe products?
- Are CeraVe products non-comedogenic?
- What makes CeraVe different from other skincare brands?

An unsupported test question can also be used:

```text
Which CeraVe shampoo cures hair loss?
```

The expected behavior is for the agent to explain that the information is not available in the provided FAQ instead of inventing an answer.

---

## Sample API Request

### Method

```text
POST
```

### Request Body

```json
{
  "message": "What is MVE technology?"
}
```

### Example Response

```json
{
  "answer": "MVE technology is a delivery system designed to release moisturizing ingredients over time.",
  "category": "Ingredient / Technology",
  "sentiment": "Neutral",
  "status": "Answered"
}
```

> The exact response fields depend on the output structure configured in the workflow.

---

## Setup Instructions

### Prerequisites

Before importing the workflow, prepare accounts or credentials for:

- n8n
- Google Drive
- OpenRouter
- Hugging Face
- Airtable

### Step 1: Import the Workflow

1. Open n8n.
2. Click the three-dot menu.
3. Select **Import from File**.
4. Upload:

```text
workflow/cerave-faq-agent-workflow.json
```

### Step 2: Configure Credentials

Reconnect the following credentials inside n8n:

- Google Drive
- OpenRouter
- Hugging Face
- Airtable

Never upload API keys, access tokens, passwords, or webhook secrets to GitHub.

### Step 3: Configure the FAQ File

Update the Google Drive node and select your FAQ PDF.

### Step 4: Run Knowledge Base Ingestion

Execute the upper branch first so that the FAQ content is embedded and stored in the vector store.

### Step 5: Test the Webhook

1. Open the Webhook node.
2. Click **Listen for Test Event**.
3. Copy the test webhook URL.
4. Send a POST request from Postman.

```json
{
  "message": "What are ceramides?"
}
```

### Step 6: Activate the Workflow

After testing successfully:

1. Activate the n8n workflow.
2. Use the production webhook URL.
3. Send questions through your frontend, Postman, or another application.

---

## Testing Checklist

The workflow passes the test when:

- The webhook receives the question successfully
- The knowledge base returns relevant FAQ context
- Different questions receive different relevant answers
- The agent does not invent unsupported products or claims
- The Airtable record is created correctly
- The webhook returns a valid response
- The output does not contain `undefined`, empty values, or broken JSON

The workflow needs revision when:

- Every question receives the same response
- The agent answers without using the FAQ
- Unsupported information is invented
- The webhook keeps loading without returning a response
- Airtable fields are incorrectly mapped
- The vector store returns no relevant context


---

## Demo

Add the LinkedIn demo video here:

```text
assets/demo/cerave-faq-agent-demo.mp4
```

You can also add a preview image:

```markdown
![CeraVe FAQ Agent Workflow](assets/screenshots/complete-workflow.png)
```

---

## What I Learned

This project helped me gain practical experience with:

- Building AI agents in n8n
- RAG workflow architecture
- Document ingestion and chunking
- Hugging Face embeddings
- Vector search
- Prompt grounding
- Webhooks and REST-style requests
- Structured JSON parsing
- Airtable integration
- Testing AI systems for hallucinations and unsupported questions

---

## Author

**Ayesha Jalil**

AI Automation Engineer focused on n8n, AI agents, workflow automation, APIs, webhooks, LLMs, Hugging Face, RAG, and vector databases.

---

## License

This project is shared for educational and portfolio purposes. Brand names and trademarks belong to their respective owners.
