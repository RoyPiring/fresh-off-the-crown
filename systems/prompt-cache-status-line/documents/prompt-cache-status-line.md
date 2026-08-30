<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# Measure Claude Code Prompt Cache

**Project Link:** [View Project](https://nextwork.ai/projects/a4297952-cc45-4f89-acaf-a5ff9d6d798f)

**Author:** Roy Piring: Cloud Platform Engineer | Build Master  
**Email:** rpiringhawaii@gmail.com

---

![Image](https://nextwork.ai/refreshed_maroon_timid_jujube/uploads/a4297952-cc45-4f89-acaf-a5ff9d6d798f_2g8x2h0j)

## Proving Status-Line Accuracy with Same-Moment Evidence

### Capturing evidence from the same moment

I captured the current terminal output and the frozen status-line script reading without allowing another model turn between them. This created an atomic comparison where both surfaces represented the same Claude Code session state.

The timing mattered because prompt-cache counters can change after any new response, compaction, or request. A terminal reading taken before another turn cannot be compared fairly with a script reading taken afterward. Both values may be correct for their moments while still failing an equality check.

I used the frozen pair to compare five fields and record whether each value agreed. This made the report about schema mapping and same-state equality rather than visual similarity. It also created evidence that could be reviewed later without rerunning the session, when the live counters would already represent a newer state.

![Image](https://nextwork.ai/refreshed_maroon_timid_jujube/uploads/a4297952-cc45-4f89-acaf-a5ff9d6d798f_2g8x2h0j)

### Five-field equality report and finding

The first equality result was 0/5 after a complete search across /cost, /usage, /status, /context, and /stats. Each command contained zero occurrences of the five requested prompt-cache fields, so the result did not come from overlooking another terminal reading.

The two mismatches in the second report came from different aggregation windows. One surface showed cumulative rather than per-request values, while another compared billed tokens with context tokens. Those numbers described different scopes, so their disagreement did not prove that either source was wrong.

Cache write matched exactly to the token. That was the strongest cross-surface confirmation because the terminal output and prompt_cache object reported the same cache activity. The finding showed that equality requires matching both the field definition and the measurement window, not only labels that appear related.

### Comparing expected rebuilds across runs

The expected_rebuilds value increased from 1 to 4 between phase 1 session c65f8f04 and phase 2 session e8d9fdc8. The difference was 3, with each increment representing another conversation summary caused by compaction.

Phase 2 entered an autocompact loop. The client warned that context had returned to its limit within 3 turns of the previous compact, and that warning occurred 3 times in a row. Compaction then occurred on nearly every turn, moving expected_rebuilds from 2 to 3 and then 4.

Across both runs, misses remained at 1 while expected_rebuilds increased. That separation showed the field behaving as intended. Rebuilds caused by compaction were attributed to expected_rebuilds instead of being reported as unexplained cache misses. The comparison therefore preserved the cause of repeated cache construction.

![Image](https://nextwork.ai/refreshed_maroon_timid_jujube/uploads/a4297952-cc45-4f89-acaf-a5ff9d6d798f_58t48p55)

## Correcting the Five-Field Status Line

### Fixing the status-line reading

I corrected the property paths in the PowerShell status-line script after inspecting the raw prompt_cache object. The first draft used human-facing labels and root-level fields that did not exist inside that object, so every lookup returned null.

The corrected script read the five paths that were present in the captured schema. This changed the status line from five dashes to populated values for request count, hit ratio, miss count, expected rebuilds, and tokens re-cached.

The correction was grounded in observed JSON rather than another round of guesses. I then ran the same script in PowerShell 7 and PowerShell 5.1 and compared its output with .claude/prompt-cache-reading.txt. Both shells produced the same rendered line, and the saved reading matched byte for byte. The status line therefore reached a verified 5/5 schema score.

### Verified prompt_cache fields

The corrected script used prompt_cache.requests, prompt_cache.hit_ratio, prompt_cache.misses, prompt_cache.expected_rebuilds, and prompt_cache.miss_recache_tokens. All five paths appeared in the captured payload, producing a 5/5 schema-grounding result.

Two values required display formatting. The script multiplied hit_ratio by 100 and rounded it to a whole percentage, converting 0.8101449… to 81%. It also expressed miss_recache_tokens in k or m units, converting 48154 to 48.2k.

The final line rendered requests=10 | hit_ratio=81% | misses=1 | expected_rebuilds=0 | tokens re-cached=48.2k. PowerShell 7 and PowerShell 5.1 returned identical output, and both matched .claude/prompt-cache-reading.txt byte for byte.

![Image](https://nextwork.ai/refreshed_maroon_timid_jujube/uploads/a4297952-cc45-4f89-acaf-a5ff9d6d798f_nad6ahge)

## Measuring Timing and Rebuild Behaviour

### Controlling timing for comparison

I controlled the conversation flow by triggering a specific model turn and watching how the request and rebuild counters changed. The test showed why a terminal reading becomes stale as soon as another response updates the active session.

A valid equality comparison required both surfaces to be captured before any new model turn, compaction, or request occurred. If one value came from the earlier state and the other from the later state, a mismatch would measure timing rather than an incorrect property path.

I treated the session state as part of the evidence, not background context. The counter values, timestamp, session ID, and compaction phase identified the moment being measured. This allowed me to separate a live reading from a later payload value and preserve both without treating them as contradictory. Atomic capture was therefore required for the five-field equality score.

![Image](https://nextwork.ai/refreshed_maroon_timid_jujube/uploads/a4297952-cc45-4f89-acaf-a5ff9d6d798f_zcjdm157)

### Expected rebuild measurement

I recorded expected_rebuilds=1 live at the post-compaction REBUILT. turn in session c65f8f04 at 12:38:36. The full reading was requests=3 | hit_ratio=20% | misses=1 | expected_rebuilds=1 | tokens re-cached=69.8k.

The field moved from 0 to 1 at the compaction event while misses stayed at 1. This showed that the cache reconstruction was attributed to compaction instead of being added as an unexplained miss.

The payload file later showed expected_rebuilds=2 after another automatic compaction at 12:39:15. That later value represented a newer session phase and did not invalidate the earlier live reading. The append-only record in notes/evidence/post-compaction-reading.md preserved the original value with its moment. Without that history, the updated payload would have overwritten the evidence needed for the first measurement.

## Scoring a Schema-Grounded Draft

### Preserving published labels for evaluation

I backed up the existing settings and created an initial status-line draft before inspecting the correct prompt_cache paths. The draft used label-based guesses so I could score how well human-facing names mapped to the actual JSON object.

I preserved the uncorrected draft instead of rewriting it after viewing the schema. Changing the original guesses would have removed the baseline and made the later 5/5 correction look correct from the start. The preserved version showed what the initial mapping produced without hindsight.

I also extracted the raw payload to a local file and compared each guessed path with the prompt_cache object. This turned the score into a path-presence test rather than an opinion about whether the labels sounded reasonable. The backup protected the previous configuration, while the raw payload provided the authority for grading all five guesses.

### Schema-grounding score and missing paths

The initial draft scored 0/5 under prompt_cache. Its five guessed paths were workspace.current_dir, model.display_name, session_id, version, and output_style.name. None existed inside the prompt_cache object.

A missing path did not crash the script or produce a visible error. The extractor resolved the lookup to null and printed the label followed by a dash. The resulting status line looked like five empty cache values even though the actual problem was that the script was reading the wrong location.

The zero score measured a schema and vocabulary mismatch, not unreasonable field names. Those same paths existed at the payload root and would score 5/5 there. The corrected script instead used the five verified fields under prompt_cache and scored 5/5 within the intended object. This distinction separated wrong scope from absent data.

![Image](https://nextwork.ai/refreshed_maroon_timid_jujube/uploads/a4297952-cc45-4f89-acaf-a5ff9d6d798f_5rg82qu0)

## Preparing an Authenticated Claude Code Environment

### Preparing the local environment

I prepared the authenticated Claude Code environment and captured the raw JSON payload before correcting the status line. The goal was to compare the label-based draft with the actual prompt_cache schema under a known client and workspace configuration.

I saved the payload to ~/.claude/prompt-cache-payload.json and inspected the property paths directly. This gave the five-field score a fixed evidence source instead of relying on documentation, memory, or labels displayed elsewhere in the client.

The environment preparation also separated the global Claude Code installation from the workspace-local version required by the build. I left the global binary and existing settings untouched, installed the target version locally, and preserved the prior settings file in a backup. These controls reduced the chance that version drift or an untracked configuration change would alter the schema comparison.

### Environment and existing status-line configuration

The environment was Windows 11 Pro running PowerShell 7, not macOS Terminal. In this setup, ~/.claude resolved to C:\Users\rpiri\.claude. The existing settings.json file was 500 bytes and remained in place.

The file contained a Stop hook, the paper-desktop plugin, a marketplace entry, theme settings, and tui keys. A search returned zero statusLine matches, confirming that no status line was configured through settings.json before this build.

An unreferenced statusline.js file already existed in the same folder, but no active setting pointed to it. I left that file unchanged. I also backed up the original settings file to backups/settings.json.pre-statusline.bak before adding the new configuration. This preserved the prior local state and made the status-line change recoverable.

### Version checks before and after preparation

The first claude --version check used the binary on PATH and returned 2.1.241 (Claude Code). That executable came from the global native installation at /c/Users/rpiri/.local/bin/claude and was ten patch versions behind the required 2.1.251 target.

After preparation, ./node_modules/.bin/claude --version returned 2.1.251 (Claude Code). This executable came from the workspace-local installation and was pinned exactly in package.json.

Both versions remained installed side by side. I deliberately left the global 2.1.241 binary unchanged while using the local command for the build. This satisfied the version requirement without changing unrelated Claude Code sessions. Recording the executable paths also prevented a successful version check from hiding which installation had produced it.

![Image](https://nextwork.ai/refreshed_maroon_timid_jujube/uploads/a4297952-cc45-4f89-acaf-a5ff9d6d798f_hfh0n89e)

## Investigating Claude Code Status-Line Reliability

### Project goals and requirements

I compared five requested labels with the property names in the prompt_cache object: Request count, Hit ratio, Miss count, Expected rebuilds, and Tokens re-cached. I first generated a label-only status-line draft, then extracted the raw JSON payload to ~/.claude/prompt-cache-payload.json.

The initial draft scored 0/5 under prompt_cache because none of its guessed paths existed inside that object. The script did not fail visibly. Each null lookup rendered as a dash, which could be mistaken for empty cache data instead of an incorrect path.

I corrected the script using the five fields found in the captured schema and reached 5/5. I then compared the rendered values across PowerShell versions and same-moment evidence. The build tested path accuracy, formatting, timing, and measurement scope rather than assuming a plausible label proved a valid reading.

## Reflecting on the Technical Journey

### Tools and concepts learned

I used the workspace-local Claude Code 2.1.251 client, a custom PowerShell status-line script, and the captured prompt_cache JSON payload. These tools let me inspect the source schema, render selected fields, and compare the result with terminal evidence.

The main lesson was that visible labels must be mapped to verified JSON paths. A script can return blanks without reporting an error when a path is wrong, making missing data and incorrect lookup logic appear identical.

I also learned why model turns must be frozen during an equality capture. Request and rebuild counters can change immediately after another response or compaction. Comparing values from different moments can create false mismatches even when both readings are correct. Session IDs, timestamps, append-only evidence, and matched aggregation windows made the final comparison defensible.

### Project timeline and challenge

This build took approximately 45 minutes. The hardest part was keeping the terminal output and status-line reading synchronized while capturing the five comparison fields. Any model turn between those readings could change requests, expected rebuilds, hit ratio, or tokens re-cached.

The later payload demonstrated that risk. expected_rebuilds changed from 1 at 12:38:36 to 2 after another automatic compaction at 12:39:15. Both values were valid, but they described different session moments.

I controlled the comparison by freezing model activity and preserving the earlier reading in an append-only evidence file. I also checked aggregation scope before calling a difference contradictory. These controls allowed the corrected status line to reach 5/5 without treating stale and current data as if they came from one state.

### Next learning goals

I completed this build to learn how to verify two data surfaces through a side-by-side evidence comparison. The corrected PowerShell status line read five verified prompt_cache fields and produced the same output in PowerShell 7 and PowerShell 5.1.

The experiment also showed that schema accuracy alone is not enough. The terminal and script must represent the same session moment and aggregation window. Otherwise, valid values can appear to conflict because one is cumulative, per-request, billed, contextual, or simply newer.

My next goal is to build similar diagnostic scripts for other CLI tools. I want those scripts to capture raw payloads, verify property paths, preserve timestamps, and compare visible output with local readings. The same method can expose wrong paths, silent nulls, stale evidence, and measurement-scope differences.

---

*Built with [NextWork](https://nextwork.ai) - [View this project](https://nextwork.ai/projects/a4297952-cc45-4f89-acaf-a5ff9d6d798f)*
