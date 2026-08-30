# Three Layers, One Model

> Part of [Fresh Off the Crown](../../README.md) · *A daily practice deciding what earns a place in the work.*

## Overview

Claude Code takes a model from a command-line flag, a settings file, an environment variable, and an account default. When two disagree, nothing tells you which won. Set a model in the wrong place and the session quietly runs something else, and the output looks identical either way, so model selection was set by habit with no way to confirm it took effect.

The experiment gives each layer a different model so the winner is identifiable, and establishes the floor first: with the variable unset, no project settings, and no user model key, the account resolves to Opus 5. That floor turns out to be the whole difficulty. The first run set the flag to opus, settings to sonnet, and the environment to haiku, and reported opus, which looks like a clean win for the flag and is not evidence, because the identical output would appear if all three overrides had failed.

Peeling the stack produced the one transition it proves. Removing the flag reported sonnet, so the settings file takes control when the flag is gone. Removing the settings key reported opus rather than the predicted haiku, because the environment layer was never active: the variable did not survive into the new shell, and `ANTHROPIC_DEFAULT_MODEL` is not a name Claude Code reads. A later run broke the tie by making the flag disagree with the floor, and `--model=haiku` against `ANTHROPIC_MODEL=opus` returned haiku, which is the one unconfounded result. Verdict **ADOPT** for `--model` as the control surface, with the order between `settings.json` and `ANTHROPIC_MODEL` left explicitly unresolved because that comparison was never run. Re-check 2026-08-30. Reversed if `--model` ever fails to override a set `ANTHROPIC_MODEL`.

## Architecture

```mermaid
---
title: Three Layers, One Model
---
%%{init: {"theme":"base","themeVariables": {"primaryColor":"#1B4332","primaryTextColor":"#F4D03F","primaryBorderColor":"#F4D03F","secondaryColor":"#264653","tertiaryColor":"#2F5233","lineColor":"#F4D03F","fontFamily":"ui-monospace, SFMono-Regular, Menlo, Consolas, monospace","fontSize":"13px"}}}%%
flowchart TD
    classDef datastore fill:#264653,stroke:#F4D03F,stroke-width:2px,color:#FFFFFF
    classDef service fill:#1B4332,stroke:#F4D03F,stroke-width:2px,color:#F4D03F
    classDef event fill:#7B42BC,stroke:#F4D03F,stroke-width:2px,color:#FFFFFF
    classDef io fill:#0d1117,stroke:#F4D03F,stroke-width:1.5px,color:#F4D03F,font-style:italic

    Operator[/Operator setting a model and not knowing if it took/]
    Order[/A precedence order backed by observation/]

    subgraph Layers["Four candidate layers"]
        Flag[("--model flag")]
        Settings[(settings.json model key)]
        EnvVar[(ANTHROPIC_MODEL)]
        Floor[(Account default: Opus 5)]
    end

    subgraph Setup["Baseline before any override"]
        Clean(Confirm nothing is already selecting a model)
        Unset{{ANTHROPIC_MODEL unset, no project settings, no user model key}}
        Distinct{{Each layer given a DIFFERENT model so the winner is identifiable}}
    end

    subgraph Conflict["All three disagree"]
        E1[(Entry 1: flag opus, settings sonnet, env haiku)]
        SawOpus{{Reported opus, matching the flag}}
        Confound{{But opus is ALSO the account default, so this proves nothing alone}}
    end

    subgraph Peel["Peel the stack"]
        E2(Remove the flag)
        SawSonnet{{Reported sonnet, so settings.json took control}}
        E3(Remove the settings key)
        SawOpusAgain{{Reported opus, the floor, not the predicted haiku}}
    end

    subgraph Why["Why the env layer never fired"]
        Shell{{The variable did not persist into the new shell}}
        WrongName{{ANTHROPIC_DEFAULT_MODEL is not a name Claude Code reads}}
    end

    subgraph Fourth["The disambiguating run"]
        E6[("Entry 6: --model=haiku against ANTHROPIC_MODEL=opus")]
        FlagWins{{haiku won, and haiku is not the default, so the flag genuinely controls}}
    end

    subgraph Result["What is supported, and what is not"]
        Proven{{Proven: flag beats settings, flag beats env, env beats the floor}}
        Unresolved{{Unresolved: settings.json against ANTHROPIC_MODEL was never run}}
        Owed[(Entry 5 owed: settings sonnet vs env opus, no flag)]
    end

    Operator -- "must choose among" --> Flag
    Operator -- "must choose among" --> Settings
    Operator -- "must choose among" --> EnvVar
    Floor -- "sits beneath" --> Flag
    Clean -- "verified" --> Unset
    Unset -- "establishes the floor as" --> Floor
    Clean -- "requires" --> Distinct
    Distinct -- "configured as" --> E1
    E1 -- "produced" --> SawOpus
    SawOpus -- "undermined by" --> Confound
    Floor -- "is the reason for" --> Confound
    E1 -- "then" --> E2
    E2 -- "produced" --> SawSonnet
    SawSonnet -- "ranks" --> Settings
    E2 -- "then" --> E3
    E3 -- "produced" --> SawOpusAgain
    SawOpusAgain -- "explained by" --> Shell
    SawOpusAgain -- "explained by" --> WrongName
    WrongName -- "corrected to" --> EnvVar
    Confound -- "forced" --> E6
    E6 -- "produced" --> FlagWins
    FlagWins -- "establishes" --> Proven
    SawSonnet -- "contributes to" --> Proven
    Proven -- "stops short at" --> Unresolved
    Unresolved -- "names" --> Owed
    Proven -- "gives" --> Order
    Unresolved -- "bounds" --> Order

    class Flag,Settings,EnvVar,Floor,E1,E6,Owed datastore
    class Clean,E2,E3 service
    class Unset,Distinct,SawOpus,Confound,SawSonnet,SawOpusAgain,Shell,WrongName,FlagWins,Proven,Unresolved event
    class Operator,Order io
```

The diagram shows the topology and data flow of the system as built. The captured evidence for this run, including screenshots of what the tools actually returned, lives in [`documents/default-model-precedence.md`](./documents/default-model-precedence.md).

## Implementation

This system is built across **5 phases**:

1. Estimating the Precedence Order
2. Setting Up the Experiment
3. Making All Three Layers Disagree
4. Peeling the Stack to Find the Fallback Order
5. Discovering the Fourth Layer

For the run as it happened, with the screenshots taken at each step, see [`documents/default-model-precedence.md`](./documents/default-model-precedence.md).

## Validation

Each phase below has its evidence in [`documents/default-model-precedence.md`](./documents/default-model-precedence.md), with the screenshots, configuration, and observed results recorded as the run happened:

- ✅ Estimating the Precedence Order
- ✅ Setting Up the Experiment
- ✅ Making All Three Layers Disagree
- ✅ Peeling the Stack to Find the Fallback Order
- ✅ Discovering the Fourth Layer
