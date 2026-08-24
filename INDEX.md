# Fresh Off the Crown · Catalog

> *Does this capability earn a place in how I actually work?*

A daily AI tooling practice run as a decision log. Each build points at a real friction, records what was actually run, and ends in one verdict with a reversal trigger. This catalog lists what has shipped so far.

## Systems

| # | System | Onboarded | Last updated | What it does |
|--:|---|---|---|---|
| 1 | [Write a Grok Bot Skill Twice](./systems/grok-bot-skill-contract/) | 2026-08-23 | 2026-08-23 | A controlled A/B of a four-field against a six-field Grok Bot curation skill over one batch carrying two planted failures and two borderline cases. Verdict WATCH: both versions scored 0 of 2, but neither received the batch, so the equal counts are no exposure rather than correct rejections. Re-check 2026-08-29. |
| 2 | [Measure What Concise Drops](./systems/measure-what-concise-drops/) | 2026-08-23 | 2026-08-23 | Six prompts and 18 required items locked before any run, measuring whether a shorter Claude Code output style drops facts. Verdict ADOPT for a custom style: Default 37 lines to Concise 22, a 41% cut with 18/18 retained by both, and the custom style kept two unscored details that change the next action. Untested: caveats and refusals. |
| 3 | [Three Layers, One Model](./systems/default-model-precedence/) | 2026-08-24 | 2026-08-24 | Four Claude Code model-selection layers set to disagree, then peeled one at a time to observe the fallback order. Verdict ADOPT for `--model` as the control surface, proven by the one unconfounded run where haiku beat a set ANTHROPIC_MODEL. Unresolved and stated: settings.json against ANTHROPIC_MODEL was never run. Re-check 2026-08-30. |

Target: 100.
