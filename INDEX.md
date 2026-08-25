# Fresh Off the Crown · Catalog

> *Does this capability earn a place in how I actually work?*

A daily AI tooling practice run as a decision log. Each build points at a real friction, records what was actually run, and ends in one verdict with a reversal trigger. This catalog lists what has shipped so far.

## Systems

| # | System | Onboarded | Last updated | What it does |
|--:|---|---|---|---|
| 1 | [Write a Grok Bot Skill Twice](./systems/grok-bot-skill-contract/) | 2026-08-23 | 2026-08-23 | An A/B of a four-field against a six-field Grok Bot curation skill over one batch of planted failures, where both scored 0 of 2 because neither ever received the batch. Verdict WATCH, re-check 2026-08-29. |
| 2 | [Measure What Concise Drops](./systems/measure-what-concise-drops/) | 2026-08-23 | 2026-08-23 | Six prompts and 18 required items locked before any run, measuring whether a shorter Claude Code output style drops facts. A 41% line cut with nothing scored lost. Verdict ADOPT for a custom style. |
| 3 | [Three Layers, One Model](./systems/default-model-precedence/) | 2026-08-24 | 2026-08-24 | Four Claude Code model-selection layers set to disagree, then peeled one at a time to find which one wins. Verdict ADOPT for the --model flag, with the middle of the order left unresolved. |
| 4 | [Probe Paper's Comments with an Agent](./systems/paper-comment-instructions/) | 2026-08-24 | 2026-08-24 | Three canvas comments under three conditions, testing whether an agent over MCP honours resolved state. Verdict ADOPT for comment state as a control surface, proven by an isolated reset run rather than by the hollow 3 of 3 canvas score. |

Target: 100.
