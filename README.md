# Fresh Off the Crown

[![License: MIT](https://img.shields.io/badge/license-MIT-1B4332?style=flat-square&labelColor=0d1117)](./LICENSE) [![Practice](https://img.shields.io/badge/AI%20Tooling-Practice-C2185B?style=flat-square&labelColor=0d1117)](https://github.com/RoyPiring) [![Verdicts](https://img.shields.io/badge/verdicts-1%20%2F%20100-2F5233?style=flat-square&labelColor=0d1117)](./INDEX.md) [![Updated](https://img.shields.io/badge/updated-2026--08--22-264653?style=flat-square&labelColor=0d1117)](./INDEX.md)

> *Does this capability earn a place in how I actually work?*

This is a decision log with a build attached. Every entry answers that one question, and the build exists to make the answer honest, because a verdict formed by reading is a guess and a verdict formed by running is evidence.

The AI tooling landscape produces more real, interesting, genuinely good news than any working engineer can act on. Most of it is not actionable, and treating it as though it were is how a practice turns back into reading. This repo is the filter I run so that keeping current stays a habit instead of a second job.

## What this is not

It is not a tutorial collection and it is not a newsletter. There is no publishing obligation here, because the moment there is one, the quality bar becomes the thing that gives. What ships is what survived the filters. On a day when nothing qualifies, nothing ships, and that is a recorded outcome rather than a gap.

## The unit is a friction, not a feature

Every entry points at a real friction in how the work actually gets done, and records what that friction cost **before**. The measured result is an **after**. It either moved or it did not.

This is deliberate, and it replaced an earlier design that tracked feature coverage. A coverage row can be marked as used by using the feature once, which measures surface touched rather than work made better. A year of that produces someone who has touched everything and made nothing better.

## Two filters, and neither is a judgment call

Selection is mechanical on purpose, so that staying current does not depend on having taste on a Tuesday morning.

**The problem join.** A candidate that matches no real friction on the list does not enter the queue. Not deprioritized, does not enter. This is what keeps benchmark leaderboards and funding news out without anyone having to argue about them. It also makes the work compound, because a friction stays on the list until something closes it.

**The first-party test.** On the day a thing is selected, is the code that would actually be run written and shipped by the same organization that published the news, and does it run locally under a flat-rate tool already paid for?

That second filter comes from a measurement, not a preference. When a specification's own organization ships the SDK, it arrives in a median of **7 days**. When a build needs a third party to catch up, the median is **204 days**, and in six tracked cases none arrived inside six months. At one day old, the only buildable surface is the thing the publisher shipped themselves.

A candidate failing the first-party test is **parked**, never discarded. It names the tool it waits on and a re-check date, which turns the queue into a ranked catalog with a do-first order. The day the awaited tool ships is a better anchor than the original announcement was, because by then something exists to build against.

## Two lanes, and the default is the one that never runs dry

The most common way a daily practice dies is a production quota meeting an event-driven source. On a day when nothing shipped, a quota forces a reach for the nearest thing that looks like a signal, and that is how something dead in six weeks enters by process rather than by taste.

| | **Known** (the default) | **New** (the interrupt) |
|---|---|---|
| Supply | Capabilities already inside tools in daily use | What shipped since the last run |
| Availability | Deep and predictable | Rare and unpredictable |
| First-party | Always, by construction | Sometimes |
| Needs a card | Never, already paid for | Often, and then it is refused |

News is a bonus, not the engine. Working the known lane also teaches the shape of each ecosystem, so when something genuinely new does land it can be judged in minutes instead of an afternoon.

## What every entry owes

- **A friction** it joins to, with a measured before
- **A run record**: the date, the tool and the version actually installed, the command actually run, and the measured result
- **A verdict**, exactly one
- **A reversal trigger**: the one line saying what would change the decision

## The four verdicts

| Verdict | Means | Owes |
|---|---|---|
| **ADOPT** | It is going into the working system | The change actually made |
| **REJECT** | It is not | A reason specific enough not to re-litigate in six weeks |
| **WATCH** | Not yet, but plausibly later | A re-check date, which jumps the queue ahead of any news |
| **PARK** | Failed the first-party test | The tool it waits on, and a re-check date |

**A build succeeds when the working setup changed or a watch resolved.** If neither happened, it was reading, and it is logged as reading. That is an honest category, not a failure.

The reversal triggers are the artifact worth keeping. A ledger of verdicts tracks tools; the frictions and the triggers track the operator, and the friction list is the one that gets shorter.

## Cadence

**The research run is daily. The build is conditional, and capped at 30 minutes.** Reading release notes every day is the habit; only the build is earned. A day with no qualifying candidate produces a queue entry and no build, which is a normal and healthy outcome.

Missing days costs a row, not a debt. A practice that accrues perishable obligations meets a real working rhythm and loses.

## Honest limits

- **A before-number can be invented and no check can tell.** A measured friction is a stronger unit than a coverage mark, and it is still only as good as the honesty of the person writing it. The only reader is the operator, and lying to yourself in a file only you read has no audience to fool.
- **A day-old release usually has no support policy yet.** No lifecycle page, no end-of-life date. Maturity is declared per entry, because a withdrawn preview is the failure this practice is most exposed to.
- **The known lane is deep but finite.** It grows when the tools ship, not when the practice runs.

## The ledger

**1 of 100 logged.** One verdict at a time, at most one a day, on a run to 100 recorded decisions. The count moves only when a run actually happened: a day with no qualifying candidate is a recorded outcome rather than a gap, and it never gets backfilled to keep a streak alive. What matters is that the run happened, the verdict is honest, and the friction list is shorter than it was. Browse what has shipped in [`INDEX.md`](./INDEX.md).
