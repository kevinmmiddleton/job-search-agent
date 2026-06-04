---
name: application-tracker
description: >
  Maintains a local, file-based job application tracker (a CSV in the user's own folder)
  and reports pipeline status. Use when the user says "I applied to X," "add this to my
  tracker," "what's pending," "what's in my pipeline," "show my applications," "update the
  status of," "mark as rejected," "I got a rejection from," "what's stale," or "follow-ups."
  Also use to log scan matches as prospects and to give a pipeline scorecard. Replaces a
  database with a plain CSV that opens in Excel, Numbers, or Google Sheets.
---

# Application Tracker

A job tracker that's just a file the user owns. No database, no server. One CSV they can open in any spreadsheet app, maintained carefully by you. This skill both records state and reads it back as a coach-style status.

## The file

Canonical tracker: `job-search/applications.csv` in the user's Cowork folder.

Columns (in this order):

```
company,role,location,comp,url,source,status,score,date_added,date_applied,last_update,notes
```

- **status** is the lifecycle: `prospect`, `applied`, `screening`, `interview`, `final`, `offer`, `rejected`, `skipped`, `ghosted`, `withdrawn`, `hold`.
- **score** is 1–3 (3 = strong fit) from the scan, optional.
- **date_added** is when it entered the tracker; **date_applied** when the user applied; **last_update** whenever status changes.

If the file doesn't exist, create it with the header row before the first write. See `references/tracker-schema.md` for full column definitions and status meanings.

## Edit the CSV with Python, not by hand

Hand-editing CSV rows is error-prone (commas in titles, quoting, alignment). For every add/update, use a short Python snippet via the shell that uses the `csv` module. This keeps quoting correct and updates atomic. `references/tracker-schema.md` has ready-to-adapt snippets for: create-if-missing, append rows, update status by company+role, and batch rejection updates. Always read the file back after writing to confirm the change landed.

## Operations

### Log prospects (from a scan)

When `find-jobs` hands over matches, append each as a row with `status=prospect`, the scan's `score`, `source` = the board, and `date_added` = today. Skip any that already exist (company + role match). Confirm: "Added N roles as prospects."

### Mark applied

When the user says "I applied to X" (or several): for each, if the role exists as a `prospect`, update it to `status=applied` and set `date_applied`. If it's new (applied outside a scan), insert a fresh row with `status=applied`. Record `resume_version` or any note they give in `notes`.

### Update status

"Moved to phone screen," "got an interview," "final round," "offer," "withdrew" → update `status` and `last_update`. Add a dated line to `notes` when the user gives context.

### Rejections (single or batch)

"I got a rejection from X" or a forwarded rejection email → find the role (fuzzy match company + role), set `status=rejected`, update `last_update`, optionally note the round it died at. Handle batches in one pass: "Marked 4 as rejected."

### Skipped

If the user reviews prospects and skips some, set those to `status=skipped`. They stay in the file so future scans dedupe them out.

## Status / coach mode

When the user asks "what's pending," "what's in my pipeline," or "how's my search going," read the CSV and report like a coach, not a clerk. Compute and present:

**Pipeline scorecard**
- Totals by status; active interviews (screening/interview/final/offer); applied this week and this month.
- A simple conversion read: interviews ÷ applications.

**Needs attention**
- **Stale actives:** any role in an active status (`applied`, `screening`, `interview`, `final`) with `last_update` 7+ days ago. Flag each and suggest a follow-up. Offer to draft a check-in.
- **Prospect backlog:** count of `prospect` rows not yet acted on; if 10+, nudge to triage.
- **Recent rejections:** anything rejected in the last 48 hours — acknowledge briefly, don't dwell.

**Momentum (when relevant)**
- If no application in 3+ days, note it gently. If they've been applying steadily, say so. Job searching is draining; notice the effort, not just the output.

Lead with what needs action. Keep it skimmable. Surface patterns if they're obvious (e.g., several applications at one company with no movement, or a level/industry with a high rejection rate) — but suggest, don't nag, and don't repeat a suggestion the user already declined.

## Exporting

The tracker is already a CSV, so it opens directly in Excel, Numbers, or Google Sheets — no export needed. If the user wants a formatted spreadsheet (tabs, filters, conditional formatting), offer to build one with the xlsx skill from the same data.

## Principles

- **The user owns the file.** It's plain CSV in their folder. They can edit it by hand, sort it, or open it anywhere. Respect their edits — read before you write.
- **Write carefully.** Use the csv module via Python; never blind-append a hand-typed line. Read back to confirm.
- **One source of truth.** Prospects, applications, and outcomes all live in one file with a status column. Don't fragment across files.
- **Coach, don't clerk.** When reporting status, lead with what needs action and notice momentum. The tracker exists to move the search forward, not just to store rows.
