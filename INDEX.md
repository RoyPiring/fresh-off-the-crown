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
| 5 | [Find the Wall](./systems/subagent-nesting-wall/) | 2026-08-25 | 2026-08-25 | A four-level agent chain run under a budget brake to locate Claude Code's subagent nesting cap, which sits after three links. Verdict ADOPT for CLAUDE_CODE_MAX_SUBAGENT_SPAWN_DEPTH, which moved the wall to one without disabling subagents. A blocked level still exits 0. |
| 6 | [Which Browser Did Claude Just Open?](./systems/browser-surface-routing/) | 2026-08-27 | 2026-08-27 | Whether the Preferred browser setting actually routes a web task between the built-in pane and local Chrome. Verdict WATCH: both runs opened the built-in pane, but the reversal was void because the baseline had already used it, so the setting remains unproven. Re-check needs Chrome selected in a fresh session. |
| 7 | [Measure Your Own Word Error Rate](./systems/personal-word-error-rate/) | 2026-08-27 | 2026-08-27 | A personal Word Error Rate for CLI dictation, measured against a sealed reference read once and scored with jiwer. Verdict WATCH at 16.5% against a published 4.0%. The sensitivity check refutes the obvious story: capture conditions moved the rate 7.7 points where vocabulary moved it 1.1. |
| 8 | [Package a Claude Code Workflow](./systems/packaged-workflow-payoff/) | 2026-08-30 | 2026-08-30 | A nine-action workflow packaged into a local Claude Code plugin, measured on both action count and elapsed time. Verdict ADOPT for repeatable execution and safe refusal, not speed: actions fell 9 to 4, a 56% cut, while the clock moved 00:46 to 00:45. |
| 9 | [Measure Claude Code Prompt Cache](./systems/prompt-cache-status-line/) | 2026-08-30 | 2026-08-30 | A five-field prompt-cache status line corrected from the captured JSON schema rather than from labels. Verdict ADOPT at 5 of 5 grounding, identical across PowerShell 7 and 5.1, with same-moment capture required because counters move on any turn. |

Target: 100.
