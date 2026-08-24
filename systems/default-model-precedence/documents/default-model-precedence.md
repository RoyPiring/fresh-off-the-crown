<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# Three Layers, One Model

**Project Link:** [View Project](https://nextwork.ai/projects/c6718760-ae6f-4116-84ce-32eef90e1c48)

**Author:** Roy Piring: Cloud Platform Engineer | Build Master  
**Email:** rpiringhawaii@gmail.com

---

![Image](https://nextwork.ai/refreshed_maroon_timid_jujube/uploads/c6718760-ae6f-4116-84ce-32eef90e1c48_b7lw0mge)

## Estimating the Precedence Order

### Pre-experiment estimate

I tested how Claude Code resolves conflicting model selections across three configuration layers: an environment variable, a settings file, and the --model command-line flag. The goal was to observe which model became active when the layers disagreed and then remove each layer to reveal the fallback order.

Before running the test, I predicted that --model would have the highest precedence because it applies to the current command. I placed the settings file second because it stores a persistent preference, and the environment variable third. The test needed distinct model values at each layer so the winning source could be identified. It also needed a known account default beneath them so any failed override would be visible.

## Setting Up the Experiment

### Goals for this step

I prepared three configuration sources with conflicting model values. The environment variable, settings file, and --model flag each needed to select a different model so the active result could reveal which layer Claude Code honored.

The setup also required a clean baseline. Before adding overrides, I checked the account's built-in model and confirmed that no tested configuration source was already selecting a model. This mattered because a hidden value could make one layer appear to win when Claude Code was falling through to another source.

The method was to change one layer at a time, record the active model, and avoid treating a matching model name as proof unless the competing values made the source distinguishable.

![Image](https://nextwork.ai/refreshed_maroon_timid_jujube/uploads/c6718760-ae6f-4116-84ce-32eef90e1c48_0rhzolpa)

### Account's built-in default model

The account-level default was Opus 5 (claude-opus-5). I treated this as the floor beneath the tested configuration layers. If no valid override applied, Claude Code resolved to this model.

I confirmed the unoverridden state before introducing conflicts. ANTHROPIC_MODEL was unset, no project settings.json was present, and ~/.claude/settings.json contained no model key. Under those conditions, Claude Code reported Opus 5.

Knowing the floor was necessary because several later runs also displayed opus. A result matching the account default could mean an explicit opus override won, or it could mean every tested override failed and resolution fell through. The model name alone could not distinguish those explanations without a conflicting control.

## Making All Three Layers Disagree

### Goals for this step

I assigned conflicting model values to the command-line flag, settings file, and environment layer, then planned to remove the winning source one layer at a time. This design was intended to expose the precedence order through observed fallback behavior.

The first run tested the top of the stack. After that, removing --model should reveal whether the settings value or environment value controlled the session. Removing the next winner should expose the remaining layer or the account default.

The main constraint was observability. If an override selected the same model as the account default, the output could not prove which source had won. Each result therefore needed to be interpreted with the configured values and known floor, not only the displayed model name.

### Which layer won when all three disagreed

With all three layers configured to disagree, Claude Code reported opus, matching the value passed through --model. I ranked the command-line flag first because it appeared to beat settings.json set to sonnet and the environment layer set to haiku.

The result carried an important caveat. Opus was also the account-level default. The same output would have appeared if the flag, settings file, and environment value had all failed. The displayed model therefore matched the predicted flag winner but did not independently prove that the flag caused the result.

Later testing provided stronger evidence for the flag's position by assigning it a model different from the account default. That distinction was necessary to separate explicit precedence from fallback behavior.

## Peeling the Stack to Find the Fallback Order

### Goals for this step

I removed the --model flag while leaving the other configuration sources in place. The next active model would show which remaining layer controlled Claude Code after the highest-ranked override disappeared.

This step tested fallback behavior rather than another full conflict. If sonnet appeared, the settings file was active. If haiku appeared, the environment value was active. If opus appeared, neither remaining source had applied and Claude Code had returned to the account default.

The result also needed to be repeatable in the shell where Claude Code started. Environment variables can disappear across shell boundaries, so checking whether the variable was present was part of interpreting the output. Removing layers without confirming their state could produce a sequence that looked ordered but was not.

![Image](https://nextwork.ai/refreshed_maroon_timid_jujube/uploads/c6718760-ae6f-4116-84ce-32eef90e1c48_5jen500w)

### Model revealed at each layer removal

After I removed the --model flag, Claude Code reported sonnet. This showed that the settings file was active and took control when the flag was absent. It also appeared to beat the intended haiku environment value.

After I removed the settings key, Claude Code reported opus / Opus 5 rather than haiku. That result came from the account default, not the planned environment layer. The environment value was never active because it did not persist into the new shell, and ANTHROPIC_DEFAULT_MODEL was not a variable name Claude Code read.

The observed fallback therefore proved the transition from flag to settings file. It did not prove where a valid ANTHROPIC_MODEL value ranked against settings.json because that conflict had not yet been run.

### Full observed sequence vs. original estimate

The observed sequence was 1) opus from --model, 2) sonnet from settings.json, 3) opus from the account default, and 4) opus from the account default. Four entries produced only three distinct outcomes and one clear precedence transition: flag to file.

