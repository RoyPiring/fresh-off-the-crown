<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# Find the Wall

**Project Link:** [View Project](https://nextwork.ai/projects/5baa3c06-1d0a-4f06-82f3-ce1629b76aa8)

**Author:** Roy Piring: Cloud Platform Engineer | Build Master  
**Email:** rpiringhawaii@gmail.com

---

![Image](https://nextwork.ai/refreshed_maroon_timid_jujube/uploads/5baa3c06-1d0a-4f06-82f3-ce1629b76aa8_awol0jie)

## Understanding the Experiment

### Project goals and motivation

I tested Claude Code's subagent nesting depth to determine whether the default wall appeared after 3 subagent links or whether the chain stopped elsewhere. The test requested 4 levels and used file artifacts to show which agents ran. I also examined whether the blocked level produced a visible failure or disappeared while the command still exited successfully.

Before running anything, I created a controlled directory structure, defined one depth-writer agent, and recorded my predictions. Each agent was expected to write its depth to a log and then request one child. This made the chain observable without relying only on Claude's summary. The goal was to locate the wall, identify its refusal shape, and test whether an environment variable could move it.

### Setting up the environment

I created a dedicated workspace and stored the test configuration at the build level rather than changing my global Claude Code settings. This isolated the depth-writer agent, local settings, logs, and generated files from unrelated work on the machine.

The depth-writer definition followed one repeatable rule: record the current depth, stop when N == M, or spawn one child for the next level. Using the same agent at every link reduced the number of changing parts in the test.

I also checked the local configuration before each run. A lingering environment override or modified depth-writer.md file could make the chain stop early for reasons unrelated to the default wall. Isolation made each result easier to trace to the tested setting.

![Image](https://nextwork.ai/refreshed_maroon_timid_jujube/uploads/5baa3c06-1d0a-4f06-82f3-ce1629b76aa8_kaws9sll)

### Why the budget brake matters

Recursion created the main risk. Every depth-writer started another full model with its own context. A faulty stop rule or an agent that spawned siblings instead of one child could turn a linear chain into geometric growth.

I set --max-budget-usd 2.00 as a hard ceiling for the full invocation. The limit covered the parent and every nested descendant rather than granting $2 to each agent. If the combined spend reached the cap, the run would terminate.

The budget brake made a failed depth test cheaper and easier to diagnose. A controlled stop with a log was safer than discovering uncontrolled recursion through the billing page. This mattered because the build intentionally pushed nesting until either the platform or the budget stopped it.

## Forming Predictions Before Running the Chain

### Planning the prediction exercise

I wrote my predictions before starting the 4-level request. This prevented me from reshaping the expected behavior after seeing the log, summary, or exit code.

The prediction record covered three separate questions. First, I predicted which depth files would exist. Second, I predicted how Claude Code would represent the blocked child request. Third, I predicted what would happen when CLAUDE_CODE_MAX_SUBAGENT_SPAWN_DEPTH was set to 1.

Separating those claims mattered because one run could confirm the wall while disproving the expected refusal shape. The artifact, agent summary, tool availability, and process exit code each measured a different part of the behavior and needed to be scored independently.

![Image](https://nextwork.ai/refreshed_maroon_timid_jujube/uploads/5baa3c06-1d0a-4f06-82f3-ce1629b76aa8_ztd3j82s)

### Three predictions and the reasoning behind them

My first prediction was that depths 1, 2, and 3 would write successfully, while depth 4 would be refused. I expected the default cap to count subagent links beneath the main session rather than counting the session itself.

My second prediction was that the refusal would appear as a failed tool call at depth 3, but the overall command would still exit 0. I expected the run summary to hide or soften that failure.

My third prediction was that setting CLAUDE_CODE_MAX_SUBAGENT_SPAWN_DEPTH to 1 would disable further nesting without disabling the first subagent. Under that condition, depth 1 should still write, while depths 2 through 4 should not run.

## Crossing the Nesting Wall and Reading the Evidence

### Running the depth chain

I ran the depth-writer chain with a target of 4 levels and left the default nesting configuration in place. Each successful agent was expected to append one line to the log before asking for the next child.

After the command completed, I inspected the log, checked for any partial depth 4 artifact, read Claude's summary, and recorded the process exit code. These checks separated completed work from reported work.

The expected wall was between depth 3 and depth 4. If the platform enforced the cap there, the log would contain exactly three entries. A fourth entry would show that the default allowed the full request. Fewer than three entries would suggest a setup failure, altered agent definition, or another active limit rather than the expected nesting boundary.

![Image](https://nextwork.ai/refreshed_maroon_timid_jujube/uploads/5baa3c06-1d0a-4f06-82f3-ce1629b76aa8_gi76thbs)

### Comparing the artifact to Claude's summary

The log contained exactly three lines for depths 1, 2, and 3. Depth 4 was absent, and no partial write existed. The artifact therefore placed the wall after the third subagent link.

Claude's summary stated that depth 4 never ran because the Agent tool was unavailable at depth 3. That description matched the file evidence. The command still exited 0, so the blocked child did not become a process-level error.

I had predicted that the summary would hide the failure. That part was wrong. The summary identified the missing Agent tool, and independent inspection confirmed its statement. The useful lesson was still to check the artifacts because an accurate summary is supporting evidence, not a substitute for the files created by the run.

## Testing the Environment Variable

### Designing the variable test

I ran the same depth-writer request again with CLAUDE_CODE_MAX_SUBAGENT_SPAWN_DEPTH set to 1. The agent definition, requested target depth, logging behavior, and budget ceiling remained unchanged.

The test asked whether the variable moved the wall or was ignored. If honored, only depth 1 should write because the first subagent could run but could not create a nested child. If ignored, the log should again contain depths 1 through 3.

Before the run, I removed the earlier log so no entries could carry over from Run 1. I also confirmed that the variable was active in the shell used to start Claude Code. These controls made the number of new lines a direct measure of the variable's effect.

![Image](https://nextwork.ai/refreshed_maroon_timid_jujube/uploads/5baa3c06-1d0a-4f06-82f3-ce1629b76aa8_80zf28ny)

### What Run 2 revealed about CLAUDE_CODE_MAX_SUBAGENT_SPAWN_DEPTH

Run 2 showed that the variable moved the nesting wall. The log fell from three lines to one, so only depth 1 executed. If the variable had been ignored, the default three-depth sequence would have appeared again.

The result also showed that the value controlled nesting rather than disabling subagents entirely. The first depth-writer still ran and produced its file entry. What disappeared was its ability to create depth 2.

The refusal shape matched Run 1. The Agent tool was absent at the blocking depth, no additional artifact appeared, and the command exited 0 without a process error. The environment variable therefore changed where the wall appeared while preserving how Claude Code represented the blocked child request.

## Proving the Wall Exists Both Ways

### Confirming results from both directions

I deleted the Run 2 log, removed the environment variable override, and reran the chain under the default configuration. This restoration test checked whether the earlier three-level result returned after the temporary limit was removed.

The chain again wrote depths 1, 2, and 3. That confirmed the subagent mechanism worked through the expected default range and that the one-line Run 2 result came from the variable rather than a damaged agent definition.

Testing both directions strengthened the conclusion. Setting the variable reduced the chain from three entries to one, and removing it restored the three entries. A single shortened run could have come from stale configuration or an execution fault. The reversible change tied the observed depth directly to the active setting.

### Final summary: where the wall is and what the predictions got wrong

The observed default wall was 3 subagent links below the main session. Setting CLAUDE_CODE_MAX_SUBAGENT_SPAWN_DEPTH to 1 moved the wall to one link without blocking the first subagent.

The blocked spawn was silent at the process level. The Agent tool disappeared at the limiting depth, the requested child produced no artifact, and the command exited 0. An over-request could therefore look like an exact request unless the log or summary was checked.

I was wrong to call the Run 1 summary dishonest because it correctly stated that depth 4 never ran. I also missed a confident but incorrect inference in Run 2. Those errors showed why both summaries and artifacts need independent review.

## Going Further: Concurrent vs. Nesting Limits

### Comparing refusal shapes across limit types

The nesting test refused additional work by withdrawing the Agent tool. The blocked child never ran, no file appeared, and the log ended early without a process error. Nesting limits therefore reduced completed work.

The concurrency test produced a different result. All 4 workers ran, and all 4 files existed even though the tested cap was 3. No worker was refused. The observed effect, if the cap applied, was limited to scheduling or elapsed time rather than missing output.

The comparison remains bounded by the evidence. The concurrency cap may never have bound during that run, so the result does not prove that the limit is unenforced. It shows only that this 4-worker test completed all work, while the nesting test produced a clear missing descendant.

## Reflections and Key Takeaways

### Tools and concepts learned

I used the Claude Code CLI, .claude/settings.local.json for workspace-specific configuration, a custom depth-writer agent, and file artifacts to record which subagents ran. These tools let me test nesting without changing global settings.

The main concepts were nesting depth, concurrent execution, budget limits, and refusal shape. Nesting was enforced by removing the Agent tool at the boundary rather than returning a process error. The environment variable moved that boundary while leaving the first subagent available.

I also learned to separate summary claims from artifact evidence. A successful exit code did not prove the full request completed. The log, generated files, tool availability, and summary each revealed a different part of the result.

### Time and challenges

This build took approximately 35 minutes. The hardest part was diagnosing why the chain consistently stopped one level earlier than the requested target. I needed to verify whether the stop came from the platform wall, the depth-writer.md definition, or an active local override.

I checked that depth-writer.md still contained the intended stop condition and single-child behavior. I also reviewed the local settings for lingering CLAUDE_CODE_MAX_SUBAGENT_SPAWN_DEPTH values that could shorten the chain.

The reversible variable test resolved the uncertainty. Setting the value to 1 reduced the log to one line, and removing it restored three. That result distinguished the configured wall from a broken agent or corrupted workspace.

### Looking ahead

I completed this build to understand how Claude Code handles nested subagents and concurrent workers. File artifacts provided the primary evidence because a command can exit 0 even when a requested descendant never runs.

The tests established a default nesting wall at 3 subagent links and showed that CLAUDE_CODE_MAX_SUBAGENT_SPAWN_DEPTH could move it to 1. The concurrency run completed all 4 workers, though the tested cap may not have bound.

My next step is to study how local configuration overrides can govern larger agent workflows. I want to build clearer diagnostics for missing descendants, partial execution, budget stops, and scheduling delays so complex agent chains remain predictable.

---

*Built with [NextWork](https://nextwork.ai) - [View this project](https://nextwork.ai/projects/5baa3c06-1d0a-4f06-82f3-ce1629b76aa8)*
