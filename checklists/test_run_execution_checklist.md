# Test Run Execution Checklist

Use this checklist when executing a red teaming test run against a Microsoft Copilot application.

Refer to [Section 3](../docs/03_red_teaming_test_execution.md) for full step-by-step instructions.

---

- [ ] Target is registered and shows “Connected” (or test execution validates connectivity for hybrid deployments)
- [ ] Probe packs selected match the Copilot’s risk profile (see Section 3.1)
- [ ] Session configuration verified (conversation ID handling for Copilot Studio; stateless vs. stateful for Azure OpenAI)
- [ ] Parallelism set appropriately (start conservative; increase after stable execution confirmed)
- [ ] Baseline run triggered and completed
- [ ] Results reviewed: failures classified by type (guardrail, hallucination, data exfil, alignment)
- [ ] Focused follow-up probes run for each failure category identified
- [ ] Test Run Report generated (PDF downloaded)
- [ ] “Harden System Prompt” feature used on any Copilot with system-prompt weaknesses
- [ ] “Generate New Policy” used to draft remediation policy for detected categories
- [ ] All findings entered into the engagement findings log
- [ ] Findings classified using the four-category framework (Section 3.3)
- [ ] Remediation owners identified for each finding category
