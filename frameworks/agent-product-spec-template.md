# Agent Product Spec — Template

*Clone this file. Fill in each section for your agent. The framework that explains why each section exists is [here](./ai-agent-product-spec.md).*

---

## Identity

- **Agent name:**
- **Version:** v1.0
- **PM (owner):**
- **Eval owner** *(single person, not a team):*
- **Date written:**
- **Next review date** *(set 90 days from today):*

---

## Section 1. Job to be Done

*The **user's** job, not the agent's task. One sentence with a specific outcome.*

> The user is trying to ______________ within ______________ without ______________.

**Success threshold for this job:**

> ______________

*(If you can't fill in Section 1 in a single sentence everyone in the room would write the same way, stop. The rest of the spec will rot.)*

---

## Section 2. Input/Output Contract

**Input the agent receives:**
-

**Output the agent produces (response + structured side effects):**
-

**Out of scope** *(inputs the agent should refuse or hand off):*
-

---

## Section 3. Decision Boundary

**Autonomous actions** *(no confirmation needed):*
-

**Confirm-then-act actions** *(user must confirm before action):*
-

**Hand-off triggers** *(conditions under which the agent defers to a human):*
-

**The handoff sentence — exact wording:**

> ______________

*(Don't leave this to the model. Write the words.)*

---

## Section 4. Failure Modes & Graceful Degradation

| Failure mode | Signal that this is happening | Fallback behavior | What the user sees |
|---|---|---|---|
| Tool call fails |  |  |  |
| Model uncertainty |  |  |  |
| User hostility |  |  |  |
| Out-of-scope request |  |  |  |
| Hallucinated policy |  |  |  |

*(Add rows for any agent-specific failure modes.)*

---

## Section 5. Evaluation Criteria

**Offline eval suite** *(fixed test cases, run before any prompt or model change ships):*

- Suite location:
- Number of test cases at launch:
- Owner *(single person, named above in Identity)*:

**Online eval / outcome metrics tied to Section 1:**

- Metric 1:
- Metric 2:
- Metric 3:

**Regression bar** *(minimum score a prompt change must maintain to ship):*

> ______________

---

## Section 6. Governance

| Activity | Owner | Reviewer | Cadence |
|---|---|---|---|
| Edit system prompt |  |  |  |
| Approve prompt changes |  |  |  |
| Swap model |  |  |  |
| Edit eval suite |  |  |  |
| Incident response |  |  |  |

**Logged events** *(what's logged, where, retention):*

-

---

## Section 7. Sunset Criteria

This agent will be retired if any of the following are true:

- [ ] *(metric ___ stays below ___ for ___ consecutive ___)*
- [ ] *(escalation rate climbs above ___)*
- [ ] *(a regulatory or business change makes the use case untenable)*
- [ ] *(the underlying user job in Section 1 no longer applies)*

*(If you cannot write a single condition here, write one anyway. "We will review the agent's value in 12 months and decide whether to keep it" is a sunset criterion.)*

---

## Change log

| Date | Section changed | What changed | Reason |
|---|---|---|---|
|  |  |  |  |

---

## Implementation Notes (Stack)

*This appendix documents the current implementation and is updated whenever the stack changes. It does **not** change the product spec above — but it must stay in sync with reality. The product spec is model-agnostic and platform-agnostic; the implementation is not.*

### Model & provider

- **Provider** *(OpenAI / Anthropic / Google / AWS Bedrock / Perplexity / Groq / Mistral / other):*
- **Model:**
- **Why this choice** *(cost / latency / capability — one line):*
- **Fallback model** *(if any):*
- **Migration cost** *(easy / medium / hard to swap providers):*

### Tools registered

| Tool | What it does | Failure-mode mapping (Section 4) |
|---|---|---|
|  |  |  |
|  |  |  |

### Memory design

- **Short-term** *(conversation history within session — keep N messages / full session):*
- **Long-term** *(cross-session — what gets persisted, by whom):*
- **Scoping** *(per user / per agent / per session):*
- **Retention policy** *(how long, who can purge):*

### Guardrails

- **PII detection** *(which types — emails, credit cards, names, addresses — and action: redact / block / flag):*
- **Toxicity threshold** *(numeric + action on breach):*
- **Content policy filters** *(domain-specific filters — legal, medical, regulated industries):*
- **Out-of-scope routing** *(what happens when asked something outside Section 1):*

### Observability

- **Events emitted** *(start / thinking / tool_call / llm_generation / completed / error):*
- **Trace store** *(where the events go — what system, what retention):*
- **Session correlation** *(how `session_id` is generated and propagated):*
- **Cost monitoring** *(token tracking, alert thresholds):*

### Cost budget

- **Token budget per run** *(target / hard cap):*
- **Monthly cost ceiling** *(target / alert threshold):*

### Knowledge layer *(if applicable)*

- **RAG / semantic search corpus** *(what documents, how indexed):*
- **Knowledge graph** *(entities and relationships, if used):*
- **Refresh policy** *(how often the knowledge layer is updated):*

---

*This template is the structural form of the [Agent Product Spec framework](./ai-agent-product-spec.md). Fill it in with the PM, the engineering lead, and the named eval owner in the same room. Lock the answers. Put the next review on the calendar before anyone leaves.*
