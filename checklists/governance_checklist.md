# Governance Checklist

Use this checklist to ensure proper governance processes are in place before and after a Zscaler AI Red Teaming engagement.

Refer to [Section 5](../docs/05_data_privacy_and_governance.md) for full governance role definitions and change management guidance.

---

## Pre-Engagement

- [ ] Governance roles identified and documented (see Section 5.4)
- [ ] Data residency requirements confirmed with Zscaler account team
- [ ] Retention period agreed and documented in engagement intake
- [ ] Data minimization requirements documented
- [ ] Scope and authorization letter / rules of engagement signed
- [ ] Test environment vs. production environment boundaries confirmed
- [ ] Customer’s application developer and security team available for the engagement

---

## Post-Engagement (Pre-Production Gate)

- [ ] All critical and high findings remediated or formally risk-accepted
- [ ] Risk acceptance documented and approved by CISO / AI Risk Officer
- [ ] Hardened system prompt version-controlled and stored in the application repository
- [ ] Azure OpenAI content filter and Prompt Shield configuration documented
- [ ] Production launch authorization issued by AppSec / AI Governance gate owner
- [ ] Post-production recurring test schedule configured in the platform
- [ ] Recurring test results assigned to SecEng / AI Security Operations owner

---

## Post-Engagement (Post-Production Recurring)

- [ ] New findings from recurring runs escalated within agreed SLA
- [ ] Change management process triggers re-runs for material Copilot changes (see Section 5.5)
- [ ] Periodic executive summary of recurring test results provided to CISO / AI Risk Officer
- [ ] Platform retention settings reviewed annually or upon regulatory change
