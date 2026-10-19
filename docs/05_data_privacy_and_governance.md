# Section 5 — Data Privacy and Governance

---

## 5.1 Data Residency

The Zscaler AI Red Teaming platform’s control plane (portal, configuration, reports) runs in Zscaler’s SaaS infrastructure. For EU customers, work with the Zscaler account team to confirm regional deployment. Probe traffic flows from Zscaler’s cloud to the Copilot endpoint. For private endpoints, a lightweight Broker Agent is deployed inside the customer’s network making outbound-only connections — no inbound connections to the customer’s network.

---

## 5.2 Retention and Data Minimization

- Retention is configurable at the tenant level — it is not hardcoded.
- Agree on the retention period with the customer before the first test run.

**Data minimization options:**

- Not storing full response transcripts where only pass/fail scoring is needed
- Anonymizing or redacting specific fields in stored probe results (e.g., removing actual document content from stored responses)
- Shortening retention periods for high-sensitivity engagements

Document the agreed retention configuration in the engagement intake template and confirm in writing with the customer before the first test run.

---

## 5.3 Talk Track — Explaining Data Handling to Customers

**On data residency:**

> “The Zscaler AI Red Teaming platform’s control plane — meaning the portal, configuration, and reports — runs in Zscaler’s SaaS infrastructure. For EU customers, we can work with the Zscaler account team to confirm regional deployment. The probe traffic itself flows from our cloud to your Copilot endpoint. For private endpoints, you deploy a lightweight Broker Agent inside your network. That agent opens an outbound-only connection — nothing comes inbound to your network. The test prompts and your Copilot’s responses travel through your internal network only between the Broker and your endpoint, which stays entirely within your perimeter.”

**On data visibility:**

> “Your test results, including the probe prompts and Copilot responses, are stored in your tenant on the Zscaler platform. Zscaler operates as a data processor under your instructions. The Zscaler Professional Services team working on your engagement can see the results, but your raw prompt/response data is not shared externally beyond your tenant unless you explicitly export it.”

**On retention and minimization:**

> “Retention is configurable at the tenant level — it is not hardcoded. Before we start, we will agree on a retention period that matches your requirements. If your security policy requires it, we can minimize what gets stored — for example, storing only pass/fail scores rather than full transcripts, or shortening the retention window after the engagement closes.”

---

## 5.4 Governance Roles and Processes

| Governance Area | Role | Responsibility |
|---|---|---|
| Pre-production Copilot red team gate | AppSec / AI Governance team | Approves the Copilot for production launch only after red team findings are remediated (or formally risk-accepted). Acts as the gate owner for the certification process. |
| Post-production testing | SecEng / AI Security Operations | Owns the recurring test schedule, monitors results, escalates new findings, and tracks remediation status. |
| Prompt and system message changes | Application team (Copilot author/owner) | Implements system prompt hardening; triggers a new red team run after material changes. |
| Guardrail configuration | Cloud / Azure platform team | Manages Azure OpenAI content filter settings, Prompt Shield configuration, Copilot Studio policies. |
| RAG data scope | Data governance / content owners | Manages which data sources are included in the RAG index; approves scope changes. |
| Red team findings triage | AppSec + Application team | Reviews each finding, assigns severity, determines fix vs. risk acceptance, sets remediation timeline. |
| Policy generation and AI Guard integration | SecEng / AI Security Operations | Uses the platform’s “Generate New Policy” feature to create detection policies; integrates with AI Guard for runtime enforcement. |
| Engagement sign-off | CISO / AI Risk Officer | Final approval for production launch after pre-production certification; periodic review of post-production recurring test results. |

---

## 5.5 Copilot Change Management

Any material change to a Copilot application should trigger a re-run of the relevant probe categories:

| Change Type | Re-Run Required |
|---|---|
| System prompt updated or replaced | Security + Safety + Business Alignment probes |
| Azure OpenAI model version upgraded | All probe categories (full baseline re-run) |
| New data source added to RAG index | Hallucination + Security (data exfil) + RAG-specific probes |
| Data classification of a source changed | Security (data exfil) probes; potentially full re-run |
| New plugin / Power Automate flow connected | Security + Business Alignment probes |
| Copilot Studio topic / skill added | Business Alignment + Safety probes |

---

## 5.6 Governance Checklist

See [`checklists/governance_checklist.md`](../checklists/governance_checklist.md).
