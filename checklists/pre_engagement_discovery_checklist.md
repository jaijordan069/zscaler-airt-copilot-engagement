# Pre-Engagement Discovery Checklist

Complete this checklist before beginning any Zscaler AI Red Teaming engagement against a Microsoft Copilot application.

---

## Licenses and Platform Access

- [ ] Confirm Zscaler AI Red Teaming subscription is in place (SKU: `ZS-AI-SCANNING-PRE` for platform; `ZS-AI-SCANNING-PROBE-UNIT-PRE` for probes)
- [ ] Confirm the AI Security Portal is provisioned and accessible as a tile in Zidentity (zslogin)
- [ ] Confirm ZIdentity readiness: RBAC roles are assigned for the engagement team (at minimum: AI Red Teaming Administrator)

---

## AI Application / Target Information

- [ ] Has the list of Copilot targets been identified and confirmed with the application team?
- [ ] Are API endpoint URLs available and documented?
- [ ] Is API documentation (request/response schema, auth method) available?
- [ ] Are the application developers identified and available for technical support during the engagement?
- [ ] Do we have enough information about the app’s purpose, persona, and RAG integration to design relevant probes?

---

## Network and Hosting

- [ ] Are the Copilot endpoints publicly accessible, or do they sit on a private network / VNet?
- [ ] If private: can the customer host a Broker Agent or Hybrid Worker on a Linux VM inside their network?
- [ ] If private: can the customer open outbound port 443 from the Broker/Worker VM to the Zscaler Broker Gateway and Zidentity OAuth URL?
- [ ] If public: can the customer allow-list Zscaler’s probe IP addresses in their API gateway or firewall?

---

## Data Classification

- [ ] What data sources does the Copilot access? (SharePoint sites, OneDrive, Azure AI Search indexes, databases, M365 mailboxes, custom connectors)
- [ ] What is the data classification of those sources? (Public / Internal / Confidential / Highly Sensitive)
- [ ] Are there any regulatory or compliance constraints on the data? (GDPR, financial data, health data, legal privileged content)
- [ ] Is the test environment isolated from production data, or will production data be reachable?
