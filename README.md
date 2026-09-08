# NFL Sunday Pick 'Em — Automation

Three pieces, same pattern as Magnolia Madness:

1. **AppsScript/** — bound to your Google Sheet. Generates the weekly
   form automatically, closes it at kickoff, pulls live scores from
   ESPN, and computes the leaderboard.
2. **leaderboard/index.html** — a static page for GitHub Pages that
   shows the live standings and everyone's picks.

## 1. Set up the Apps Script

1. Open (or create) the Google Sheet you want to be the season's
   home base.
2. Extensions > Apps Script.
3. Create four script files matching the names in `AppsScript/` here
   (Config, FormGenerator, ScoreIngestion, Triggers) and paste in the
   matching contents.
4. In **Config.gs**, set `SPREADSHEET_ID` to this Sheet's ID (from its
   URL) and confirm `SEASON_YEAR`.
5. Run `generateWeeklyForm` once manually from the editor's function
   dropdown. Approve the permission prompts (it needs access to
   Forms, Sheets, and external requests to ESPN). Check the
   **Execution log** for the form link.
6. Run `installTriggers` once. This sets up:
   - `generateWeeklyForm` every Tuesday at 8am (next week's form)
   - `ingestScores` every 15 minutes (live scoring)

   The form's own auto-close-at-kickoff trigger is created
   automatically each week — nothing to do there.

7. Share the form link (`form.getPublishedUrl()`, shown in the log)
   with your group each week the same way you do now.

## 2. Publish the two data tabs

After the first `ingestScores` run creates the **Standings** and
**PicksGrid** tabs:

1. File > Share > Publish to web.
2. Under "Link", choose **Standings** as the sheet and **CSV** as the
   format, then Publish. Copy the URL.
3. Repeat for **PicksGrid**.

## 3. Set up the leaderboard page

1. Paste the two CSV URLs from step 2 into `leaderboard/index.html`,
   replacing `PASTE_STANDINGS_PUBLISHED_CSV_URL_HERE` and
   `PASTE_PICKSGRID_PUBLISHED_CSV_URL_HERE`.
2. Push this folder to a GitHub Pages repo (same pattern as
   `redraider16.github.io/magnolia_madness/` — could even live at
   `redraider16.github.io/nfl-pickem/`).
3. Share that page link — it refreshes itself every 60 seconds as
   games finish.

## Notes / things to sanity-check week 1

- **Payment stays manual**, same as today — the form still lists
  Venmo/CashApp, this just automates everything after that.
- **Tied games** (rare in the NFL): no team gets the 10-point team
  bonus that game, but combined-score points still apply normally.
- **Multiple entries per person** are supported — each form
  submission is scored as its own entry, same as your current sheet.
- Sheets' "Publish to web" CSV can lag a few minutes behind live
  edits — that's Google's caching, not a bug in the script.
