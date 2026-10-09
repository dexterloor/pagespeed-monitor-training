# PageSpeed Monitor: training pack

Training for the people who **receive** the [PageSpeed Monitor](https://github.com/dexterloor/pagespeed-monitor)'s alerts: virtual assistants and account staff with no developer background.

The monitor checks web pages once a day (and on demand) with Google PageSpeed Insights, has Claude explain what's wrong in plain English, logs everything to a Google Sheet and posts a Slack digest. This pack teaches someone in their first week how to read those results, decide what they mean, and pass them to the right person.

> 🎥 Loom walkthrough: _link to come_

## Start here

**[guide.md](guide.md)**: the one-page guide. If you read only one thing, read this.

| File | Read it when… |
|---|---|
| [guide.md](guide.md) | You're new. Everything in one page |
| [reading-the-digest.md](reading-the-digest.md) | You want to understand every line of the Slack message |
| [reading-the-sheet.md](reading-the-sheet.md) | You need the detail, the trend, or to check that a run happened |
| [what-to-do.md](what-to-do.md) | You've seen something and want to know what to do and who to tell |
| [glossary.md](glossary.md) | A word doesn't make sense |
| [self-check.md](self-check.md) | You want to test yourself, plus a first-week practice list |

## How this fits with the other docs

One tool, four documents, each for a different reader. They link to each other and use the same words for the same things ([glossary](glossary.md)).

| Document | For | Answers |
|---|---|---|
| **This training pack** | VAs and account staff who receive the alerts | "What does this alert mean, and what do I do about it?" |
| [Runbook](https://github.com/dexterloor/pagespeed-monitor/blob/main/docs/runbook.md) | The operator who keeps the monitor running | "Something's broken or odd. How do I fix it?" |
| [Incident write-ups](https://github.com/dexterloor/pagespeed-monitor/tree/main/docs/incidents) | Engineers | "How did a real bug get found and fixed?" |
| [README](https://github.com/dexterloor/pagespeed-monitor#readme) | Developers | "How does it work, and how do I set it up?" |

The hand-off is explicit: every row in [what-to-do.md](what-to-do.md) says when to pass something on, and the operator rows link to the matching runbook section.

## How it was written

- **Facts come from the code, not from memory.** Thresholds, risk rules, digest wording and Sheet columns were checked against the monitor's source (`scripts/lib/`, `prompts/analyze-report.md`, `scripts/build-workflows.js`). The example summary and root causes are real Claude output for a public page.
- **Plain words, short sentences.** Every technical term is in the [glossary](glossary.md), and the reader is never asked to run a command.
- **The reader looks things up instead of judging.** [what-to-do.md](what-to-do.md) is one table: what you see, what to check, what to send, and when. Every message has a template, and an escalation log records what has been sent, so a VA in their first week never has to decide how serious something is.
- **Built for a VA team.** VAs report to the client contact; problems with the monitor go to the operator. 👀/✅ reactions show who's handling each digest. "Today" means "before the end of your shift", because teams work across time zones.
- **Reviewed adversarially.** A second reviewer checked every claim against the code and attacked the escalation rules. That review led to two changes in the monitor itself: it now posts a ✅ line when every page passes, so silence always means something broke, and it works out risk in code instead of trusting the AI's grading.
- **Practice, not just reading.** [self-check.md](self-check.md) has questions based on real situations and a first-week exercise using the real tools.

## License

MIT
