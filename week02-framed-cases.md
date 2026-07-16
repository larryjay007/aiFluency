# Week 2 — Framed Cases

## Voice Card

> Analytical, practical, curious, thoughtful, honest, professional, supportive.

---

## Case Study: Reading Results Before Trusting Them

### The Problem

Early in FlyRank's ML internship, a routine check produced a result that looked stable: a correlation between search volume and real page traffic stayed at 0.001, before and after filtering the dataset to only pages with real impressions. A stable number is easy to read as a confirmed finding.

### What I Did

When it was flagged that the filter hadn't removed a single row — 30,000 pages before, 30,000 after — I didn't take that at face value. I re-checked the counts myself, then worked through what "0 rows removed" actually implied: if the filter's job was to drop pages with zero impressions, and it dropped nothing, every page in the dataset already had impressions before I touched it. The dataset had been pre-cleaned upstream. The filter test hadn't run against anything.

That meant separating two claims I'd been treating as one:

- The original correlation (0.001) — still a real finding.
- The filtered re-test — once I understood the pre-cleaning, it told me nothing new.

I corrected the write-up to say exactly that, instead of the more convenient reading.

The same pattern showed up again in a decision tree exercise: swapping one feature for another, expecting it to matter, and finding it never appeared in the tree at all — `avg_position` separated the two outcome classes more cleanly than the feature I'd assumed would win.

### What Came of It

I now check, before trusting any "unchanged" or "stable" result: **could this test have changed anything at all, given the data it ran against?**

That question would have caught the first issue immediately, and it's the one I bring to every result now — not just the ones that get flagged for me.

---

## Bio

Before ML, I worked in construction, checking work against plans, interpreting working drawings, and designating tasks to workers. Now I bring the same habit to machine learning: I verify a result is actually right before I report it as one — not because I distrust the work, but because a convincing output isn't automatically a correct one.

---

## Contact / CTA

If you're about to trust an ML result you can't fully evaluate yourself, that's exactly what this is for.

**Email me** — Odesanyalanre3@gmail.com

---

## Before / After

**Generic AI line:**
> "I leverage data-driven insights to deliver results-driven solutions that drive impactful outcomes for stakeholders."

**Edited version:**
> "When a result looks fine, I check what would have to be true for that to actually be the case — and I say so plainly when it isn't."
>

