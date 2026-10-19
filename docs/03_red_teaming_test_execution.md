# Section 3 — Red Teaming Test Execution

---

## 3.1 Selecting Probe Packs for Copilot Scenarios

| Probe Category | What It Tests | Priority for Copilot Scenarios |
|---|---|---|
| Security | Resistance to adversarial inputs: prompt injection, jailbreaks, data exfiltration attempts, authorization bypass, safe handling of malicious instructions | HIGH for all Copilot scenarios; critical for RAG and M365 data access |
| Safety | Avoidance of harmful, abusive, or dangerous content: illegal content, hate speech, harassment, discriminatory outputs, user privacy protection | HIGH for customer-facing or employee-facing agents |
| Hallucination & Trustworthiness | Accurate, grounded, verifiable outputs: hallucinated facts, grounding to RAG sources, consistency across prompts | HIGH for RAG-based Copilots and any scenario involving factual claims |
| Business Alignment | Adherence to organizational policies: handling out-of-scope behavior, brand voice, escalation paths, unauthorized commitments, regulatory guardrails | HIGH for all; critical for regulated industries |
| Custom Probes | Customer-defined test cases targeting specific known risks or use-case behaviors | Use when the scenario has unique risks not covered by predefined packs |

For a typical Copilot Studio agent or RAG-based Copilot, enable all five categories for a comprehensive baseline run.

---

## 3.2 Step-by-Step Test Execution Flow

### Step 1 — Configure the Test Run

22. In the AI Security Portal, navigate to **AI Infrastructure → AI Red Teaming → Overview**.
23. Click **“New Test Run”**.
24. Select the **Target** (the Copilot application configured in Section 2).
25. Select the **probe packs** to include (see Section 3.1). For a first run, select all applicable predefined packs plus any custom probes.
26. Configure the **session settings**:
    - For Copilot Studio agents: the platform opens a new Conversation ID per probe session to maintain stateful context where needed.
    - For Azure OpenAI endpoints: each probe is typically stateless (a fresh `messages` array per call). If the Copilot application maintains session state via a session ID, configure the session ID field accordingly.

---

### Step 2 — Review Results and Identify Failure Patterns

**Failure pattern categories:**

- **Security Failures:** The Copilot revealed data from its context or RAG index it should not have; executed injected instructions; bypassed role constraints in response to adversarial prompts.  
  _Common causes:_ weak system prompt, no Prompt Shield, overly permissive RAG index.

- **Safety Failures:** The Copilot produced harmful, abusive, or policy-violating content in response to jailbreak probes.  
  _Common causes:_ Azure OpenAI content filters disabled or set to low, no Copilot Studio safety policy.

- **Hallucination / Trustworthiness Failures:** The Copilot made factual claims not supported by its RAG sources; cited non-existent documents; gave inconsistent answers to the same question.  
  _Common causes:_ temperature too high, no citation enforcement in system prompt.

- **RAG-Specific Injection Failures:** The Copilot executed instructions embedded in a retrieved document, treating document content as commands.  
  _Common causes:_ absence of anti-injection instructions in the system prompt, no input sanitization in the RAG pipeline.

- **Business Alignment Failures:** The Copilot gave legal, medical, or financial advice it was prohibited from giving; acted outside its defined persona; made unauthorized commitments.  
  _Common causes:_ insufficient role constraints in system prompt, no escalation path defined.

---

### Step 3 — Run Focused Follow-Up Probes

Based on the failure patterns identified, run targeted follow-up probe sets:

- **If security / data exfil failures were found:** run additional data-exfiltration and prompt-injection probes with varied encoding, indirect injection, and multi-turn attack techniques
- **If safety failures were found:** run jailbreak and policy-evasion probes (role-play framing, hypothetical framing, persona-switching attempts)
- **If hallucination failures were found:** run citation-validation and source-grounding probes
- **If business alignment failures were found:** run out-of-scope and unauthorized action probes specific to the Copilot’s defined role

To repeat a finished test run with the same probes: in the Test Runs table, click **Action → Repeat Test Run**.  
To cancel a running test: **Action → Cancel**.

---

### Step 4 — Capture and Classify Findings for Reporting

30. For each failed probe run, record: probe category, probe name, prompt used, Copilot response, failure type.
31. Classify each finding using the framework in Section 3.3.
32. Generate the Test Run Report: in the Single Test Run page, click **Action → Generate Report**. The report downloads as a PDF.
33. Use the platform’s **“Harden System Prompt”** feature (**Action → Harden System Prompt → enter the Original System Prompt → Start**) to get AI-assisted hardening suggestions from the platform.
34. Use the **“Generate New Policy”** feature (**Action → Generate New Policy → Select Detectors → Create**) to generate a policy draft based on the findings.

---

## 3.3 Findings Categorization Framework

| Finding Category | Root Cause Type | Remediation Owner | Remediation Action |
|---|---|---|---|
| System Prompt Weakness | The Copilot’s system prompt lacks explicit refusal rules, boundary definitions, or anti-injection language | Application team (prompt author) | Harden system prompt (see Section 4) |
| Guardrail Misconfiguration | Azure OpenAI content filters are disabled, set to low sensitivity, or Copilot Studio safety policies are not enforced | Application team (Azure / Copilot Studio config) | Enable/tighten Azure OpenAI content filters; configure Copilot Studio policies |
| Orchestration Logic Issue | The RAG pipeline, LangChain workflow, or session management introduces a vulnerability | Application team (developer/architect) | Fix pipeline design: sanitize retrieved content, enforce citation validation, isolate retrieval from instruction execution |
| Data Scope Issue | The Copilot has access to data sources that are too broad, misclassified, or include content it should not surface | Application team + data governance | Narrow RAG index scope, apply data classification filters, remove overly permissive data connectors |

---

## 3.4 Test Run Execution Checklist

See [`checklists/test_run_execution_checklist.md`](../checklists/test_run_execution_checklist.md).
