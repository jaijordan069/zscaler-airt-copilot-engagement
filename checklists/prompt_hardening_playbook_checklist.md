# Prompt-Hardening Playbook Checklist

Use this checklist when reviewing and hardening a Copilot system prompt. Refer to [Section 4](../docs/04_hard_prompting_and_prompt_engineering.md) and [`prompt-patterns/`](../prompt-patterns/) for detailed patterns and examples.

---

## System Prompt Review

- [ ] Does the prompt include a clear, specific role definition?
- [ ] Does the prompt explicitly list forbidden actions (not just implied)?
- [ ] Does the prompt include a policy precedence statement?
- [ ] Does the prompt address anti-override / anti-jailbreak attempts explicitly?
- [ ] Does the prompt address role-play and persona-switch attacks?
- [ ] Does the prompt address hypothetical/fictional framing attacks?
- [ ] Does the prompt address developer/admin impersonation attacks?
- [ ] For RAG scenarios: does the prompt include content-vs-command separation?
- [ ] For RAG scenarios: does the prompt enforce citation and provenance requirements?
- [ ] For RAG scenarios: does the prompt include source trust hierarchy language?
- [ ] Does the prompt include a PII / confidential data protection rule?
- [ ] Does the prompt specify behavior when the user is out-of-scope?

---

## Technical Guardrails Review

- [ ] Azure OpenAI content filters enabled for all harm categories?
- [ ] Filter thresholds appropriate for the data sensitivity level?
- [ ] Prompt Shield (prompt injection detection) enabled in Azure AI Content Safety?
- [ ] Groundedness detection enabled for RAG scenarios?
- [ ] Copilot Studio safety policies configured?
- [ ] Agent authentication required (sign-in enforced) where appropriate?
- [ ] Temperature set to ≤ 0.2 for factual/policy Copilots?
- [ ] RAG index scoped to only the data the Copilot should access?
