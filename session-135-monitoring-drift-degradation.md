# Session 135 — Monitoring Drift & Degradation: Catching Model Rot
**Act:** 11 — Evaluation, Observability & Production AI | **Date Completed:** 
**Previous:** Session 134 — A/B Testing AI Features | **Next:** Session 136 — Evals-Driven Development

---

## The Key Idea

You ship an AI feature that works beautifully. Six months later, users are quietly getting worse answers — but nobody filed a bug, no error appeared in the logs, the system is still running normally. The model has "rotted." This is the most insidious failure mode in production AI: silent degradation, where quality declines gradually and nobody notices until user trust has already eroded. Monitoring for drift and degradation means building systems that detect these invisible declines automatically — before your users become your QA team.

---

## Three Types of Drift You Must Know

"Drift" is the general word for when something about your system changes in a way that wasn't intended. In AI systems, it shows up in three distinct forms:

```
┌──────────────────────────────────────────────────────────────────────┐
│              THREE TYPES OF DRIFT                                    │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  1. DATA DRIFT (Input Drift)                                        │
│  Definition: The questions your users are asking have changed.      │
│  The model is the same, but it's being asked things it wasn't      │
│  originally designed or tested for.                                 │
│                                                                      │
│  Example: Your HR chatbot was designed for leave queries.          │
│  Six months later, employees are asking about the new hybrid        │
│  work policy introduced in March — a topic that wasn't in the      │
│  original training documents.                                       │
│  Result: The bot confidently answers questions about a policy it   │
│  doesn't have good information on.                                  │
│                                                                      │
│  2. CONCEPT DRIFT                                                   │
│  Definition: The "right answer" has changed, even though the       │
│  questions haven't.                                                 │
│                                                                      │
│  Example: The maternity leave policy changed from 26 weeks to      │
│  28 weeks in January. Your documents weren't updated. Now the      │
│  bot gives the old (wrong) answer to the same question it used     │
│  to answer correctly.                                               │
│  Result: Outputs that were right last month are wrong today.       │
│                                                                      │
│  3. MODEL DEGRADATION                                               │
│  Definition: The underlying model itself has changed (or you       │
│  changed to a different model), affecting quality.                 │
│                                                                      │
│  Example: OpenAI silently updates gpt-4o in the background.       │
│  Or you migrate from Claude 3.5 Sonnet to a cheaper model.        │
│  Result: The same prompts produce subtly different quality.        │
│          Hallucination rate may increase. Tone may shift.          │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

The reason these are dangerous is that none of them trigger an error. The system keeps running, keeps returning HTTP 200 responses, keeps appearing healthy on standard monitoring dashboards — while quietly delivering worse answers.

---

## Why Silent Degradation Is the Most Dangerous Failure Mode

Consider the analogy of a slow gas leak in a building. The alarm doesn't go off because the concentration isn't yet at the explosive threshold. People start getting headaches, feeling foggy, underperforming — but nobody connects it to the leak. By the time someone notices, the damage is done.

```
┌──────────────────────────────────────────────────────────────────────┐
│              THE SILENT DEGRADATION TIMELINE                        │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  REAL PATTERNS FROM PRODUCTION AI SYSTEMS:                          │
│                                                                      │
│  PATTERN 1: Model Provider Update (undocumented)                    │
│  Month 1: Quality score 4.2/5                                       │
│  Month 2: Provider silently updates underlying model weights        │
│  Month 3: Quality score 3.7/5 (no error, no alert)                 │
│  Month 4: User satisfaction drops 20%. Team investigates.          │
│  Month 5: Root cause found — model update changed reasoning style  │
│           Rollback or prompt adjustment restores quality            │
│           Two months of degraded user experience could have been   │
│           caught in week 1 with eval monitoring                     │
│                                                                      │
│  PATTERN 2: Concept Drift (policy changed, docs not updated)        │
│  January: Leave policy updated in HR manual                         │
│  February: Docs re-uploaded but wrong section wasn't updated        │
│  March–June: 500 employees receive outdated leave balance info     │
│  July: First employee disputes HR record. Investigation begins.    │
│                                                                      │
│  PATTERN 3: Distribution Shift (new user segment)                   │
│  Months 1–6: Chatbot serves mostly English-speaking employees      │
│  Month 7: Company acquires firm with primarily Hindi-speaking staff │
│  Month 8: Answer quality for new employees is much lower            │
│           (model tuned for English policy docs; code-switching     │
│           and Hindi queries not tested)                             │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## What to Monitor: The Core Metrics Dashboard

