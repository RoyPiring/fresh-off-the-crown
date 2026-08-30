<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# Package a Claude Code Workflow

**Project Link:** [View Project](https://nextwork.ai/projects/222ec695-83b2-4ba6-bf47-0287f323912c)

**Author:** Roy Piring: Cloud Platform Engineer | Build Master  
**Email:** rpiringhawaii@gmail.com

---

![Image](https://nextwork.ai/refreshed_maroon_timid_jujube/uploads/222ec695-83b2-4ba6-bf47-0287f323912c_j5yea7p4)

## Measuring Whether the Package Earned Its Keep

### Measuring the packaged onboarding

I measured how much practitioner work remained after moving the repeatable parts of a nine-action onboarding workflow into a local Claude Code plugin. The experiment compared a clean manual baseline with packaged runs that used the same actions, order, targets, and grading rules.

I tracked two forms of evidence: the number of actions still assigned to the practitioner and the total time required to finish the workflow. This distinction mattered because removing several actions does not prove that the package removed meaningful effort.

The package reduced the practitioner count from 9 actions to 4, but elapsed time moved from 00:46 to 00:45. I used the later runs to test consistency and safe refusal. The keep decision therefore depended on correct, repeatable behavior rather than a claim that the plugin made onboarding much faster.

### Remaining practitioner work and timed after result

Actions 1, 5, 6, and 9 remained with the practitioner after packaging. They covered supplying the target, deciding what the status line should say, and judging whether the final output was correct. The plugin could not replace those actions because they required case-specific input or human judgment.

The other five actions were mechanical writes that the package could perform consistently. Removing them reduced the practitioner action count from 9 to 4, a drop of five actions. However, those actions required little time during the manual run.

The measured duration changed from 00:46 to 00:45, so packaging saved only one second. The remaining four actions held almost all the original effort. The result showed that a lower action count did not equal the same reduction in time or difficulty. The package removed mechanical work, but it did not remove the actual bottleneck.

### Evidence behind the keep-or-delete decision

Runs 2 and 3 returned PASS and reported the same four remaining practitioner actions. That consistency showed that the package applied the rubric the same way across both valid targets. Run 4 also matched the prewritten failure condition on both required evidence halves and refused to repair the target.

The safety and correctness evidence supported keeping the package. It behaved consistently on valid inputs and stopped under the declared failure condition instead of changing a target it should reject.

The timing result did not support a speed claim. Total time changed from 00:46 to 00:45 because the five absorbed actions were inexpensive mechanical writes. I kept the package for repeatable execution and safe refusal, not for meaningful time savings. I also kept the 56% action-count reduction separate from effort because presenting those figures as equivalent would misstate the measured outcome.

![Image](https://nextwork.ai/refreshed_maroon_timid_jujube/uploads/222ec695-83b2-4ba6-bf47-0287f323912c_ojccfl34)

### Auditing the four-run evidence

The audit confirmed that the arithmetic and evidence trail were internally consistent. The difference between 9 original actions and 4 remaining actions was 5, matching the five steps claimed as packaged. The 00:46 and 00:45 results both used mm:ss, and the manual baseline was recorded before packaging existed.

The audit also traced Run 4’s grade to the prewritten failure condition. Exactly four runs were present, and the package did not change between them. These checks supported the comparison’s provenance and calculation.

The audit did not prove that KEEP was the only correct judgment. No acceptance threshold had been committed before the runs, and Run 3 repeated behavior that the package structure already constrained. The numbers held, but the decision still relied on my view that consistent execution and safe refusal justified keeping a package that saved only one second.

![Image](https://nextwork.ai/refreshed_maroon_timid_jujube/uploads/222ec695-83b2-4ba6-bf47-0287f323912c_bcroly7x)

## Building a Strictly Scoped Local Plugin

### Packaging the deterministic onboarding work

I packaged the repeatable onboarding work into a local Claude Code plugin stored at the workspace level. The plugin contained the configuration and skill instructions needed to run the fixed parts of the nine-action workflow without placing them in my global Claude Code environment.

Keeping the package local limited its effect to this build. Other repositories and sessions outside the workspace did not inherit the onboarding behavior. This boundary mattered because the experiment tested one workflow and did not justify changing every Claude Code session on the machine.

The package handled deterministic steps while leaving target selection, status-line judgment, and output review with the practitioner. It did not attempt to turn every action into automation. Its purpose was to encode the parts that could be repeated from declared rules while preserving the human decisions that depended on the current target.

### Keeping all nine actions visible in the comparison

The skill listed all nine original onboarding actions separately and in their committed order. None was merged, dropped, or renamed after the baseline. This kept the packaged run tied to the exact sequence used for the manual measurement.

Each action carried one ownership label. Before packaging, all 9 actions belonged to the practitioner. After packaging, the best possible result was 4 practitioner actions and 5 packaged actions. The denominator remained 9 in both conditions, which made the subtraction valid.

Rule 4 required the skill to walk through every action in order and count each PRACTITIONER entry as remaining human work. This prevented the package from reporting a lower count by skipping work it could not perform. The measurement therefore changed ownership, not the definition or number of actions. That kept the before-and-after evidence comparable.

![Image](https://nextwork.ai/refreshed_maroon_timid_jujube/uploads/222ec695-83b2-4ba6-bf47-0287f323912c_m5rn4ez4)

### Strict validation and session-local loading

I validated the plugin structure with strict error reporting before loading it for the timed run. The validation checked that the local files were present and correctly arranged, including plugin.json for the plugin manifest and SKILL.md for the workflow rules.

Strict reporting mattered because a partially loaded package could produce a misleading result. If the plugin silently ignored a missing field or file, the timed run could fall back to manual behavior while appearing packaged. A clear validation result established that the intended configuration was available before measurement.

I then loaded the skill only for the active session. This preserved the workspace boundary and avoided changing global Claude Code behavior. Session-local loading also made the test easier to reset because the package’s effect did not need to be removed from every future session. The timed evidence therefore came from a validated, local package under a known scope.

![Image](https://nextwork.ai/refreshed_maroon_timid_jujube/uploads/222ec695-83b2-4ba6-bf47-0287f323912c_njomgb2i)

## Establishing a Clean Manual Baseline

### Defining the nine actions, targets, and success threshold

I defined the manual workflow before packaging it: [List your nine onboarding steps here]. I also recorded the experiment targets as [List your two valid targets and your one target with a predeclared expected failure]. These placeholders remained visible because the source did not provide the missing action and target names.

The planned keep condition was that four recorded runs would show a consistent reduction in practitioner effort without reducing onboarding accuracy. Two targets were expected to pass, while one target carried a predeclared failure condition that the package needed to recognize and refuse.

The condition described the type of evidence required but did not set a numeric threshold for acceptable time savings. That omission later limited the keep-or-delete judgment. The experiment could confirm consistency, refusal, arithmetic, and provenance, but it could not test the final decision against a precommitted minimum gain.

### Capturing the untouched 9/9 baseline

I recorded the manual onboarding workflow before creating or loading the local plugin. The baseline contained all 9 actions under practitioner ownership and measured the time required to complete them from a clean state. This established the control condition for the later packaged runs.

The baseline needed to remain untouched because it defined the original work. Changing the action names, order, targets, or success rules after packaging would make the two conditions measure different workflows.

The manual run took 00:46. That value became the reference for the packaged result of 00:45. The baseline also preserved the full 9/9 practitioner count, allowing the later four-action result to be expressed against the same denominator. Recording both time and action ownership prevented the comparison from treating removed steps as proof of equal effort saved.

### Preventing automation from contaminating the measurement

The manual run served as the control condition, so the plugin had to remain absent until after that run was complete. Its purpose was to measure the work required to configure the status line and complete the onboarding actions by hand.

If the plugin folder had existed first, Claude Code could have loaded its configuration while I recorded the supposed manual baseline. I would then be unable to separate work performed by the practitioner from behavior supplied by the package. The causes would be mixed, making the before measurement invalid.

Keeping the package absent guaranteed that the 9/9 count and 00:46 time came from the manual workflow in a clean state. Only after preserving that evidence did I create and load the plugin. This ordering gave the later 4/9 and 00:45 results a valid reference instead of comparing two conditions that both contained automation.

## Reflecting on an Evidence-Driven Workflow Experiment

### Tools and concepts applied

I used the Claude Code CLI to load and run the local plugin. The plugin.json manifest defined the package, while SKILL.md stored the nine-action rubric, ownership labels, ordering rules, and prewritten failure condition.

The central concept was scope control. I kept the configuration inside the workspace so the experiment did not alter global Claude Code behavior. I also recorded the manual baseline before creating the package, preserving a clean control condition for the comparison.

The four-run method tested more than one successful execution. It checked valid targets, repeated behavior, and a target expected to fail under a declared rule. The audit then traced the results back to the original baseline and rubric. This structure supported claims about consistency and refusal while keeping the weak timing result visible instead of hiding it behind the lower action count.

### Time investment and challenge

This build took approximately 40 minutes. The hardest part was keeping the plugin restricted to the workspace. I initially struggled to configure the local directory without allowing the package to affect my global Claude Code settings.

The four-run requirement added another challenge. I had to preserve the same plugin, nine-action rubric, target rules, ownership labels, and timing format across every run. A change between runs would weaken the claim that different targets were tested under the same package.

I also tracked action reduction separately from elapsed time. The count fell from 9 to 4, but time moved only from 00:46 to 00:45. Keeping both measurements clear required careful evidence logging. That distinction prevented a large percentage drop in action count from being reported as an equally large reduction in practitioner effort.

### Next learning goal

I completed this build to learn how to package a repeatable onboarding workflow as a local Claude Code plugin. The package absorbed five mechanical actions, preserved four practitioner decisions, passed the valid runs, and refused the target that matched the prewritten failure condition.

The experiment also showed that packaging does not guarantee meaningful time savings. Practitioner actions fell from 9 to 4, while elapsed time changed by only one second. I kept the package because its consistent execution and refusal behavior had value, not because it removed the main effort.

My next learning goal is to build more complex multi-agent plugins that can coordinate tasks across repositories. Any expansion should preserve the same controls: local scope, a clean baseline, fixed comparison rules, explicit failure conditions, and evidence that separates fewer actions from less work.

---

*Built with [NextWork](https://nextwork.ai) - [View this project](https://nextwork.ai/projects/222ec695-83b2-4ba6-bf47-0287f323912c)*
