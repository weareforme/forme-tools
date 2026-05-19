# Forme Tools — Page Speed Tool
## Project Brief for Cursor / Claude

---

## What This Is

A standalone HTML tool hosted on GitHub Pages at:
`https://weareforme.github.io/forme-tools/pagespeed-tool.html`

It runs PageSpeed Insights checks on any URL, displays full results (scores, Core Web Vitals, opportunities, diagnostics), logs to Google Sheets, and generates actionable fixes.

Built by Studio Forme for ongoing performance tracking of client sites, starting with nory.ai.

---

## Repo

- GitHub: `weareforme/forme-tools`
- Branch: `main`
- Single file: `pagespeed-tool.html`

---

## Config (already in the file)

```js
const PSI_API_KEY     = '[ALREADY SET]';
const OAUTH_CLIENT_ID = '[ALREADY SET]';
const SHEET_ID        = '1eAL61WfIlZKNY6EVfkgw36DwiNmYj4lqQd7efmHbRQA';
const SHEET_NAME      = 'Sheet1';
```

Google Cloud project: `forme-website-1734088282497`
Both PageSpeed Insights API and Google Sheets API are enabled.
OAuth consent screen configured, app in testing mode (External).

---

## Current State

- PSI check works: runs mobile/desktop analysis, displays scores, Core Web Vitals, opportunities and diagnostics with expandable tables showing specific files/resources
- Google Sheets logging works: auto-logs each run with timestamp, URL, strategy, all scores and metrics. Headers auto-created on first run
- History table works: shows last 10 runs pulled from Sheets
- Action plan button: currently broken. Direct calls to the Anthropic API from the browser are blocked by CORS

---

## Next Task

Fix the action plan feature. The goal is: after a PSI check, the user clicks a button and gets a prioritised list of actionable fixes they can follow to improve the score.

**Approach to implement:** Format the PSI results into a well-structured prompt and copy it to the clipboard (or display it inline), so the user can paste it into Claude. No Anthropic API call needed.

The prompt should include:
- URL and strategy (mobile/desktop)
- All four category scores
- Core Web Vitals values
- Top failing audits with savings and specific file names
- A clear instruction asking for prioritised, Webflow-specific fixes

Alternatively, if a proxy/serverless approach is preferred: a Netlify function (`netlify/functions/claude.js`) can relay the request to the Anthropic API server-side, avoiding CORS.

---

## Stack

- Pure HTML/CSS/JS, no build step, no framework
- Google Identity Services (GSI) for OAuth
- Google Sheets API v4 for logging
- PageSpeed Insights API v5
- Fonts: DM Mono + DM Sans via Google Fonts

---

## Design

Dark theme. CSS custom properties throughout. Key colours:
- Background: `#0e0e0f`
- Accent (lime): `#c8f060`
- Good: `#4ade80`, Warn: `#fbbf24`, Bad: `#f87171`
- Font mono: DM Mono, Font sans: DM Sans

---

## Google Sheets Log Structure

| Timestamp | URL | Strategy | Performance | Accessibility | Best Practices | SEO | FCP | LCP | TBT | CLS | TTI |

---

## Future Features (backlog)

- Site-wide checks: loop through multiple URLs from a list
- Score trend chart: visualise performance over time using Sheets data
- Scheduled checks: automated runs (would need a backend or GitHub Action)
- Export report: PDF or shareable summary for clients
