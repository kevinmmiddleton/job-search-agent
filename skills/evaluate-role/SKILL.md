---
name: evaluate-role
description: >
  Scores a single job posting against the user's profile and gives a clear
  apply / skip recommendation. Use when the user pastes a job description or
  posting URL and asks "should I apply to this," "score this," "evaluate this
  role," "how good a fit is this," "rate this posting," "is this worth my
  time," or wants to compare two or three roles. Produces dimension scores, a
  requirement-by-requirement match table against their profile, and a verdict.
  Not for interview prep — that's interview-guide-engine. Requires the shared
  profile; if none exists, route to job-search-setup first.
---

# Evaluate Role — Score One Posting

A scan tells the user a role *exists* and looks plausible. This skill answers the next question: **is this one worth applying to?** It reads the full posting, maps every requirement to the user's actual background, scores the fit, and says what it would do.

## Step 1: Load the profile

Read `job-search/profile.md` from the user's Cowork folder. No profile → hand off to `job-search-setup`; don't score against guesses.

Pull out: target titles and seniority, strong-fit signals, exclusions, location/work-mode rules, comp floor, and the Background section (career arc, proof points).

## Step 2: Get the full posting

- **Pasted JD** — use it as-is.
- **URL** — if a Claude-driven browser is connected, navigate and read the page. Otherwise try a direct fetch for ATS-hosted postings (Greenhouse/Lever/Ashby pages are server-rendered and fetch cleanly). If the page comes back empty or walled (LinkedIn postings often do), ask the user to paste the JD text. Never scrape around a login.

If all you have is a result-card snippet (title + company + location), say the evaluation will be shallow and offer to proceed anyway or wait for the full JD.

## Step 3: Extract the requirements

Pull the real requirements out of the wishlist. List 5–10 items, split into:

- **Must-haves** — stated as required, or structurally central to the role.
- **Nice-to-haves** — "bonus," "ideally," or clearly peripheral.

Name each one plainly ("ships 0-to-1 products," not "demonstrated end-to-end ownership of the product lifecycle").

## Step 4: Map requirements to the profile

This is the heart of the evaluation. Build a match table — every requirement, traced to the specific proof point or background line that answers it:

```
| Requirement | Evidence from your profile | Read |
|---|---|---|
| 5+ yrs platform PM | Proof point 2: internal tools platform at [Co], 4 yrs | ✅ match |
| Healthcare domain | Regulated-industry work at [Co] (finance, not health) | 🟡 partial — adjacent |
| People management | (nothing in profile) | ❌ gap |
```

Rules:
- **Cite the actual profile line.** "✅ match" with no evidence is a vibe, not a read.
- **Never invent or stretch.** If the evidence is adjacent, call it partial and say why.
- **For each gap:** one line on the honest mitigation (transferable skill, learning curve) — or "real gap, expect to be pushed on this."

## Step 5: Score it

Apply `references/scoring-rubric.md`. Score each dimension 1–5, then compute the weighted global score:

- **Requirements match** (40%) — from the table above
- **Target fit** (25%) — title, seniority, function vs. Search Targets; strong-fit signals
- **Logistics** (15%) — location, work mode, comp vs. the profile's rules
- **Company signals** (10%) — industry fit, stage/size if the user has stated preferences
- **Posting quality** (10%) — clarity and specificity of the JD itself; deduct for understaffing tells and vagueness

Unknowns score neutral (3), never punitive — most postings omit comp.

## Step 6: Give the verdict

Lead with the score and the action, then the table, then the reasoning:

```
**4.2 / 5 — strong match, worth applying.**

[match table]

What pushes it up: ...
What holds it back: ...
If you apply: lead with [proof point], expect pushback on [gap].
```

Tiers: **4.5+** apply now, prioritize it · **4.0–4.4** strong, apply · **3.5–3.9** decent — apply only with a specific reason · **3.0–3.4** weak fit, probably skip · **below 3.0** skip.

The verdict is decision support, not a command. If the user disagrees with a read, update the evaluation — and if the disagreement reveals a preference the profile doesn't capture, offer to save it to `profile.md` so future scores improve.

## Step 7: Log it

Offer to record the role in the tracker via `application-tracker`: status `prospect` (or `skipped` if the verdict was skip and the user agrees), with the global score rounded to the nearest integer in the `score` column and the one-line verdict in `notes`.

If the user is comparing multiple roles, score each one the same way and finish with a one-line ranking.

## Principles

- **Evidence or it didn't happen.** Every "match" cites a profile line. Honest partials and gaps make the score trustworthy.
- **The profile is the rule set.** Don't import preferences the user never stated. If something seems to matter but isn't in the profile, ask and save it.
- **Unknowns are neutral.** Don't punish a posting for not listing comp.
- **Skip is a valid verdict.** The point of scoring is to spend application effort where it pays.
