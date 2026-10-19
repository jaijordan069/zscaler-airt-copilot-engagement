# Section 2 — Platform Setup and Target Configuration

---

## 2.1 Architecture Overview

- The Broker Agent makes only **outbound** connections on port 443
- All test probe traffic flows bidirectionally through the WSS tunnel
- The Broker Gateway in the cloud is managed and scaled by Zscaler

---

## 2.2 Broker Deployment — Step-by-Step

### Prerequisites

| # | Requirement | Detail |
|---|---|---|
| 1 | AI Security tenant access | Admin rights required in the AI Security Portal |
| 2 | ZIdentity access | Ability to create API Clients in the Zidentity portal |
| 3 | Linux VM | In the customer’s network or Azure VNet, co-located with the Copilot endpoints |
| 4 | VM resources | ≥ 1 vCPU, 128–256 MiB RAM |
| 5 | Docker | Installed on the Linux VM |
| 6 | Docker Compose plugin | Installed (use `docker compose` not `docker-compose`) |
| 7 | Outbound port 443 | From the VM to the Broker Gateway host |
| 8 | Outbound port 443 | From the VM to the ZIdentity OAuth token URL host |
| 9 | Outbound HTTP/HTTPS (80/443) | From the VM to the internal Copilot target app(s) |
| 10 | DNS resolution | VM must resolve the Broker Gateway FQDN, ZIdentity FQDN, and internal target hostnames/private endpoints |

---

### Step 1 — Register the Broker in the AI Security Platform

1. Log in to the AI Security Portal at **ai-console.zscaler.com**.
2. Navigate to: **Administration → Red Teaming → Broker → Register Broker**.
3. Enter a name for the Broker (e.g., `"CopilotEngagement-Broker"`) and select a Business Unit.
4. Click **“Open IAM Admin”** — this redirects to the ZIdentity API Clients page.

---

### Step 2 — Configure ZIdentity OAuth Credentials

5. In the ZIdentity API Clients page, click to add a new API Client key.
6. Set **Status: Enabled**. Set **Validation Type: Secret**.
7. Click **“Add”** — the Client Secret is displayed. **COPY AND SAVE IT IMMEDIATELY. It is shown only once.**
8. Click **“Resources”** and under AISECURITY, assign the role: **AI Red Teaming Administrator** (or Administrator for full access).
9. Click **Save**.
10. Copy the **Client ID** shown in the API Clients list.
11. Return to the AI Red Teaming portal and click **“Create Broker”**.

> **⚠ WARNING:** The Client Secret is shown only once. If you miss it, you must create a new API Client key.

---

### Step 3 — Deploy the Broker Container

12. In the broker configuration page, go to **Commands / Instructions**.
13. Download or copy the Docker Compose configuration file provided by the platform.
14. Edit the Docker Compose file and update these fields:
    - `OAUTH_TOKEN_URL`: `<as per your tenant>` (update only if required; otherwise leave as-is)
    - `OAUTH_CLIENT_ID`: `<Client ID copied from ZIdentity portal>`
    - `OAUTH_CLIENT_SECRET`: `<Client Secret copied from ZIdentity portal>`
15. Transfer the Docker Compose file to the Linux VM (e.g., to `/opt/zscaler-airt-broker/`).
16. From that directory, run:
    ```bash
    docker compose up -d
    ```
17. Verify: the **Broker Connection** state in the AI Security Portal should change to **“Connected”**.
18. Review the **Audit Log** tab on the broker page to confirm the connection event (look for a `"Connected from <IP>"` log entry).

---

### Broker Troubleshooting

- Run: `sudo docker logs <container_ID>` — look for connection or authentication errors
- Confirm the Broker state shows **“Connected”** in the portal
- Confirm outbound 443 is open to the Broker Gateway host and the ZIdentity OAuth token URL host
- Confirm the Broker VM can reach the internal Copilot target app base URLs
- Verify DNS resolution is working on the broker VM for all required FQDNs

---

### Step 4 — Connect the AI App Target (for SaaS + Broker)

19. In the AI Security Portal, navigate to: **AI Infrastructure → AI Red Teaming → Overview → Connect Asset**.
20. Select **“AI App for Red Teaming”**.
21. Fill in the **AI App Details** form:
    - **AI App Name:** (descriptive name, e.g., `"CustomerService-CopilotAgent"`)
    - **Business Unit:** (select the appropriate BU for this engagement)
    - **Network Type:** Private
    - **Private Network Access Type:** Broker
    - **Broker:** (select the broker registered in Steps 1–3, e.g., `"CopilotEngagement-Broker"`)
    - **Connection Type:** see Section 2.3 below for Copilot-specific settings

