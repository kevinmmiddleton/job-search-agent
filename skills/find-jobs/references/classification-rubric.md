# Classification Rubric

How to tag each scanned role STRONG / INCLUDE / EXCLUDE. The user's `profile.md` is the rule set; this file is the method. Run it per role, fast and consistently.

## Decision order

Check in this order. The first failing rule sends a role to EXCLUDE.

1. **Skip-list company?** If the company is on the user's named skip list, or matches an excluded company type (staffing/recruiting firm, agency, etc.) → EXCLUDE, reason "skip list."
2. **Wrong level?** If the title's seniority is clearly below (or far above) the user's target seniority → EXCLUDE, reason "wrong level." Be generous with ambiguous titles ("Product Manager" with no modifier can be senior — don't cut it on level alone).
3. **Wrong function/role type?** If it's an excluded function from the profile (e.g., ML/Data-Science PM when excluded, ad-tech PM, sales engineering) → EXCLUDE, reason "excluded function."
4. **Excluded industry?** If the company is in an industry the user avoids → EXCLUDE, reason "excluded industry."
5. **Wrong location?** If onsite-only in a place the user won't work, or a location they ruled out → EXCLUDE, reason "location." Remote/unspecified always passes.
6. **Comp below floor?** Only if the posting actually lists comp and it's clearly below the floor → EXCLUDE, reason "comp below floor." **Unlisted comp passes** — most postings don't list it.

If a role survives all six, it's at least INCLUDE. Then check for STRONG:

7. **Strong-fit signal?** If it hits one or more of the user's strong-fit signals (e.g., 0-to-1 build, regulated industry, internal tools/platform, AI tooling) → upgrade to **STRONG**.

## The tie-breaker rule

**When genuinely unsure, INCLUDE.** A scanner that only surfaces perfect matches misses good roles with imperfect titles. Optimize for recall: show the borderline role with an honest one-line read and let the user decide. The cost of a false include (a quick "skip") is far lower than the cost of a false exclude (a missed opportunity the user never sees).

## Level inference (titles are messy)

Map common title words to levels, but don't be rigid:

- **Below target (cut for a senior+ search):** Intern, Junior, Associate, Coordinator, Analyst-titled PM roles.
- **At target:** Product Manager, Senior PM, Staff PM, Principal PM, Lead PM, Group PM, Product Lead, Head of Product (for director-level searches), Director of Product.
- **Above target (flag, don't auto-cut):** VP Product, CPO — surface with a note rather than dropping, in case the user wants a stretch.

A bare "Product Manager" with no seniority word is ambiguous. Default to INCLUDE and note the uncertainty rather than excluding.

## Company-type inference

When the company name alone tells you it's a staffing/recruiting firm, agency, or other excluded type (and it matches the user's exclusions), cut it without needing the full posting. Common staffing tells: "Staffing," "Talent," "Recruiting," "Consultants," generic three-letter outfits that repost the same roles. Only apply this if the user's profile excludes that company type.

## Fuzzy dedup (against the tracker)

Two roles are the "same" for dedup purposes if the company matches and the role is a near-match after normalizing:
- Strip seniority prefixes ("Sr.", "Senior", "Staff", "Lead") before comparing.
- Normalize punctuation and spacing.
- Treat "Sr Product Manager" and "Senior Product Manager, Platform" at the same company as a likely duplicate — when in doubt about a near-match, treat it as seen (the user already triaged that company/role family).

## Output per role

For each non-excluded role, produce:
- tier (STRONG / INCLUDE)
- one-line "why it fits" that names the specific signal it hit
- the fields needed for the tracker: company, role, location, comp (if listed), url

For excluded roles, keep only a tally by reason for the "hid N" summary line. Don't trust model-paraphrased URLs — always carry the real posting URL from the parse step.
