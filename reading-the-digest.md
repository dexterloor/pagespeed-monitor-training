# Reading the digest

*The digest is the one Slack message the monitor posts after every check. This page goes through one line by line.*

← Back to the [guide](guide.md)

---

## When a digest arrives

- **Daily check:** once a day, after every page on the list has been checked. Ask the operator what time that is in your time zone.
- **"Check a page" form:** usually within a few minutes of clicking **Check now**. A slow page can take up to 15.
- **Every time.** When every page passes, it's a single line: `✅ PageSpeed Monitor: all 7 pages passed`. **If no digest arrives, something's wrong**: [tell the operator](what-to-do.md#no-digest).

---

## An example, line by line

The layout below is exactly what the monitor posts for a check of two pages: one slow page and one that doesn't exist. The summary and quick win are real Claude output from a test of The Verge's home page (October 2026). The run number is an example.

```
① PageSpeed Monitor: 1 of 1 page needs attention · 1 page couldn't be tested

② 🔴 https://www.theverge.com/: high risk · mobile 27 · desktop 53
③ The Verge scores poorly: 27/100 on mobile and 53/100 on desktop. On mobile the main
   content takes about 20 seconds to appear (LCP, the time until the biggest visible
   element loads), while desktop shows it in 1.8 seconds. The main culprit on both is
   heavy JavaScript that locks up the page for 3 to 4 seconds (TBT, time the page can't
   respond to taps and clicks).
④ Quick win: Add fetchpriority="high" (and remove lazy-loading) on the main hero image,
   and set long cache lifetimes on static files, to cut the 20-second mobile LCP.

⑤ ❌ https://this-domain-does-not-exist.invalid/: no PageSpeed data (6 failed runs)

⑥ Run 31 · details in the PageSpeed Monitor sheet (findings, history and errors tabs)
```

| # | Line | What it tells you |
|---|---|---|
| ① | **Headline** | *"1 of 1 page needs attention"*: of the pages that **could** be tested, how many were flagged. Pages that couldn't be tested are counted separately, after the `·`. |
| ② | **Page line** | The colored dot and word give the **risk**: 🔴 `high`, 🟠 `medium`, 🟡 `low`. Then the average **scores out of 100**, for a phone (mobile) and a computer (desktop). 90 or more is the target. Pages are listed highest risk first. A `–` instead of a score means every test on that device failed. |
| ③ | **Summary** | Two or three plain-English sentences: how the page does and what slows it down most. Technical terms are explained in brackets. Look up any others in the [glossary](glossary.md). |
| ④ | **Quick win** | The change the AI thinks gives the best payoff for the effort. Pass it on as it is. You don't need to understand it. |
| ⑤ | **❌ line** | The page **could not be tested**: no usable result, even after an automatic retry. This says nothing about speed. See [what to do](what-to-do.md#a-page-could-not-be-tested). |
| ⑥ | **Footer** | The **run number**, and which Sheet tabs have results from this check. Search for the run number in the Sheet's `run_id` column, and quote it when you ask the operator anything. |

---

## Other things you might see

| You see | Meaning |
|---|---|
| ✅ *all 7 pages passed* | Every page passed. Nothing to do. |
| *_(fallback summary)_* after the scores | Claude, the AI that writes summaries, wasn't available, so the monitor wrote a simpler one. It ends *"This summary was generated automatically because the AI analysis was unavailable."* **The scores and the risk are the same either way.** See [what to do](what-to-do.md#fallback-summary). |
| *_(error summary)_* after the scores | No real summary was written, only a placeholder. Tell the operator the first time you see it. |
| ⚠️ *"… 2 of the runs failed, results use the rest"* | Some of the 6 tests failed, but enough worked to give a result. Nothing to do. If the same page shows it every day, mention it to the operator. |
| A headline with only *"… couldn't be tested"* | No page could be tested. If it's the whole daily list, tell the operator. If it's one page you sent with the form, check the address. |
| `[TEST]` at the start | A test message from the operator. Ignore it. |
| 🚨 *PageSpeed Monitor failed at …* | Not a digest. The monitor itself broke during a check. It ends with an **Execution** number, which is the run number. Paste the whole message to the operator. |

---

## What the digest doesn't show

The digest is short on purpose. For the following, open the [Sheet](reading-the-sheet.md):
- **Root causes**: the full list of problems behind the score, each with an effort estimate (`findings` tab)
- **Trend**: whether the page was flagged yesterday, last week… (`history` tab)
- **Why a page couldn't be tested**: the error message (`errors` tab)

Next: [reading-the-sheet.md](reading-the-sheet.md)