A production AI monitoring dashboard should track these indicators continuously:

```
┌──────────────────────────────────────────────────────────────────────┐
│              WHAT TO MONITOR IN PRODUCTION                          │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  OUTPUT QUALITY METRICS (most important)                            │
│  ✓ LLM-judge quality score (rolling 7-day average)                 │
│  ✓ Faithfulness score (anti-hallucination)                         │
│  ✓ User thumbs-up/down rate (proxy for satisfaction)               │
│  ✓ Follow-up query rate (did user have to ask again?)              │
│                                                                      │
│  INPUT DISTRIBUTION METRICS (catches data drift early)             │
│  ✓ Query topic distribution (cluster queries; watch for new topics) │
│  ✓ Query length distribution (unusual change = different user base) │
│  ✓ % of queries with no good retrieved context (low similarity)    │
│                                                                      │
│  SYSTEM HEALTH METRICS                                              │
│  ✓ Latency: p50, p95, p99 (most users vs worst-case users)         │
│  ✓ Token usage trends (sudden increase = prompt changes or loops)  │
│  ✓ Cost per query (rolling 7-day; alert on >30% increase)          │
│  ✓ Error rate (HTTP errors, parsing failures, tool call failures)  │
│                                                                      │
│  MODEL-SPECIFIC METRICS                                             │
│  ✓ Log the exact model version used per trace                      │
│  ✓ Alert if model version changes (provider-side update detected)  │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Monitoring Architecture: The Three Signals You Need

Think of drift monitoring like the instruments in a car: the speedometer (output quality), the fuel gauge (cost/tokens), and the engine warning light (error rate/model change). You need all three because each catches different problems:

**Signal 1: Scheduled eval runs**
Every night (or every week for lower-traffic systems), run your full golden test set through the live system and log the RAGAS and LLM-judge scores. Compare to the baseline. If any metric drops more than a threshold, fire an alert.

**Signal 2: Online eval on production traffic**
Sample 5–10% of real production queries in real time. Run the LLM-judge on those samples. Maintain a rolling 7-day average quality score. Alert on sustained decline.

**Signal 3: Input distribution monitoring**
Cluster user queries using embedding similarity. Monitor the distribution of query topics over time. If a new cluster appears (users suddenly asking about a new topic that wasn't in your original design), flag it for investigation. This is early warning for concept drift.

---

## Alerting Strategy: What to Alert On and How Urgently

Not all anomalies are equal. A useful alerting hierarchy:

| Signal | Threshold | Response |
|--------|-----------|----------|
| Quality score drops >15% vs 30-day baseline | Page-level alert | Investigate immediately; consider feature rollback |
| Quality score drops 5–15% for 3+ consecutive days | Slack alert to team | Review recent changes; check model versions; audit traces |
| New query cluster detected (>5% of traffic, not previously seen) | Weekly digest | Review cluster; update retrieval docs; test quality on new topics |
| Cost per query increases >30% | Slack alert | Check for prompt loops, token count spikes, traffic anomalies |
| Model version change detected | Immediate notification | Run full eval suite; do not wait for quality drop to materialise |

---

## When to Retrain, Retune, or Switch Models

Drift monitoring tells you there's a problem. But what do you do about it? The decision tree:

```
┌──────────────────────────────────────────────────────────────────────┐
│              WHAT TO DO WHEN YOU DETECT DEGRADATION                 │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  Is the degradation caused by outdated documents (concept drift)?   │
│  → YES: Update your document store. Re-ingest new/updated docs.    │
│         Re-run RAGAS. Quality should recover without model changes. │
│                                                                      │
│  Is the degradation caused by new query types (data drift)?         │
│  → YES: Add new test cases covering the new query types.           │
│         Add new documents covering the new topics.                  │
│         Possibly refine chunking strategy for new content format.  │
│                                                                      │
│  Is the degradation caused by a model version change?               │
│  → YES: Test the current model version against your eval suite.    │
│         If it's worse: contact provider, pin to previous version   │
│         if available, or adjust prompts to compensate.             │
│                                                                      │
│  Is the degradation in all dimensions simultaneously?               │
│  → Likely a fundamental model issue. Consider switching models.    │
│         Run an offline A/B test: current model vs alternative.     │
│         Switch if alternative scores better on your eval suite.    │
│                                                                      │
│  WHEN NOT TO RETRAIN:                                               │
│  If you're using a frontier API model (GPT-4o, Claude), you don't  │
│  have a model to retrain — the provider controls the model.        │
│  Your levers are: prompt changes, document updates, model version  │
│  pinning, or switching providers.                                   │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Real Examples of Drift in Production

