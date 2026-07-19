# Prompt Iteration Log — "What Is Machine Learning?"

**Task:** FL-01 target task — Researching Machine Learning Topics
**Goal:** Build a prompt that goes from a generic textbook answer to one that's
directly useful for my actual work on FlyRank's Lane 2 ranking prototype, and that
honestly reflects what my model can and can't claim.

---

## Baseline

**Prompt:**
> "What is machine learning"

**Output:**
> "Machine learning is a subset of artificial intelligence that enables computers to
> learn patterns from data and make predictions or decisions without being explicitly
> programmed for every scenario... There are three main types: supervised learning...
> unsupervised learning... reinforcement learning... Machine learning powers things
> like spam filters, recommendation systems, voice assistants, and self-driving cars."

**Notes:**
- What changed in the prompt: n/a — starting point.
- What actually happened in the output: a broad, encyclopedia-style overview —
  correct but disconnected from anything I'm actually doing.
- What still failed: no connection to my actual internship work, no indication of
  depth level, reads like it's aimed at nobody in particular.
- What I'd try next: a role for the explainer is a natural first move.

---

## Version 1 — Added: Role Assignment

**Prompt:**
> "You are a senior ML engineer mentoring me, a new intern. Explain: what is machine
> learning?"

**Output (excerpt):**
> "Good question to start with... Here's a mental model I use with new engineers:
> think of a junior dev being handed 10,000 old support tickets... What's the actual
> task in front of you right now? That'll tell us which of these three types is
> actually relevant."

**Notes:**
- What changed in the prompt: added a role — senior ML engineer mentoring a new
  intern.
- What actually improved in the output: tone shifted from textbook to conversational
  mentoring — relatable analogies instead of generic examples, and it ended by
  asking about my actual task instead of just stopping.
- What still failed: the analogies are still generic mentor-analogies, not tied to
  anything about FlyRank or what I'm actually working on.
- What I'd try next: context and motivation — give it the real situation instead of
  a generic "new intern."

---

## Version 2 — Added: Context and Motivation

**Prompt:**
> "...Context: I'm working on FlyRank's ML internship, building a system that ranks
> which web pages a content reviewer should check first, based on search and
> engagement signals. I need to understand ML well enough to explain honestly what
> my model can and can't claim — not just build something that works."

**Output (excerpt):**
> "For your refresh-ranking system: you could write a rule like 'flag pages older
> than 180 days with high impressions.' That's not ML, that's a hand-rule... an ML
> model doesn't 'understand' why a page is declining. It finds statistical patterns
> in signals that correlate with your label."

**Notes:**
- What changed in the prompt: added context and motivation — the actual FlyRank
  task, and why I'm asking.
- What actually improved in the output: stopped being a generic ML explainer and
  directly connected the definition to my actual baseline-vs-model distinction — it
  pre-empted my "careful words" concern (correlation vs. causation) without being
  asked.
- What still failed: still one long block of reasoning — hard to quickly reference
  later.
- What I'd try next: output structure — make the can-claim/can't-claim distinction
  scannable.

---

## Version 3 — Added: Output Structure

**Prompt:**
> "...Structure your answer in exactly three labeled sections: DEFINITION (2-3
> sentences), CAN CLAIM (bullet list), CAN'T CLAIM (bullet list)."

**Output (excerpt):**
> "**CAN CLAIM** — This ranking is based on signals that have historically
> correlated with decline... **CAN'T CLAIM** — That any single page is declining
> 'because of' a specific factor... Any insight into how Google's real ranking
> algorithm works."

**Notes:**
- What changed in the prompt: added output structure — three labeled sections,
  specified content type each.
- What actually improved in the output: genuinely reference-able now — could
  screenshot the CAN CLAIM/CAN'T CLAIM lists and use them while writing an
  assignment. Findability improved enormously.
- What still failed: the structure enforces clarity but not depth — it condensed
  Version 2's reasoning without adding anything new, and doesn't check that I
  actually understood *why* each item belongs where it is.
- What I'd try next: step decomposition — show the reasoning that gets from "here's
  a signal" to "here's why it belongs in CAN CLAIM vs CAN'T CLAIM."

---

## Version 4 — Added: Step Decomposition

