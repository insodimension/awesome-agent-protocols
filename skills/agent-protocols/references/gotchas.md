# Gotchas — agent-protocols skill

Append-only LOG of one-off learnings from real runs. Each entry is 1-3 lines. The fix lives in the promoted reference — entries here are breadcrumbs.

**Format:**
```markdown
### YYYY-MM-DD — <one-line title>
Symptom → cause → fix. See [<promoted-ref>#<anchor>](<promoted-ref>#<anchor>) for the full pattern.
Context: <what you were doing>.
```

---

### 2026-06-02 — ACP acronym collision is the #1 source of confusion
Four unrelated protocols all use "ACP" — Zed (editor↔agent), IBM (merged into A2A), AGNTCY (remote invocation), OpenAI+Stripe (commerce). See [decision-tree.md#the-four-acps--disambiguation](decision-tree.md#the-four-acps--disambiguation) for the full table.
Context: initial research across 60+ protocols for awesome-agent-protocols repo.

### 2026-06-02 — x402 v2 renamed the payment header
x402 v1 used `PAYMENT-SIGNATURE`, v2 renamed to `X-PAYMENT`. Spec examples must use the v2 name. See repo `examples/x402-payment.md` for the correct wire format.
Context: parallel research agents caught the version-specific nuance.

### 2026-06-02 — A2A v1.0 removed the `kind` field from message parts
Draft A2A had a `kind` field on message parts; v1.0 removed it (breaking change). Don't reference `kind` in recommendations. See repo `examples/a2a-agent-card.md`.
Context: parallel research agents caught the version-specific nuance.

### 2026-06-02 — IBM ACP is merged into A2A, not a separate protocol anymore
IBM's Agent Communication Protocol was donated to Linux Foundation and merged into A2A in August 2025. Don't recommend IBM ACP as a standalone option. See [decision-tree.md#the-four-acps--disambiguation](decision-tree.md#the-four-acps--disambiguation).
Context: initial protocol research — confirmed via GitHub discussion and LF announcement.

---

## Append-only rules

1. **Promote BEFORE you append.** The full fix lives in the appropriate reference; the entry here is the breadcrumb.
2. **Use absolute dates** (the user's current date). Never relative.
3. **One entry per discrete learning.** If a single run surfaces 5 things, log 5 separate entries.
4. **Never edit existing entries.** This is an audit log of what the skill learned and when. Strikethrough or supersede with a new dated entry instead.
5. **User pushback is the highest-value gotcha source.** If the user corrected you on phrasing or methodology, that's a must-log moment.
