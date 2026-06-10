---
name: incident-postmortem
description: Write a blameless, actionable incident postmortem from the raw facts. Use after an outage/incident, or when asked to write up "what happened" for the team.
version: 1.0.0
license: MIT
keywords: [postmortem, incident, outage, sre, writing]
---

# incident-postmortem

A postmortem exists to change the system, not to assign blame or to perform
remorse. Optimize for the engineer two years from now who hits something
similar at 3am.

## Procedure

1. **Timeline first, from artifacts.** Build it from logs, alerts, deploy
   records and chat timestamps — not memory. Every entry: time (with zone),
   what happened, what was *known* at that moment. Mark detection, diagnosis,
   mitigation, resolution.
2. **Impact, quantified.** Who/what/how long/how many: "checkout failed for
   12 customers over 3h; all recovered" beats "some users were affected".
3. **Root cause as a causal chain**, not a single line. Use "because" links:
   *webhooks dropped → because the endpoint 500'd on duplicates → because a
   unique-constraint violation was treated as fatal → because the handler
   assumed Stripe never retries*. The last "because" is usually an assumption,
   and assumptions are what you fix.
4. **Blameless mechanically:** name systems and decisions, not people. "The
   deploy script allowed a config-less rollout" — not "X forgot the config".
   If a person *had* to be careful for the system to be safe, that's the
   finding.
5. **What went well / what got lucky.** Luck is a finding too ("we noticed
   only because someone happened to be watching dashboards").
6. **Action items that would have prevented or shortened THIS incident**, each
   with an owner and a date. Test each one: "would this have helped on the
   timeline above?" If not, cut it. 3–5 strong items beat 15 aspirational ones.

## Skeleton

```markdown
# 2026-04-02 — Payments webhook outage (3h, 12 failed checkouts)

**Summary** (3 sentences: what broke, impact, fix)
**Timeline** (timestamped, detection → resolution)
**Root cause** (causal chain, ending at the broken assumption)
**What went well / what got lucky**
**Action items** (owner · due date · "would it have helped?")
```
