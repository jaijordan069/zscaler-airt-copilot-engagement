# Appendix B — Connection Type Quick Reference

Use this table to quickly identify the correct Connection Type setting and key configuration fields for each Microsoft Copilot scenario.

| Copilot Scenario | Connection Type in Platform | Key Configuration Fields |
|---|---|---|
| Copilot Studio Agent (public or via Broker) | Platform → Copilot Studio | Agent Secret + Direct Line Region (Non-Auth); OR Client ID + Tenant ID + Environment ID + Schema Name (Auth Mode) |
| Azure OpenAI Chat Endpoint (public) | API → OpenAI REST API | Endpoint URL, API Key, deployment name |
| Azure OpenAI Chat Endpoint (private, via Broker) | API → OpenAI REST API + Broker headers | URL set to Broker gateway path; X-Target-Url header; X-Broker-Id header; api-key header |
| Custom REST API / RAG Orchestration (via Broker) | API → REST API + Broker headers | URL set to Broker gateway path; X-Target-Url header; X-Broker-Id header; custom auth header; request body template |

For full configuration details, see [Section 2.3](../02_platform_setup_and_target_configuration.md#23-target-connection-configuration-for-microsoft-copilot-scenarios).
