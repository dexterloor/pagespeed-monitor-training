# Glossary

*Every term used in this pack, the Slack digest and the Sheet, in plain words. The operator's [runbook](https://github.com/dexterloor/pagespeed-monitor/blob/main/docs/runbook.md) uses the same words.*

← Back to the [guide](guide.md)

---

## The monitor

### PageSpeed Monitor
The tool that checks the pages. It runs on its own once a day, and whenever someone uses the form.

### Run
One go of the monitor: either the daily check of every page, or one form submission. Each run has a number, shown in the digest footer ("Run 31") and in the Sheet's `run_id` column. A 🚨 alert calls the same number "Execution".

### Trigger
What started a run. `schedule` = the daily check. `form` = someone used "Check a page". `webhook` = another computer system asked for a check. It's a column in the `history` tab.

### "Check a page" form
A web page where you paste addresses to check them straight away, instead of waiting for the daily check.

### Digest
The single Slack message the monitor sends after a run, in `#pagespeed-alerts`. It lists flagged pages (highest risk first) and pages that could not be tested. If every page passed, it's a single ✅ line. **There's a digest after every check, so no digest means something went wrong.**

### Operator
The person who keeps the monitor running and fixes it when it breaks. Tell them about problems with the monitor, not problems with web pages.

### Client contact
The person at the client you report a page to. Your lead keeps the list of who that is for each site. They decide what gets fixed, and pass it to their developer.

### Escalation log
The `escalations` tab in the Sheet, which you fill in. One row for each page you've reported to a client (or logged for Monday's report), updated as things change. It's how everyone knows what has already been sent. See [the escalation log](what-to-do.md#the-escalation-log).

---

## Results

### Score
Google's speed score for the page. **0–100 in Slack, 0–1 in the Sheet** (27 = 0.27). The monitor runs the test 3 times on each device and averages the scores, because single tests vary. **The target is 90 (0.9).**

### Mobile / desktop (also *strategy*)
The two ways each page is tested: as a phone on a slow mobile connection, and as a computer on a fast connection. Mobile scores are almost always lower.

### Flagged
The page needs attention. A page is flagged when, on mobile or desktop:
- its score is **below 90**, or
- one possible fix would make it load **at least 0.1 seconds (100 ms) faster**.

So a page can score 95 and still be flagged with `low` risk.

### Risk
How urgent a flagged page is:
- `high` 🔴: a score below **50**, or one fix would save **1 second or more**
- `medium` 🟠: a score below **90**, or one fix would save **0.3 seconds or more**
- `low` 🟡: anything else that was flagged

The monitor works this out from the scores itself, so the same scores always give the same risk, whoever writes the summary. See [what to do](what-to-do.md) for each.

### Could not be tested
The monitor got no usable result for the page, even after one automatic retry 30 seconds later. Shown with ❌ in the digest. It says nothing about the page's speed. Possible causes: the page is down, the address is wrong, the site blocks Google's tester, or a problem on the monitor's side. [What to do](what-to-do.md#a-page-could-not-be-tested).

### Partial (⚠️ in the digest)
Some of the 6 tests for a page failed, but the rest were enough to give a result.

### Root cause
One underlying problem behind a slow page, for example "Too much JavaScript running on page load". Several of Google's detailed checks can share one root cause, so the monitor groups them. Listed in the `findings` tab.

### Quick win
The single fix the AI thinks gives the best result for the least effort. Shown in the digest and the `findings` tab.

### Effort
The AI's estimate of how big a job a fix is: `small` (a settings or small code change), `medium` (a few hours), `large` (a rebuild or a change of supplier). Automatic (fallback) summaries always say `medium`.

### ms (milliseconds)
Thousandths of a second. 1,000 ms = 1 second. "~4400ms" next to a root cause is Google's rough estimate of the time a fix could save, about 4.4 seconds. It's an estimate, not a promise.

---

## Who wrote the summary

### Claude
The AI that writes the plain-English summary, the root causes and the quick win for each flagged page. It only sees the test results, never the page itself.

### Analysis source
Who wrote a page's summary. A column in `findings`:
- `claude`: written by the AI (normal)
- `fallback`: the AI wasn't available, so the monitor wrote a simpler automatic summary. Shown as *(fallback summary)* in the digest
- `error`: no real summary was written, only a placeholder. Shown as *(error summary)*

### Fallback
The automatic summary used when the AI isn't available. Scores and risk are exactly the same either way. Only the wording is simpler.

---

## The Sheet

### `history` tab
Every page, every run, two rows each (mobile and desktop). Use it to see trends. Pages that could not be tested have no rows here.

### `findings` tab
One row per flagged page per run, with risk, summary, root causes and quick win.

### `errors` tab
One row per failed test, with the error message, plus a row whenever the monitor itself broke.

See [reading-the-sheet.md](reading-the-sheet.md).

---

## The detailed timings

These appear in the `history` tab and sometimes in summaries. You don't need to act on them; they're for developers.

| Term | Column | Plain words | Good on mobile: at or under |
|---|---|---|---|
| **LCP**, Largest Contentful Paint | `lcp_ms` | Time until the biggest thing on screen (usually the main image or headline) has appeared | 2.5 s (2500 ms) |
| **FCP**, First Contentful Paint | `fcp_ms` | Time until *anything* appears on screen | 1.8 s |
| **TBT**, Total Blocking Time | `tbt_ms` | How long the page is frozen and can't respond to taps or clicks while it loads | 0.2 s |
| **CLS**, Cumulative Layout Shift | `cls` | How much things jump around as the page loads (no unit: 0 is perfect) | 0.1 |
| **Speed Index** | `si_ms` | How quickly the page *looks* complete | 3.4 s |

The limits for desktop are stricter. Rather than learning the numbers, compare a page with itself over time.

### UTC
A world-standard clock. Times in the Sheet are in UTC, which may be several hours ahead of or behind your local time.

### Lighthouse
The testing engine inside Google's PageSpeed tool. You'll see the name in some error messages.

### Best practices
A second Google score (the `best_practices` column) for general good practice, such as security and browser errors. It's recorded but doesn't cause a flag.