**Prompt:**
> "...Before giving your final structured answer, walk through your reasoning step
> by step, using one concrete example: take the signal `avg_position` from my
> dataset, and show, step by step, how you'd decide whether a claim about it belongs
> in CAN CLAIM or CAN'T CLAIM. Then give the three-section structured answer."

**Output (excerpt):**
> "1. What does avg_position actually measure?... 3. Was it measured before the
> decision point, or could it leak the future?... 5. Conclusion: 'Average position
> is one of the signals used to rank this page for review' → CAN CLAIM. 'This page's
> position proves it will decline' → CAN'T CLAIM."

**Notes:**
- What changed in the prompt: added step decomposition — forced the reasoning chain
  for one concrete signal before the final answer.
- What actually improved in the output: first version that shows *why* something
  belongs in each list, not just that it does — the leakage check (step 3) is
  genuinely new content, directly useful for the internship's actual data contract
  work later on.
- What still failed: only walked through one example signal — a demonstrated
  pattern, not something I could apply myself to a new signal without asking again.
- What I'd try next: few-shot examples — show 2-3 worked examples so the reasoning
  pattern becomes reusable.

---

## Version 5 — Added: Few-Shot Examples

**Prompt:**
> "...Before your final answer, work through TWO examples in `<example>` tags,
> showing the same 5-step reasoning for two different signals: `content_age_days`
> and `ctr`. Then give the three-section structured answer, generalized from the
> pattern in both examples."

**Output (excerpt):**
> "`<example>` Signal: content_age_days... `<example>` Signal: ctr... **CAN CLAIM**
> Each signal used (position, age, CTR, etc.) is part of what the ranking is based
> on, and historically correlates with the label..."

**Notes:**
- What changed in the prompt: added few-shot examples — two worked signals instead
  of one.
- What actually improved in the output: the reasoning pattern is now clearly
  generalizable — seeing it applied to two genuinely different kinds of signals (a
  temporal one, a rate-based one) makes it obvious how the same 5 steps would apply
  to a new signal.
- What still failed (honest note): the CAN CLAIM/CAN'T CLAIM sections barely changed
  from Version 4 — the examples improved the *demonstration*, not really the *final
  deliverable*. More examples don't always change everything.
- What I'd try next: verification — ask it to double check its own CAN'T CLAIM list
  against the lane guide's actual "Do Not Do This" section.

---

## Cross-Model Comparison (Version 5 Prompt, Run Identically on Both)

| | Claude | ChatGPT |
|---|---|---|
| **Tone** | Stayed in the "senior engineer mentoring an intern" voice throughout — direct, conversational | Read like a formatted report — headers and bullets, little sense of a person talking |
| **Structure compliance** | Followed the exact instruction: three sections, labeled exactly DEFINITION / CAN CLAIM / CAN'T CLAIM | Used four differently-named sections instead — didn't follow the literal structure requested |
| **The 5-step reasoning** | Used the exact 5 steps specified (measure, mechanism, leakage/decision-point check, causal-claim check, conclusion) | Opened with a refusal — "I can't provide my private reasoning process" — then substituted a *different* 5-point framework of its own, skipping the leakage/decision-point check entirely |
| **FlyRank-specific accuracy** | Explicitly included "no insight into Google's actual ranking algorithm" in CAN'T CLAIM — matches the lane guide's real rule | Never mentioned this at all — a real, specific omission |
| **Where ChatGPT was stronger** | — | Its CAN'T CLAIM list included "predicts correctly in situations very different from what it was trained on" — a valid generalization point Claude never raised in any version |

**Most specific finding:** ChatGPT misread "walk through your reasoning step by
step" as a request to expose internal chain-of-thought, and refused that framing —
even though the prompt asked for a visible, written-out 5-step framework, not
internal reasoning. That refusal also caused it to quietly drop the leakage-timing
check, which matters specifically for this lane's actual leakage rules.

---

## Final Reusable Template

> "You are a senior [role] mentoring me, a new [your role]. Explain: [topic].
> Context: I'm working on [your actual project/goal] — I need to understand this
> well enough to [your real reason for asking, not just get an answer]. Before your
> final answer, work through [N] examples in `<example>` tags, applying the same
> explicit reasoning steps to each: [list your specific steps]. Then give your final
> answer structured in exactly these labeled sections: [section names]."
