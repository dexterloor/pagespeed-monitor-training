# Self-check

*Eight questions to check you're ready, then a practice list for your first week. Answers are hidden: click to open them.*

← Back to the [guide](guide.md)

---

## Questions

**1. It's an hour after the daily check time and there's no digest in `#pagespeed-alerts`. Is something wrong?**

<details><summary>Answer</summary>

Yes. The monitor posts a digest after every check, even when every page passes (a single ✅ line). No digest means the check didn't run or broke. Tell the operator with template E, unless a teammate already has (check `#pagespeed-alerts`). ([No digest](what-to-do.md#no-digest))
</details>

**2. A digest line says `🟡 https://example.com/: low risk · mobile 94 · desktop 99`. Both scores are above 90. Why was it flagged, and what do you do?**

<details><summary>Answer</summary>

A page is also flagged when one fix could save at least 0.1 seconds, even with good scores. You do nothing: low risk is never reported on its own. ([Flagged](glossary.md#flagged), [table row 6](what-to-do.md#the-table))
</details>

**3. The `history` tab shows `performance` = `0.42`. What would Slack show, and what risk is that?**

<details><summary>Answer</summary>

Slack shows **42**: the Sheet uses 0–1, Slack uses 0–100. Below 50 is `high` risk. ([Reading the Sheet](reading-the-sheet.md#before-you-start-two-things-that-trip-people-up))
</details>

**4. A digest shows `❌ https://shop.example.com/: no PageSpeed data (6 failed runs)`. Is the page slow?**

<details><summary>Answer</summary>

You can't tell. ❌ means the page **could not be tested**. Open it in your browser.
- It doesn't load, on two connections: tell the client contact (template F).
- It loads: try it at pagespeed.web.dev, then send the operator the run number, one `error_message` and what pagespeed.web.dev did.

([A page could not be tested](what-to-do.md#a-page-could-not-be-tested))
</details>

**5. A 🔴 page shows *(fallback summary)*, and it isn't in the escalation log. Should you wait for a "proper" result before reporting it?**

<details><summary>Answer</summary>

No. The scores and the risk are the same whoever writes the summary. Re-check it once, as with any new 🔴, and report it if it's still 🔴 or 🟠. Tell the operator only if **every** flagged page in the digest shows *(fallback summary)*. ([Fallback summary](what-to-do.md#fallback-summary))
</details>

**6. You pasted `www.example.com` into the form and got "Nothing was checked". Why?**

<details><summary>Answer</summary>

Addresses must start with `https://` (or `http://`). Copy the address from your browser's address bar: `https://www.example.com/`. ([The form said "Nothing was checked"](what-to-do.md#the-form-said-nothing-was-checked))
</details>

**7. A page has been 🟠 `medium` every day for three weeks. The log shows you reported it on day one. Today it's 🔴. What do you do?**

<details><summary>Answer</summary>

The risk is higher than in the page's log row, and `last_sent` was three weeks ago. Re-check it once. If it's still 🔴, send template B and edit the page's existing row: new `last_sent`, `risk` and `mobile_score`, status `updated`. If the risk hadn't changed, it would only go in Monday's weekly report. ([Table rows 4 and 5](what-to-do.md#the-table))
</details>

**8. Slack shows 🚨 *PageSpeed Monitor failed at Build reports*. Which page has the problem?**

<details><summary>Answer</summary>

None. That message means **the monitor itself** broke, not a web page. Paste the whole message, including its Execution number, to the operator. Don't try to re-run anything. ([The monitor itself failed](what-to-do.md#the-monitor-itself-failed))
</details>

---

## First-week practice

Do these once in your first week. Each takes under 10 minutes. If you can't open the form from your computer, do the form tasks with the operator, on their screen.

- [ ] **Run a check.** Use the form to check one fast page (`https://www.wikipedia.org/`) and one heavy page (`https://www.theverge.com/`) together. Find the digest in Slack. Did both pages get a block? For each one, say why, using the risk and the scores. (Results vary from day to day: even Wikipedia is sometimes flagged.)
- [ ] **Match a run.** Take the run number from the digest footer. Find its rows in `history` and `findings` by searching for it.
- [ ] **Read a trend.** Pick one page from the daily list. In `history`, filter to that page and `mobile`. Has its score gone up, down, or stayed the same this week?
- [ ] **Read a root cause.** Open one `findings` row. For its first root cause, say in your own words: what's wrong, which device, roughly how much time Google estimates it could save, and how big the fix is.
- [ ] **Test a mistake.** Send the form `example.com` (no `https://`) and read the message. Then send `https://www.wikipedia.org/` and `not-a-page` together, and find which one was skipped.
- [ ] **Practise an escalation.** Pick a real 🔴 or 🟠 page. Go through table row 2: check the log, post "Re-checking" in the digest's thread, re-check the page, fill in template A, and write the log row. Ask your lead to check them before anything is sent.

When you've done these, you're ready to handle the digest on your own.
