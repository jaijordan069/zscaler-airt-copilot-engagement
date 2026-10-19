# Section 1 — Pre-Engagement Preparation

---

## 1.1 Define the Copilot Use Case

Before any technical configuration, the consultant must clearly understand what the Copilot application does and what business problem it solves. This shapes the choice of probe packs, the risk classification, and the deployment model.

---

### Step 1 — Identify the Copilot Surface in Scope

**a) Copilot Studio Agent**  
A conversational AI agent built in Microsoft Copilot Studio, backed by Azure OpenAI or Azure AI Foundry. Exposes a Direct Line or Bot Framework–style API endpoint. May include knowledge sources (SharePoint, websites, custom connectors).

**b) Copilot for Microsoft 365**  
Copilot embedded in Teams, Outlook, Word, etc. Backed by Microsoft’s own model infrastructure with access to the user’s M365 tenant data (emails, documents, Teams chats). Testing typically focuses on data access scope and policy enforcement.

**c) Custom Copilot Integration**  
A customer-built Copilot-like assistant using Azure OpenAI or Azure AI Foundry chat completion endpoints, optionally combined with Azure AI Search (RAG), LangChain orchestration, or other APIs. These expose a custom REST API or OpenAI-compatible endpoint.

---

### Step 2 — Document the Business Purpose and Risk Context

Capture:

- What business problem the Copilot solves (HR assistant, customer support, legal research, financial advisor, etc.)
- Who the end users are (employees, customers, partners)
- What actions the Copilot can take (read-only Q&A vs. write/transact actions via plugins or Power Automate flows)
- What data the Copilot can access (see Section 1.3)
- Whether this is pre-production (certification gate) or post-production (recurring test)

---

### Step 3 — Identify the Testable Interface

Determine the testable interface based on the Copilot surface:

**For Copilot Studio agents:**
- Direct Line endpoint URL (e.g., `https://directline.botframework.com/v3/directline/conversations`)
- Direct Line secret or token-based auth
- Conversation ID management (stateful sessions require a new conversation ID per session)
- Request schema: POST with activity type `"message"`, `text` field for the user utterance
- Response schema: array of activities; extract the bot’s reply from the `text` or `speak` field of activity type `"message"`

**For Azure OpenAI / Azure AI Foundry chat endpoints:**
- Endpoint URL (e.g., `https://<resource>.openai.azure.com/openai/deployments/<deployment>/chat/completions?api-version=2024-02-01`)
- API key or Managed Identity / Entra ID token
- Request schema: JSON body with `messages` array (system + user roles), model/deployment name
- Response schema: JSON with `choices[0].message.content` for the reply; `content_filter_results` for safety signals

**For RAG-style / orchestration APIs:**
- Custom REST API endpoint URL
- Auth scheme (API key, OAuth 2.0, managed identity)
- Request payload schema (e.g., `{ "question": "...", "session_id": "..." }`)
- Response payload schema (e.g., `{ "answer": "...", "citations": [...], "relevance_scores": [...] }`)

---

### Step 4 — Decide the Deployment Model

| Condition | Recommended Deployment Model |
|---|---|
| Copilot endpoint is publicly accessible (internet-reachable IP/FQDN) and data sensitivity is low/internal | SaaS Direct — platform calls the endpoint directly over HTTPS:443 |
| Copilot endpoint is on a private network / VNet / behind a firewall, or data is confidential/highly sensitive | SaaS + Broker (Hybrid) — deploy a Broker Agent inside the customer’s network |
| Customer requires no data to leave their network perimeter | SaaS + Broker (Hybrid) or On-Prem (future option; check with Zscaler account team) |

> **Note:** Most enterprise Copilot scenarios will use **SaaS + Broker** because Azure OpenAI endpoints, Copilot Studio agents backed by internal knowledge, and RAG pipelines are typically private or involve sensitive data.

---

## 1.2 Pre-Engagement Discovery Checklist

See [`checklists/pre_engagement_discovery_checklist.md`](../checklists/pre_engagement_discovery_checklist.md).

---

## 1.3 Engagement Intake Template

Complete this template before the engagement begins. Store it in the engagement project folder.

See [`templates/engagement_intake_template.md`](../templates/engagement_intake_template.md).
