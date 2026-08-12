# Ship an Automation Workflow v2 — Source-Grounded Study Notes Pipeline

## Why This Pipeline

From my FL-01 workflow audit, "Researching Machine Learning Topics" was classified
"Delegate to AI with Review" — I need clear summaries and beginner-friendly explanations
fast, but I verify key details before relying on them. This pipeline is that classification,
built as an actual repeatable system.

## Step Diagram

```
[1. GATHER]  →  [2. SYNTHESIZE]  →  [3. DRAFT]  →  [4. REVIEW]  →  [5. FORMAT]
  NotebookLM       NotebookLM        Claude          Claude         Claude
  (add real         (grounded         (study notes    (self-check    (final
   sources)          briefing per      in template,    every claim    template,
                      topic, cited)     grounded        traces to      per topic)
                                        only in          briefing)
                                        briefing)
```

**Handoffs:**
- Gather → Synthesize: NotebookLM produces a briefing grounded only in the added sources —
  not general training knowledge.
- Synthesize → Draft: the briefing is pasted into the drafting step as the *only* allowed
  source of facts.
- Draft → Review: every claim in the draft gets checked against the briefing for
  traceability.
- Review → Format: only claims that survived the check get locked into the final template.

## Tools Used

- **NotebookLM** — sources added: `docs/data-dictionary.md`,
  `docs/ml-intern-dataset-and-lane-guide.md`, `skills/framing-ml-problems/SKILL.md` (real
  documents from the FlyRank internship repo).
- **Claude** — running Steps 3-5 via a structured instruction set (below), tested directly
  in conversation as a proxy for a saved Claude Project.

## Exact Prompt / Configuration Used

**Gather prompt (run once per topic in NotebookLM):**
> "Based only on the sources I've provided, give me a grounded briefing on [topic]: what it
> means, why it matters for a content-ranking ML project, and any gotchas mentioned in the
> sources. Cite which source each claim comes from."

**Draft/Review/Format instructions (Claude):**
> "You are running Step 3-5 of a source-grounded study notes pipeline.
> INPUT: a NotebookLM-grounded briefing on one ML topic, provided by the user.
> STEP 3 - DRAFT: Using ONLY the facts in the provided briefing (never add outside
> knowledge), write study notes covering: Definition, Why it matters for Lane 2
> (content-ranking), Key gotchas.
> STEP 4 - REVIEW: Go back through your own draft. For each claim, confirm it traces to
> something in the briefing. Flag anything that doesn't, explicitly.
> STEP 5 - FORMAT: Output the final version in this template: Definition / Why it matters
> for Lane 2 / Watch out for / One question to verify before trusting this / Source."

## The Five Runs

### Run 1 — Decision Trees
**Definition:** A supervised ML model evaluated on the 30,000-row starter dataset; in Lane
2, a "learned ranking" method where added complexity should earn its keep over simpler
baselines.
**Why it matters for Lane 2:** Precision@50 of 0.540 (27/50) vs. the baseline's 0.240
(12/50) — real evidence a learned model beats a fixed rule on this slice.
**Watch out for:** Feature leakage — `is_declining_label` is derived from `trend_direction`,
so including `trend_direction`/`trend_pct` as features creates a circular, worthless result.
**One question to verify before trusting this:** Were those columns actually excluded from
this specific model, or just described as something to exclude generally?
**Source:** NotebookLM briefing (citation markers not preserved in plain-text copy)

### Run 2 — Precision@K
**Definition:** Of the top K pages prioritized, how many actually matched the target label.
**Why it matters for Lane 2:** Matches real editorial capacity and is the actual proof ML
beats the baseline (0.240 → 0.540 → 0.740 across baseline/tree/forest).
**Watch out for:** The proxy-label trap, leakage from future-window features, the causal
illusion, and low-volume noise without minimum-impression filters.
**One question to verify before trusting this:** Does my own Lane 2 rule apply a
minimum-volume filter before ranking?
**Source:** NotebookLM briefing

### Run 3 — Data Leakage
**Definition:** Target-window or answer information sneaking into training features,
inflating validation scores while breaking production performance.
**Why it matters for Lane 2:** Named forms: circular results (feeding product decisions or
label-derived columns as features) and overlapping time windows.
**Watch out for:** Direct label derivation, query-table window overlap (only `*_prev30` is
safe there), client cross-contamination, and silently leaking `content_type` through blind
`fillna(0)`.
**One question to verify before trusting this:** Did my own ML-07 baseline run all six
leakage-audit questions, or only some?
**Source:** NotebookLM briefing

### Run 4 — Client-Holdout Validation
**Definition:** Grouping rows by `client_id` so whole clients are held out of training,
instead of randomly splitting individual pages.
**Why it matters for Lane 2:** Prevents memorizing client-specific quirks; a random split
makes the test artificially "too easy."
**Watch out for:** The unbalanced panel, the GSC-only early-history trap, and sibling/URL
leakage across splits.
**One question to verify before trusting this:** Does my own ML-04/ML-07 work use a
client-grouped split anywhere yet, or only a random one so far?
**Source:** NotebookLM briefing

### Run 5 — ROC-AUC vs. Precision/Recall Trade-offs
**Definition:** ROC-AUC measures overall separation across all thresholds; precision/recall
reflect the real policy choice at one specific cutoff.
**Why it matters for Lane 2:** Reviewers only act on a limited queue, so Precision@K matters
more than overall ROC-AUC — similar AUCs (0.750 vs 0.742) hid a large Precision@50 gap
(0.740 vs 0.540).
**Watch out for:** Circular labels inflating both metrics, the sparse-positive illusion, and
the causal fallacy.
**One question to verify before trusting this:** Given my ML-07 baseline used ROC-AUC (0.607
honest, 0.999 leaked), should I also track Precision@K for that same rule?
**Source:** NotebookLM briefing

## Time-Saved Estimate

Gather + synthesize (NotebookLM setup, adding 3 sources, running 5 grounded-briefing
prompts): **under 10 minutes**, timed honestly.

Draft + review + format (Claude, run live for all 5 topics in one pass): a few minutes.

Doing this manually — reading all three source documents cover-to-cover, then writing
structured notes by hand for 5 separate topics, cross-checking each claim myself — would
reasonably take 45-90 minutes for someone at my current reading speed with this material.
That puts the honest time saved somewhere around 30-75 minutes for this batch of 5 topics,
not counting the one-time setup cost of learning NotebookLM itself.

## Known Failure Points / What a Human Must Still Check

1. **Citation loss on copy-paste.** NotebookLM's inline citation markers (which source
   backs which claim) don't survive being pasted as plain text — I could confirm every
   claim traced to "the sources" generally, but not which specific one of the three
   documents. A human has to go back into the NotebookLM interface to check a specific
   claim's source if it matters.
2. **NotebookLM appends unsolicited follow-up offers** ("Would you like me to draft a
   leakage checklist?") mixed into the output — these need to be manually trimmed before
   the briefing is clean enough to hand to the drafting step.
3. **The "one question to verify" line in each note is a real, unresolved question**, not
   decoration — each one points at something about my own actual ML-04/ML-07 work that I
   have not yet gone back to double-check.
4. **The pipeline trusts NotebookLM's briefing as ground truth.** If NotebookLM itself
   misreads a source (summarization error), nothing downstream would catch it, since Steps
   3-5 are explicitly forbidden from adding outside knowledge to correct it.
