# Scoring Rubric

How to score one posting against the user's profile. Five dimensions, each 1–5; global score is the weighted average. The profile is the rule set; this file is the method.

> The dimensional approach here is adapted from [career-ops](https://github.com/santifer/career-ops) (MIT), reworked for this plugin's profile schema and on-demand flow.

## Dimensions and weights

| Dimension | Weight | What it measures |
|---|---|---|
| Requirements match | 40% | How much of the JD the user's background actually answers |
| Target fit | 25% | Whether this is even the kind of role they're hunting |
| Logistics | 15% | Location, work mode, comp vs. their stated rules |
| Company signals | 10% | Industry/stage fit against stated preferences |
| Posting quality | 10% | Whether the JD itself is a good sign |

## Anchors

Score whole or half points. Anchors below are the calibration; interpolate between them.

### Requirements match (40%)
Built from the match table (every requirement traced to a profile line).
- **5** — every must-have is a cited match; gaps only in nice-to-haves
- **4** — must-haves nearly all matched; one partial with a credible mitigation
- **3** — most must-haves matched or partial; one real gap
- **2** — multiple must-have gaps; the application would lean on stretch framing
- **1** — the role is asking for a different person

### Target fit (25%)
- **5** — target title + seniority, and hits one or more strong-fit signals
- **4** — target title and seniority, no strong-fit signal
- **3** — adjacent title or ambiguous seniority, plausibly in range
- **2** — outside the stated targets but arguably transferable
- **1** — wrong function or clearly wrong level

### Logistics (15%)
- **5** — work mode and location squarely inside the rules; comp listed at/above floor
- **4** — fits the rules; comp unlisted
- **3** — workable with a compromise the user has said they'd consider
- **2** — conflicts with a stated rule (e.g., hybrid in a city they ruled out)
- **1** — violates a hard rule (onsite-only in a banned location; listed comp far below floor)

### Company signals (10%)
- **5** — industry/stage the user actively wants (a strong-fit signal)
- **4** — neutral-to-positive; nothing in the exclusions
- **3** — no information either way
- **2** — adjacent to an excluded industry or a stated dislike
- **1** — excluded industry or company type (this usually EXCLUDEs at the gate — score 1 only if the user asked anyway)

### Posting quality (10%)
A clear, specific JD signals a team that knows what it's hiring for.
- **5** — specific responsibilities, named team/scope, clear level, comp listed
- **4** — clear and specific, comp unlisted
- **3** — standard boilerplate; nothing alarming
- **2** — vague responsibilities, kitchen-sink requirements, "wear many hats" understaffing tells
- **1** — barely a JD: generic paragraphs, contradictory level signals, copy-paste artifacts

## Rules

- **Unknowns score 3.** Missing comp, unstated work mode, no stage info — neutral, never punitive.
- **Exclusion-gate first.** If the role fails the hard gates in the find-jobs classification rubric (skip-list company, banned location, excluded function), say so — it doesn't need a score, it needs a skip. Score it anyway only if the user explicitly asks.
- **Round the global score to one decimal** for the verdict; round to the nearest integer for the tracker `score` column.
- **Show the dimension scores.** The global number earns trust by being decomposable.

## Tiers

| Global | Verdict |
|---|---|
| 4.5–5.0 | Apply now — prioritize it |
| 4.0–4.4 | Strong — apply |
| 3.5–3.9 | Decent — apply only with a specific reason (name it) |
| 3.0–3.4 | Weak fit — probably skip |
| < 3.0 | Skip |

## Calibration notes

- Early evaluations are only as good as the profile. If scores keep feeling off, the fix is usually a missing preference — capture it in `profile.md` rather than hand-adjusting scores.
- Resist grade inflation. If everything scores 4+, the rubric has stopped doing its job: re-anchor on the 3s.