**Medical imaging AI (classic concept drift):** A skin lesion classifier trained on data from a specific hospital population began degrading as the hospital's patient demographics shifted over three years. Same model, same infrastructure — but the distribution of lesion types it encountered gradually diverged from its training distribution. Quality dropped 8% over 18 months. No alert was triggered because there was no monitoring. Caught only during an annual audit. This is the textbook argument for continuous monitoring.

**Customer service chatbot (provider model update):** A major retailer's customer service bot suddenly started giving longer, more verbose answers and had measurably lower task completion rates. Investigation: the underlying model had been silently updated by the provider. The new model version was more verbose and worse at following the concise-answer instructions in the system prompt. The retailer had no model version logging — they only discovered the update because a developer checked the API changelog after users complained.

**GitHub Copilot (quality variance across model versions):** GitHub has publicly acknowledged that code suggestion quality varies across model updates. This is why they now run continuous automated evaluation on a test suite of coding tasks before adopting new model versions. They've invested heavily in eval infrastructure specifically to manage provider-side model drift.

---

## What This Means for ECHO India

ECHO India faces two specific drift risks:

**Risk 1: Policy concept drift**
ECHO India's HR policies change every year (salary cycles, leave policy revisions, PF regulations). Every time a policy changes, if the RAG document store isn't updated, the chatbot gives wrong answers. Mitigation: set up a document freshness check — every HR policy document should have a "last verified" date, and a monthly alert should fire if any document hasn't been reviewed in 90 days.

**Risk 2: Provider model drift**
If ECHO India uses GPT-4o or Claude via API, the underlying model can change without notice. Mitigation: log the exact model version in every trace (the API returns this). Alert on any version change. Run the RAGAS golden test set immediately when a version change is detected.

The simplest ECHO India monitoring setup: a weekly scheduled eval run against 50 golden test cases, an alert if faithfulness drops below 0.75, and a monthly "policy freshness" review. Three hours of setup, ongoing protection.

---

## Key Takeaway

Model rot is real, silent, and common. The world changes, providers update their models, your users ask new questions, your policies evolve — and your AI product quality degrades quietly while your dashboards show green.

The solution is a monitoring stack that measures output quality continuously — not just system health. Schedule weekly eval runs against your golden dataset. Monitor input distribution for new query topics. Log the exact model version in every trace. Alert on quality drops before users notice them.

The teams that build and maintain great AI products are not the ones with the best models at launch — they're the ones with the best systems for detecting and responding to quality changes over time.