> **NOTE:** For public endpoints (SaaS Direct), set **Network Type** to `"Public"` and skip the Broker fields. The platform will call the endpoint directly.

---

## 2.3 Target Connection Configuration for Microsoft Copilot Scenarios

### Option A — Copilot Studio Agent

- **Connection Type:** Platform → Copilot Studio

**Non-Auth Mode (Agent Secret):**  
Enter the Agent Secret (a Direct Line secret key obtained from the Copilot Studio publishing settings). Select the Direct Line Region from the dropdown (choose the region where the Copilot is deployed).

**Auth Mode (OAuth / Managed Identity):**  
Provide:
- Client ID
- Tenant ID
- Environment ID
- Schema Name

These correspond to the Microsoft Entra/Azure and Dataverse environment settings for the agent. Click **“Authorize”** to complete the OAuth handshake.

**Session handling:**  
Each test session creates a new Conversation ID. The platform manages stateful conversation turns within a session. The agent’s reply text is extracted from the activity response; guardrail/denial messages (e.g., `"I can't help with that"`) are captured and scored as policy enforcement signals.

---

### Option B — Azure OpenAI / Azure AI Foundry Chat Endpoint

- **Connection Type:** API → OpenAI REST API
- **URL:** Enter the full Azure OpenAI chat completions endpoint  
  (e.g., `https://<resource>.openai.azure.com/openai/deployments/<deployment>/chat/completions?api-version=2024-02-01`)
- **API Key:** Enter the Azure OpenAI API key
- **System Prompt:** (optional) Enter the system prompt if you want the platform to prepend it
- **Model:** Enter the deployment name

**For Broker-routed access (private Azure OpenAI endpoint or APIM gateway):**
- URL: `http://airt-broker-gateway.probe:80/proxy/openai/deployments/<deployment>/chat/completions?api-version=2024-02-01`
- Add required headers:
  - `X-Target-Url: https://<your-private-aoai-endpoint>.openai.azure.com`
  - `X-Broker-Id: <broker-id-provided-by-Zscaler>`
  - `api-key: <your-api-key>`

**Response extraction:**  
The platform extracts the reply from `choices[0].message.content`. Content filter results (safety signals from Azure OpenAI content filtering) are captured as guardrail markers in the test results.

---

### Option C — RAG-Style Copilot / Custom Orchestration API

- **Connection Type:** API → REST API
- **URL (via Broker):** `http://airt-broker-gateway.probe:80/proxy/<your-rag-api-path>`
- Add required headers:
  - `X-Target-Url: https://<internal-rag-api-hostname>`
  - `X-Broker-Id: <broker-id-provided-by-Zscaler>`
  - `Authorization: Bearer <token>` or `x-api-key: <key>`
- Configure the **request body template** to map the probe text into the correct input field  
  (e.g., `{ "question": "{{probe}}", "session_id": "{{session_id}}" }`)
- Configure the **response extraction path** to pull the answer text (e.g., `$.answer`) and optionally citations

> **NOTE:** If the RAG API requires multi-step request/response transformation (e.g., session open before the first query), a custom Proxy may be required. Prepare a proxy handoff package (endpoint URL, auth, request/response samples, session lifecycle) and escalate to the AIRT Platform/Engineering team for proxy creation.

---

## 2.4 Network Ports Reference Table

| Component | Direction | Protocol | Port | Destination | Purpose |
|---|---|---|---|---|---|
| Broker Agent | Outbound | WSS (HTTPS) | 443 | Broker Gateway host | Broker tunnel |
| Broker Agent | Outbound | HTTPS | 443 | ZIdentity OAuth token URL | Authentication |
| Broker Agent | Outbound | HTTP/HTTPS | 80/443 | Internal Copilot target app(s) | Reach private AI app |
| Hybrid Worker | Outbound | AMQP TLS | 5671 | RabbitMQ (Zscaler cloud) | Job queue |
| Hybrid Worker | Outbound | HTTPS | 443 | ECR (container registry) | Pull container images |
| Hybrid Worker | Outbound | HTTPS | 443 | S3 / Azure Blob storage | Result/object storage |
| Hybrid Worker | Outbound | HTTPS | 443 | Translator service | Probe translation |
| Hybrid Worker | Outbound | HTTP/HTTPS | 80/443 | Private AI app | Execute test probes |
