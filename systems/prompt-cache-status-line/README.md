# Measure Claude Code Prompt Cache

> Part of [Fresh Off the Crown](../../README.md) · *A daily practice deciding what earns a place in the work.*

## Overview

Prompt cache behaviour is invisible unless something surfaces it, and a status line that reports the wrong thing is worse than none. The friction is twofold: the fields are not exposed where you would look for them, and a comparison between two surfaces can disagree for reasons that have nothing to do with either being wrong.

The first draft guessed from human-facing labels and root-level fields, so every lookup returned null and the line rendered as five dashes. The correction came from inspecting the captured JSON rather than guessing again, giving five real paths under `prompt_cache` and a verified 5 of 5 schema score. Two values need display handling, a ratio multiplied and rounded to 81% and a token count rendered as 48.2k. The line was then run under PowerShell 7 and 5.1, producing identical output that matched the saved reading byte for byte.

Two findings outlast the script. Equality between surfaces requires the same field definition **and** the same measurement window: an initial sweep of the obvious commands returned 0 of 5 because none of them carry these fields at all, and two later mismatches came from comparing cumulative against per-request values and billed against context tokens, which are different scopes rather than contradictions. Cache write matched exactly to the token, which is the strongest cross-surface confirmation available. Second, counters move on any turn, so both readings must be captured with no model turn between them or the comparison measures timing rather than correctness. The field semantics also check out: across two sessions `expected_rebuilds` climbed from 1 to 4 through an autocompact loop while `misses` held at 1, so compaction rebuilds are attributed rather than reported as unexplained misses. Verdict **ADOPT**. Reversed the moment a release changes the schema, with five dashes as the detector.

## Architecture