The original estimate expected haiku in entry 3. That did not happen because the environment value did not persist into the new shell and ANTHROPIC_DEFAULT_MODEL was not recognized by Claude Code.

Entries 1, 3, and 4 all displayed opus for different but visually identical reasons. Entry 1 was intended as a flag result, while entries 3 and 4 were fallback results. This showed why matching the default model cannot prove which layer controlled the session.

## Discovering the Fourth Layer

![Image](https://nextwork.ai/refreshed_maroon_timid_jujube/uploads/c6718760-ae6f-4116-84ce-32eef90e1c48_pidf6fx9)

### Placing ANTHROPIC_MODEL in the full precedence order

The later test placed ANTHROPIC_MODEL below the --model flag and above the account default. Entry 6 provided the upper boundary: --model=haiku beat ANTHROPIC_MODEL=opus. Because the flag value differed from the account default, this was stronger evidence that the command-line flag controlled the result.

The exact order between settings.json and ANTHROPIC_MODEL remained unresolved. Entry 5 needed to test settings=sonnet against ANTHROPIC_MODEL=opus with no command-line flag. That comparison was not run.

The supported order was therefore partial rather than complete. --model ranked above ANTHROPIC_MODEL, and ANTHROPIC_MODEL ranked above the account floor. The settings file and environment variable remained unordered relative to each other.

## Reflections and Takeaways

### Key tools and concepts

I used PowerShell environment variables, the ~/.claude/settings.json file, settings.json, and the Claude Code command-line interface. These tools let me assign conflicting model values, remove one source at a time, and observe which model became active.

The main concept was configuration precedence. A useful test requires more than setting several values and reading the final model name. Each layer needs a distinct value, and the account default must be known so an explicit override can be separated from fallback.

I also learned to treat unresolved comparisons honestly. The runs supported the flag-over-file transition and the flag-over-ANTHROPIC_MODEL result. They did not establish the exact order between settings.json and ANTHROPIC_MODEL because that direct conflict was never executed.

### Time and challenges

This build took approximately 35 minutes. The hardest part was isolating each configuration layer while keeping the remaining values active and observable. The environment value created the main difficulty because it did not persist into the new shell.

The account default also matched several intended test values. When opus appeared, the output alone could not show whether an opus override had won or Claude Code had ignored every override and returned to the floor. Assigning different models to competing layers was necessary to remove that ambiguity.

The exercise showed that peeling a stack only works when each layer is confirmed before the run. Otherwise, removing one value can expose a missing layer rather than the next valid source, producing an incomplete precedence sequence.

### What's next

I completed this build to understand how Claude Code resolves conflicting model choices across command-line flags, settings files, environment variables, and the account default. The observed runs established part of the hierarchy while leaving one comparison unresolved.

The next required test is settings=sonnet against ANTHROPIC_MODEL=opus with no --model flag. That run would determine whether the settings file or valid environment variable holds the remaining higher position.

After completing the precedence map, I want to automate environment setup for other command-line tools. The goal is to create repeatable configurations across development environments while preserving a clear record of which layer supplies each value and how command-specific overrides change it.

---

*Built with [NextWork](https://nextwork.ai) - [View this project](https://nextwork.ai/projects/c6718760-ae6f-4116-84ce-32eef90e1c48)*
