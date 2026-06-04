# What this plugin connects to

This plugin is deliberately low-dependency. It works with tools you already have, and degrades gracefully when something isn't connected.

## Browser (for scanning)

The `find-jobs` scan reads live results from a job board. The best experience uses a browser Claude can drive directly:

- **Claude-in-Chrome extension** — lets Claude open your saved search in your own logged-in Chrome and read the results. This is the closest thing to a real scanner: it sees what you'd see, including roles behind a login.

If the extension isn't installed, `find-jobs` falls back to a **paste flow**: you open your saved search yourself, copy the results (or the page), and paste them in. Slower, but zero setup and works on any machine.

The plugin never logs into anything on your behalf and never stores credentials. It reads pages in a browser session you already control.

## Scheduling (for automation, optional)

The `job-search-automation` tool can set up a **Cowork scheduled task** so your scan runs on a cadence (e.g., every weekday at 8am) without you asking. This uses Cowork's built-in scheduling — no extra connector. See that skill for the honest tradeoffs of running a browser-based scan unattended.

## Job board

Examples throughout assume **LinkedIn**, because its public search URLs are easy to save and reuse. The same approach works for Indeed, Built In, Wellfound, a company careers page, or any board with a stable, filterable search URL. Setup will help you build the link for whichever board you use.