```mermaid
---
title: Measure Claude Code Prompt Cache
---
flowchart TD
%%{init: {"theme":"base","themeVariables": {"primaryColor":"#1B4332","primaryTextColor":"#F4D03F","primaryBorderColor":"#F4D03F","secondaryColor":"#264653","tertiaryColor":"#2F5233","lineColor":"#F4D03F","fontFamily":"ui-monospace, SFMono-Regular, Menlo, Consolas, monospace","fontSize":"13px"}}}%%
    classDef datastore fill:#264653,stroke:#F4D03F,stroke-width:2px,color:#FFFFFF
    classDef service fill:#1B4332,stroke:#F4D03F,stroke-width:2px,color:#F4D03F
    classDef event fill:#7B42BC,stroke:#F4D03F,stroke-width:2px,color:#FFFFFF
    classDef io fill:#0d1117,stroke:#F4D03F,stroke-width:1.5px,color:#F4D03F,font-style:italic

    Operator[/Operator who cannot see cache behaviour/]
    Trustworthy[/A status line worth believing/]

    subgraph Guess["The draft built from labels"]
        Labels(Human-facing names and root-level fields)
        Nulls{{Every lookup returned null}}
        Dashes{{Rendered as five dashes}}
    end

    subgraph Schema["Grounding in the captured payload"]
        Payload[(prompt-cache-payload.json, saved)]
        Paths[(requests, hit_ratio, misses, expected_rebuilds, miss_recache_tokens)]
        Score{{5 of 5 schema grounding}}
        Format{{Display handling: ratio to 81%, tokens to 48.2k}}
    end

    subgraph CrossShell["Proving the reading is stable"]
        Ps7(PowerShell 7)
        Ps51(PowerShell 5.1)
        Identical{{Identical output, byte for byte against the saved reading}}
    end

    subgraph Windows["Why surfaces disagree"]
        Sweep{{Sweep of the obvious commands returned 0 of 5}}
        Absent{{Those commands carry none of these fields at all}}
        Cumulative{{Cumulative against per-request}}
        Billed{{Billed tokens against context tokens}}
        NotWrong{{Different scopes, so disagreement proves neither wrong}}
        CacheWrite{{Cache write matched exactly to the token}}
    end

    subgraph Atomic["Why timing is part of the evidence"]
        NoTurn{{Both readings captured with no model turn between}}
        Stale{{Any response, compaction or request moves the counters}}
        Measures{{Otherwise the mismatch measures timing, not a wrong path}}
        Append[(Append-only record preserves each reading with its moment)]
    end

    subgraph Semantics["The field behaving as intended"]
        Rebuilds[(expected_rebuilds 1 to 4 across two sessions)]
        Loop{{Autocompact loop: context back at the limit within 3 turns, warned 3 times}}
        MissesFlat{{misses held at 1 throughout}}
        Attributed{{Compaction rebuilds attributed, not reported as unexplained misses}}
    end

    Operator -- "first writes" --> Labels
    Labels -- "produced" --> Nulls
    Nulls -- "rendered as" --> Dashes
    Dashes -- "forced inspection of" --> Payload
    Payload -- "yields" --> Paths
    Paths -- "scored" --> Score
    Paths -- "needed" --> Format
    Paths -- "run under" --> Ps7
    Paths -- "run under" --> Ps51
    Ps7 -- "compared with Ps51 gives" --> Identical
    Ps51 -- "compared with Ps7 gives" --> Identical
    Operator -- "began with" --> Sweep
    Sweep -- "explained by" --> Absent
    Sweep -- "later contrasted with" --> Cumulative
    Cumulative -- "alongside" --> Billed
    Billed -- "together give" --> NotWrong
    Cumulative -- "together give" --> NotWrong
    Payload -- "confirmed by" --> CacheWrite
    Stale -- "requires" --> NoTurn
    NoTurn -- "otherwise" --> Measures
    NoTurn -- "recorded in" --> Append
    Append -- "keeps the earlier value from being overwritten in" --> Rebuilds
    Rebuilds -- "driven by" --> Loop
    Rebuilds -- "alongside" --> MissesFlat
    MissesFlat -- "together show" --> Attributed
    Score -- "gives" --> Trustworthy
    Identical -- "gives" --> Trustworthy
    NotWrong -- "is the reading rule behind" --> Trustworthy
    Measures -- "is the timing rule behind" --> Trustworthy
    Attributed -- "confirms the semantics behind" --> Trustworthy
    CacheWrite -- "is the strongest evidence for" --> Trustworthy

    class Payload,Paths,Rebuilds,Append datastore
    class Labels,Ps7,Ps51 service
    class Nulls,Dashes,Score,Format,Identical,Sweep,Absent,Cumulative,Billed,NotWrong,CacheWrite,NoTurn,Stale,Measures,Loop,MissesFlat,Attributed event
    class Operator,Trustworthy io
```

The diagram shows the topology and data flow of the system as built. The captured evidence for this run, including screenshots of what the tools actually returned, lives in [`documents/prompt-cache-status-line.md`](./documents/prompt-cache-status-line.md).

## Implementation

This system is built across **6 phases**:

1. Proving Status-Line Accuracy with Same-Moment Evidence
2. Correcting the Five-Field Status Line
3. Measuring Timing and Rebuild Behaviour
4. Scoring a Schema-Grounded Draft
5. Preparing an Authenticated Claude Code Environment
6. Investigating Claude Code Status-Line Reliability

For the run as it happened, with the screenshots taken at each step, see [`documents/prompt-cache-status-line.md`](./documents/prompt-cache-status-line.md).

## Validation

Each phase below has its evidence in [`documents/prompt-cache-status-line.md`](./documents/prompt-cache-status-line.md), with the screenshots, configuration, and observed results recorded as the run happened:

- ✅ Proving Status-Line Accuracy with Same-Moment Evidence
- ✅ Correcting the Five-Field Status Line
- ✅ Measuring Timing and Rebuild Behaviour
- ✅ Scoring a Schema-Grounded Draft
- ✅ Preparing an Authenticated Claude Code Environment
- ✅ Investigating Claude Code Status-Line Reliability
