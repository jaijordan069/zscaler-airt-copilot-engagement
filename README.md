# Zscaler AI Red Teaming — Microsoft Copilot Engagement Guide

**Version:** 1.0  
**Classification:** Confidential — Zscaler Professional Services Internal  
**Audience:** Zscaler PS Consultants

---

## Description

Step-by-step guide for Zscaler PS consultants to design, deploy, execute, and remediate AI Red Teaming engagements against Microsoft Copilot–based applications.

---

## Scope

This guide covers:

- **Copilot Studio agents** (Bot Framework / Direct Line–style APIs, backed by Azure OpenAI / Azure AI Foundry)
- **Copilot for Microsoft 365 (M365)** with enterprise data access
- **Custom Copilot integrations** (Azure AI Foundry endpoints, LangChain-based RAG, or orchestration APIs)
- **Pre-production certification engagements** and post-production recurring testing

---

## Platform References

| Platform | Reference |
|---|---|
| Zscaler AI Red Teaming (formerly SPLX Red Teaming) | Primary red teaming platform |
| AI Security Portal | ai-console.zscaler.com |
| Zidentity | zslogin |
| AI Guard Proxy | proxy.zseclipse.net |

---

## Repository Structure

```
zscaler-airt-copilot-engagement/
├── README.md
├── CHANGELOG.md
├── .gitignore
├── docs/
│   ├── 00_introduction.md
│   ├── 01_pre_engagement_preparation.md
│   ├── 02_platform_setup_and_target_configuration.md
│   ├── 03_red_teaming_test_execution.md
│   ├── 04_hard_prompting_and_prompt_engineering.md
│   ├── 05_data_privacy_and_governance.md
│   └── appendices/
│       ├── appendix_a_engagement_intake_template.md
│       └── appendix_b_connection_type_quick_reference.md
├── checklists/
│   ├── pre_engagement_discovery_checklist.md
│   ├── test_run_execution_checklist.md
│   ├── prompt_hardening_playbook_checklist.md
│   └── governance_checklist.md
├── templates/
│   ├── engagement_intake_template.md
│   └── docker-compose-broker.yml
└── prompt-patterns/
    ├── system_prompt_design_patterns.md
    └── anti_jailbreak_patterns.md
```

### docs/
Phased engagement documentation from pre-engagement through governance.

### checklists/
Actionable checklists for each engagement phase.

### templates/
Reusable templates: engagement intake form and Broker Docker Compose file.

### prompt-patterns/
System prompt design patterns and anti-jailbreak patterns for Copilot hardening.

---

> **⚠ NOTICE:** This is an internal Zscaler Professional Services document. Do not distribute outside of authorized Zscaler PS personnel.
