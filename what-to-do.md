# What to do

*Find what you're looking at in the table and do what that row says. The table decides how serious it is, so you don't have to.*

← Back to the [guide](guide.md)

> Demo setup: the daily check is switched off for now, so ignore "no digest" (row 9) until the operator turns it back on.

---

## Before you use the table

1. Problems with a page go to the client contact for that site. Your lead has the list. The client decides what gets fixed and when.
2. Problems with the monitor go to the operator, the person who keeps it running. They work from the [runbook](https://github.com/dexterloor/pagespeed-monitor/blob/main/docs/runbook.md).
3. Look in the [escalation log](#the-escalation-log) before you message a client, and fill it in straight after. It's the `escalations` tab in the Sheet, and it's how the team knows what's already gone out.
4. React 👀 on a digest when you start on it and ✅ when you've finished. If someone else's 👀 is already there, leave it to them.
5. "Today" means before your shift ends. The weekly report goes out on Mondays: one message to each client contact, with their C lines and then their D lines. Your lead will tell you who sends it.
6. If what you see isn't in the table, or you're not sure, ask your lead rather than guess.

---

## The table

| # | You see | Check first | Then | When |
|---|---|---|---|---|
| 1 | ✅ *… passed* | | Nothing. React ✅ | |
| 2 | 🔴 or 🟠 on a page that isn't in the log | [Re-check it once](#re-checking-a-page) | Still 🔴 or 🟠: send [template A](#a-new-problem) and add a log row | Today |
| 3 | A digest from your own re-check | | Use it as the result of the re-check. Don't re-check it again | Today |
| 4 | Risk higher than in the page's log row (🟠 → 🔴), and `last_sent` is more than 7 days ago | [Re-check it once](#re-checking-a-page) | Still higher: send [template B](#b-it-got-worse) and update the row | Today |
| 5 | 🔴 or 🟠 on a page already in the log, and row 4 doesn't apply | | Nothing today. It goes in the weekly report as a [C line](#c-weekly-still-open) | Monday |
| 6 | 🟡 low | | Nothing. Low is never reported on its own | |
| 7 | A logged page that hasn't been 🔴 or 🟠 in the last 3 daily checks | `history`, filtered to the page and `trigger` = `schedule` | Put a [D line](#d-fixed) in the weekly report and set the row to `fixed` | Monday |
| 8 | ❌ *could not be tested* on one or a few pages | Already in the log as `down`? Then nothing today | Otherwise [see below](#a-page-could-not-be-tested) | Today |
| 9 | Any of these: ❌ on every page · *(fallback summary)* on every flagged page · *(error summary)* on any page · no digest an hour after the daily check time · a digest whose run number isn't in the Sheet | Has someone already told the operator? Look in the digest's thread | [Template E](#e-to-the-operator) to the operator, once | Today |
| 10 | 🚨 *PageSpeed Monitor failed at …* | | Paste the whole message to the operator. That also covers a missing digest | Today |
| 11 | *(fallback summary)* on only some pages | | Nothing extra. Use the results as normal | |
| 12 | The client says the page [looks fast to them](#the-client-says-it-looks-fast-to-me) | | Send [template G](#g-it-looks-fast-to-me). If they push back again, hand it to your lead | Today |
| 13 | The form says ["Nothing was checked"](#the-form-said-nothing-was-checked) | | Fix the addresses and try again | |

---

## Re-checking a page

Scores move a few points between checks, so before you report a new or worse problem, check it once more.

1. Reply in the original digest's thread: "Re-checking [url]". That stops a teammate re-checking the same page. If you have several pages to re-check, put them all in one form (up to 10).
2. Run them through the ["Check a page" form](guide.md#2-the-check-a-page-form).
3. Wait for the digest. It usually takes a few minutes and should never take more than 15. React 👀 on it so nobody else picks it up. If nothing arrives after 15 minutes, tell the operator when you sent it and which pages, and use tomorrow's check instead.
4. If the page is still 🔴 or 🟠, send, using the summary and run number from the re-check. If it dropped to 🟡 or passed, don't send. Say so in the original digest's thread.

If the page dropped on yesterday's re-check and it's 🔴 or 🟠 again today, don't re-check it a second time. Send.

Can't open the form? Tell the operator once. Until it's fixed, the next daily check is your re-check: send if the page is still 🔴 or 🟠. Don't use pagespeed.web.dev as a re-check, because it scores pages differently.

## A page could not be tested

❌ means the monitor got no usable result for the page, even after an automatic retry. It says nothing about speed. The page might be down, the address might be wrong, the site might block Google's tester, or the problem might be on the monitor's side.

1. Open the page in your browser.
2. If it doesn't load, try it on your phone on mobile data (not Wi-Fi) to rule out your own connection. Still down? Send the client contact [template F](#f-page-down) today and add a log row with risk `down`. Stop there.
3. If it loads, open [pagespeed.web.dev](https://pagespeed.web.dev/), paste the address and click Analyze. All you're finding out is whether Google can test the page. Ignore the score, because that site tests differently from the monitor.
4. Send the operator [template E](#e-to-the-operator) with the run number, one `error_message` from the `errors` tab, and whether pagespeed.web.dev worked. They'll work out whether the problem is the site or the monitor.

When a page logged as `down` comes back and shows 🔴 or 🟠, treat it as new (row 2).

→ Operator: [runbook: a page could not be tested](https://github.com/dexterloor/pagespeed-monitor/blob/main/docs/runbook.md#a-page-could-not-be-tested)

## Fallback summary

*(fallback summary)* means Claude, the AI that writes the summaries, wasn't available, so the monitor wrote a simpler one. The scores and risk are the same either way, because the monitor works out risk itself. Only the wording and the fix suggestions are plainer.

If only some pages have it, carry on as normal. If every flagged page in a digest has it, the AI step is probably down, so tell the operator once (row 9). *(error summary)* is different: only a placeholder was written, and the operator needs to know the first time you see it.

→ Operator: [runbook: Claude unavailable](https://github.com/dexterloor/pagespeed-monitor/blob/main/docs/runbook.md#claude-unavailable-fallback)

## No digest

Every check posts a digest, even when all pages pass (that's the ✅ line). If there isn't one, the check didn't run or broke partway. The usual cause is that the computer running the monitor was asleep or switched off.

The daily check runs at 07:00 on the operator's computer. Ask them what that is in your time zone and write it down. If there's no digest an hour after that, send template E. If you've already pasted a 🚨 message today, that covers it.

→ Operator: [runbook: the daily run didn't happen](https://github.com/dexterloor/pagespeed-monitor/blob/main/docs/runbook.md#the-daily-run-didnt-happen)

## The form said "Nothing was checked"

The form tells you which of these it was:
- The box was empty.
- None of the entries look like web addresses. Each one needs to start with `https://` (or `http://`) and have no spaces in it. Copying from your browser's address bar is the safest way.
- There were more than 10 addresses. Send 10 or fewer at a time.

One address per line works best, but spaces or commas between them are fine too. Repeats only get checked once. If only some entries were wrong, the form says "Check started" and lists the ones it skipped.

If the form page won't open at all, tell the operator. → [runbook: the form isn't loading](https://github.com/dexterloor/pagespeed-monitor/blob/main/docs/runbook.md#the-form-isnt-loading)

## The client says "it looks fast to me"

They're probably on a fast computer, with the page already saved in their browser. The mobile test copies a mid-range phone on a slow connection, which is a lot stricter.

Send [template G](#g-it-looks-fast-to-me). It has the main problems from the page's `root_causes` in `findings` (leave out the ms figures, which are Google's estimates, not promises) and the trend from `history`. One score can be a few points out, but three checks in a row saying the same thing are harder to argue with. If they still disagree, hand it to your lead and note it in the log's `notes`.

## The monitor itself failed

🚨 *PageSpeed Monitor failed at …* names the step that broke and ends with an Execution number, which is the run number. Paste the whole message to the operator and don't try to re-run anything. Once they say it's fixed, re-check any pages you need.

→ Operator: [runbook: error alert in Slack](https://github.com/dexterloor/pagespeed-monitor/blob/main/docs/runbook.md#error-alert-in-slack)

---

## Message templates

Copy one, fill in the brackets and send it. Client messages go the way you normally contact that client. Operator messages go as a reply in the digest's thread in `#pagespeed-alerts`, so everyone can see they've been sent.

### A. New problem
> Hi [name], our daily speed check has found a problem on [url].
> [summary from the digest]
> Suggested fix: [quick win from the digest]
> I can send the full details for your developer if that helps.

### B. It got worse
> Hi [name], an update on [url]: it's got worse since we flagged it on [first_sent from the log]. The mobile score is now [n], down from [mobile_score in the log].
> [summary from the digest]

### C. Weekly: still open
> Still open: [url], [risk], mobile score [n], first flagged [first_sent].

### D. Fixed
> No longer a problem: [url] (mobile score [n]). Thanks for sorting it!

### E. To the operator
> PageSpeed Monitor: [what you saw, e.g. "no digest today", "every summary is a fallback", "page couldn't be tested"]. Run [number, if there is one]. [error message or screenshot, if any]

### F. Page down
> Hi [name], [url] isn't loading for us. We checked on two different connections at [time]. Could you or your developer take a look?

### G. It looks fast to me
> Hi [name], our test copies a mid-range phone on a slow mobile connection, so it's stricter than a fast laptop that has visited the page before. Mobile score over the last 3 checks: [n], [n], [n]. The main causes: [the first 2 or 3 root causes, without the ms figures]. Happy to send the full list for your developer.

---

## The escalation log

It's the `escalations` tab in the PageSpeed Monitor Sheet. Keep one row per page: add it when you send template A or F, and edit that same row when you send B or D. If the tab isn't there yet, ask the operator to add it.

| Column | What goes in it |
|---|---|
| `first_sent` | The day you first sent anything about this page |
| `last_sent` | The day you last sent A, B, D or F |
| `url` | The page, copied from the digest |
| `risk` | Its risk when you last sent, or `down` for template F |
| `mobile_score` | The mobile score when you last sent |
| `sent_to` | Who you sent it to |
| `run_id` | The run you last saw it in (your re-check, if you did one) |
| `status` | `open`, `updated` or `fixed` |
| `notes` | Anything useful, e.g. "client says fix planned for next month" |

A page counts as in the log if its row says `open` or `updated`.
