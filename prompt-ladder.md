# Prompt Ladder — "Explain This Data"

Baseline plus five versions, each adding exactly one layer, run against the same
underlying goal: understanding `content_refresh_anonymized.csv` well enough to know
which pages need a content review first.

---

## Baseline

**Prompt:**
> "Explain this data"

**Output:**
> "I'd be happy to help explain some data! However, I don't see any data attached to
> your message — could you share the dataset, file, or specific numbers you'd like me
> to explain? Once I can see it, I can walk you through what it contains, any patterns
> worth noting, and answer any specific questions you have about it."

**Notes:**
- What changed in the prompt: n/a — this is the starting point.
- What actually improved in the output: n/a — nothing got explained. The prompt
  didn't function as a request, it functioned as a request for a request.
- What still failed: everything — zero actual content.
- What I'd try next: the prompt needs to actually reference real data.

---

## Version 1 — Added: Real Context

**Prompt:**
> "Explain this data: `content_refresh_anonymized.csv`, 30,000 pages, 44 columns
> including `impressions_90d`, `avg_position`, `ctr`, `trend_direction`,
> `content_age_days`, `word_count`."

**Output (excerpt):**
> "This looks like a content performance dataset... `impressions_90d` — how many
> times each page appeared in search results over the last 90 days... `ctr` —
> click-through rate... Would you like me to go deeper into any particular column?"

**Notes:**
- What changed in the prompt: added real context — named the actual file and six
  real column names.
- What actually improved in the output: it stopped asking "what data?" and produced
  specific, plausible column explanations instead of a generic refusal.
- What still failed: the explanations were inferred from column *names alone*, not
  verified against the actual data dictionary — confident-sounding, but unconfirmed.
  It also never asked what kind of explanation I actually wanted.
- What I'd try next: a clearer goal — tell it what kind of explanation I actually
  want, since right now it just guessed.

---

## Version 2 — Added: A Clearer Goal

**Prompt:**
> "...My goal is to understand what these columns can tell me about which pages
> might need a content review first."

**Output (excerpt):**
> "Given your goal — identifying pages that need review first — here's how these
> columns likely combine to signal that: High `impressions_90d` but low `ctr`...
> `trend_direction` showing decline... No single column tells you 'review this page'
> on its own — it's really about combinations."

**Notes:**
- What changed in the prompt: added a clearer goal — what the explanation is *for*,
  not just what data exists.
- What actually improved in the output: it stopped listing columns in isolation and
  started explaining how they combine to signal something — much closer to
  something I could actually act on.
- What still failed: still speaking in general SEO logic, not my actual data — never
  asked what thresholds count as "low," or who's going to act on this.
- What I'd try next: a defined audience — the tone/depth should change depending on
  who this explanation is actually for.

---

## Version 3 — Added: A Defined Audience

**Prompt:**
> "...Explain it for a non-technical startup founder who has no ML or SEO
> background."

**Output (excerpt):**
> "Think of this data as a health checkup for every page on a website... The pages
> you'd want a team to look at first are usually the ones that are still being
> found by people (decent impressions), but something's clearly working against
> them."

**Notes:**
- What changed in the prompt: added a defined audience — non-technical startup
  founder, no ML/SEO background.
- What actually improved in the output: technical terms got reframed with plain
  analogies ("health checkup," "page 1 vs page 5"), and it made an actual
  prioritization judgment explicit, which Version 2 only implied.
- What still failed: still fairly long for someone with zero background — a founder
  skimming this might bounce partway through.
- What I'd try next: a specified output format — something tighter than flowing
  paragraphs.

---

## Version 4 — Added: A Specified Output Format

**Prompt:**
> "...Keep it under 120 words, using no more than 5 short bullet points, no long
> paragraphs."

**Output (excerpt):**
> "- Seen but not clicked — pages that show up in search but people scroll past.
> - Losing ground — pages that used to perform well but are trending down...
> Priority order: pages with real visibility and one of these problems first."

**Notes:**
- What changed in the prompt: added a specified output format — word limit and
  bullet-count cap.
- What actually improved in the output: genuinely scannable — readable in 15
  seconds instead of a minute, and the priority logic survived the cut.
- What still failed (honest "made it worse" moment): it dropped all the plain-
  English analogies from Version 3 to hit the word count — more skimmable, but
  less accessible for someone with truly zero background. Real tension between
  "short" and "founder-friendly," resolved here by sacrificing the second one.
- What I'd try next: a constraint that tries to recover the analogy without giving
  up the tight format.

---

## Version 5 — Added: A Constraint

**Prompt:**
> "...Constraint: include exactly one plain-English analogy somewhere in the
> explanation, but keep everything else as terse as possible."

**Output (excerpt):**
> "Think of this like a health checkup for every page on a website. Here's what to
> watch for: - Seen but not clicked... - Losing ground... Priority: fix pages with
> real visibility and one of these problems first."

**Notes:**
- What changed in the prompt: added a constraint — exactly one analogy required,
  everything else stays terse.
- What actually improved in the output: got both things at once — the "health
  checkup" framing came back for accessibility, while the rest stayed exactly as
  scannable as Version 4. The two constraints didn't fight each other.
- What still failed: doesn't tell the founder what to do next (no call to action),
  and never states any real numbers from the dataset — every version so far has
  reasoned from column *names*, never actual computed values.
- What I'd try next: a verification requirement — ground the explanation in at
  least one real, computed number instead of reasoning from column names alone.

---

## Final Reusable Prompt

> "Explain [dataset name], [N rows/pages], [M columns] — name the columns most
> relevant to your question. Goal: [what decision or understanding you're trying to
> reach]. Audience: [who this explanation is for and their background level].
> Format: keep it under [X] words, using no more than [Y] short bullet points, no
> long paragraphs. Include exactly one plain-English analogy, but keep everything
> else terse. Ground the explanation in at least one real, computed number from the
> data — don't reason from column names alone."
