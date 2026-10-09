# Reading the Sheet

*The **PageSpeed Monitor** Google Sheet keeps every result. It has three tabs the monitor fills in, plus the `escalations` tab you fill in. You only need a few columns in each.*

← Back to the [guide](guide.md)

---

## Before you start: two things that trip people up

1. **Scores in the Sheet are 0 to 1, not 0 to 100.** `0.27` in the Sheet is the same as `27` in Slack. The target is `0.9` (90).
2. **Times are in UTC**, a world-standard clock that may be hours ahead of or behind yours. For example, `2026-10-09T06:02:11.000Z` means 06:02 UTC on 9 October; the `Z` at the end means UTC. Don't use the date in this column to decide whether "today's" check ran: use the digest for that.

Each **run** (one daily check, or one form submission) has a **run number**. It's in the `run_id` column and in the footer of the Slack digest, so you can match the two.

---

## `history`: every page, every run

**Two rows per page per run**: one for mobile (phone), one for desktop (computer), whether the page passed or not. Exceptions: a page that *could not be tested* gets no rows here (look in `errors`), and if every test on one device failed, that device's row is missing.

| Column | You need it? | What it means |
|---|---|---|
| `timestamp` | ✅ | When the results were put together, at the end of the check (UTC) |
| `run_id` | ✅ | The run number, the same as in the Slack footer |
| `trigger` | ✅ | What started the run: `schedule` (the daily check), `form` (someone used "Check a page") or `webhook` (another computer system asked for a check) |
| `url` | ✅ | The page |
| `strategy` | ✅ | `mobile` (tested as a phone) or `desktop` (tested as a computer) |
| `performance` | ✅ | The speed score, 0 to 1. Below `0.9` is flagged |
| `flagged` | ✅ | `TRUE` if this page crossed a limit on this device |
| `flag_reasons` | Sometimes | Why it was flagged, for example `performance 0.27 < 0.9; 6 opportunities saving >= 100ms (top: unused-javascript 4560ms)`. That reads: score 0.27 is below 0.9, and Google found 6 possible fixes that would each save at least a tenth of a second |
| `best_practices` | Rarely | A second score, 0 to 1, for general good practice (security, errors). It doesn't cause a flag |
| `lcp_ms`, `fcp_ms`, `tbt_ms`, `cls`, `si_ms` | Rarely | Detailed timings for developers. See the [glossary](glossary.md#the-detailed-timings) |
| `runs` | Rarely | How many tests were averaged. Normally `3`. Lower means some tests failed |

### Find a run from the digest

Take the run number from the digest footer, then press **Ctrl+F** (**Cmd+F** on a Mac) and search for it. Its rows share that `run_id`. Ctrl+F only searches the tab you're on, so try `history` first, then `errors` (a run where every page was ❌ only has rows there). If the number isn't in the Sheet at all, [tell the operator](what-to-do.md#the-table) (row 10).

### Is this page getting worse, or is it always like this?

Turn on filters: click any cell in the header row, then **Data → Create a filter**. Click the filter icon on `url` and pick the page, then on `strategy` and pick `mobile`. Read the `performance` column down the dates. A page that has always been `0.5` is a known slow page. A page that dropped from `0.9` to `0.5` this week has a new problem, which is more urgent.

---

## `findings`: one row per flagged page

Only flagged pages get a row here, one per check. This is where the detail behind the Slack message lives.

| Column | You need it? | What it means |
|---|---|---|
| `timestamp`, `run_id`, `url` | ✅ | Same as in `history` |
| `risk` | ✅ | `low`, `medium` or `high`, worked out by the monitor from the scores. [How](glossary.md#risk) |
| `mobile_score`, `desktop_score` | ✅ | Speed scores, 0 to 1 |
| `summary` | ✅ | The same plain-English summary as in Slack |
| `root_causes` | ✅ | The list of problems, roughly most important first. See below |
| `quick_win` | ✅ | The same quick win as in Slack |
| `analysis_source` | Sometimes | Who wrote the summary: `claude` (the AI), `fallback` (an automatic summary, used when the AI wasn't available) or `error` (only a placeholder was written) |
| `fallback_reason` | Rarely | Why the fallback was used. Copy it to the operator if you're asked; you don't need to understand it |

### Reading `root_causes`

Each line looks like this (a real example):

```
1. Too much JavaScript running on page load [both, ~4400ms, large]: Profile the main-thread work and defer or lazy-load non-essential scripts ...
```

| Part | Meaning |
|---|---|
| `Too much JavaScript running on page load` | The problem |
| `both` | Which devices it affects: `mobile`, `desktop` or `both` |
| `~4400ms` | Google's rough estimate of the time a fix could save on one speed measure: 4,400 milliseconds, about 4.4 seconds. **It's an estimate, not a promise**, so don't quote it to a client as "your page will be 4 seconds faster". `~0ms` means Google couldn't put a number on it, not that it doesn't matter |
| `large` | The AI's estimate of how big a job the fix is: `small` (a settings or small code change), `medium` (a few hours) or `large` (a rebuild or a change of supplier). Fallback rows always say `medium` |
| After the `:` | The fix, written for a developer. Pass it on as it is |

The savings overlap, so **don't add them up**. Fixing two problems that each save 4 seconds won't save 8.

---

## `errors`: things that went wrong

One row for every test that failed, even after the automatic retry, plus a row whenever the monitor itself broke (those have an empty `url`). You rarely need this tab. Look here when the digest shows a ❌ or ⚠️ line.

| Column | You need it? | What it means |
|---|---|---|
| `timestamp`, `run_id`, `url` | ✅ | When, which run, which page |
| `error_message` | ✅ | What went wrong, for example `mobile run 1: Lighthouse returned error: FAILED_DOCUMENT_REQUEST.` That means Google's tester couldn't load the page. You don't need to understand these: copy one to the operator |
| `http_status`, `attempts`, `workflow`, `node` | Rarely | Technical details for the operator |

A page that couldn't be tested has **6 rows** here (3 tests × 2 devices). Copy one `error_message` when you report it.

---

## `escalations`: what's been sent

You fill this tab in, not the monitor. There's one row per page you've reported to a client, or logged for Monday's report, which you edit as things change. See [the escalation log](what-to-do.md#the-escalation-log).

Next: [what-to-do.md](what-to-do.md)
