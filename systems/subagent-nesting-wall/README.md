# Find the Wall

> Part of [Fresh Off the Crown](../../README.md) · *A daily practice deciding what earns a place in the work.*

## Overview

Agent chains fail quietly. A nested subagent that never runs does not raise an error, and if the depth limit is unknown, a design can be built on a level that will never execute. The friction is that nesting depth was a guess: no measured wall, no idea what a refusal looks like, and no way to tell a blocked level from a level that simply had nothing to do.

The rig keeps one thing moving. A single `depth-writer` agent logs its depth, stops when it reaches the target, and otherwise spawns exactly one child, so the link count is the only variable across runs. Configuration sits at the build level rather than in global settings, and `--max-budget-usd 2.00` runs as a hard brake over the parent and every descendant together rather than per agent, because a faulty stop rule turns a linear chain into geometric growth and the billing page is a bad place to discover that. Three predictions were written before the first run.

Requesting four levels produced exactly three log lines, with depth 4 absent and no partial write, so the wall sits after the third subagent link. Rerunning with `CLAUDE_CODE_MAX_SUBAGENT_SPAWN_DEPTH=1` dropped the log to one line, which shows the variable moves the wall rather than being ignored, and that it caps nesting without disabling subagents, since depth 1 still ran. The refusal shape held in both directions: the Agent tool is simply absent at the blocking depth, nothing is written, and **the process still exits 0**. One prediction was wrong in a useful way, that the summary would hide the failure. It named the missing tool accurately, which is worth recording precisely because it was checked rather than assumed. Verdict **ADOPT** for the variable as the nesting control and the budget flag as the recursion brake. Reversed if a run shows the variable ignored or the default wall moving on its own.

## Architecture

```mermaid
---
title: Find the Wall
---
%%{init: {"theme":"base","themeVariables": {"primaryColor":"#1B4332","primaryTextColor":"#F4D03F","primaryBorderColor":"#F4D03F","secondaryColor":"#264653","tertiaryColor":"#2F5233","lineColor":"#F4D03F","fontFamily":"ui-monospace, SFMono-Regular, Menlo, Consolas, monospace","fontSize":"13px"}}}%%
flowchart TD
    classDef datastore fill:#264653,stroke:#F4D03F,stroke-width:2px,color:#FFFFFF
    classDef service fill:#1B4332,stroke:#F4D03F,stroke-width:2px,color:#F4D03F
    classDef event fill:#7B42BC,stroke:#F4D03F,stroke-width:2px,color:#FFFFFF
    classDef io fill:#0d1117,stroke:#F4D03F,stroke-width:1.5px,color:#F4D03F,font-style:italic

    Operator[/Operator designing an agent chain/]
    Known[/Where the wall is, and how it announces itself/]

    subgraph Safety["Brake before the experiment"]
        Budget("--max-budget-usd 2.00")
        WholeRun{{One ceiling for the parent AND every descendant, not $2 each}}
        Recursion{{A faulty stop rule turns a chain into geometric growth}}
    end

    subgraph Rig["One variable, one agent"]
        Workspace[(Build-level settings, global config untouched)]
        Writer("depth-writer: log depth, stop at N==M, else spawn one child")
        SameAgent{{Same definition at every link, so the link count is the only variable}}
        Log[(Append-only depth log)]
    end

    subgraph Predicted["Three predictions, written first"]
        P1{{Depths 1 to 3 write, depth 4 refused}}
        P2{{Refusal hidden or softened, command still exits 0}}
        P3{{Setting the depth variable to 1 leaves depth 1 alive}}
    end

    subgraph Run1["Run 1, default configuration"]
        Target4(Request 4 levels)
        Three[(Log holds exactly three lines)]
        NoPartial{{Depth 4 absent, no partial write}}
        ToolGone{{Agent tool unavailable at depth 3}}
        Exit0{{Process exits 0, a blocked child is not an error}}
    end

    subgraph Wrong["The prediction that missed"]
        Accurate{{Summary named the missing tool correctly}}
        P2Fails{{Prediction 2 wrong: nothing was hidden}}
        StillCheck{{An accurate summary is supporting evidence, not a substitute for the files}}
    end

    subgraph Run2["Run 2, the variable"]
        EnvVar("CLAUDE_CODE_MAX_SUBAGENT_SPAWN_DEPTH=1")
        Cleared(Prior log removed so lines cannot carry over)
        One[(Log falls to one line)]
        Moves{{The variable moves the wall rather than being ignored}}
        NotOff{{Depth 1 still runs, so it caps nesting and does not disable subagents}}
        SameShape{{Same refusal shape: tool absent, no artifact, exit 0}}
    end

    Operator -- "sets before anything runs" --> Budget
    Budget -- "applies as" --> WholeRun
    WholeRun -- "bounds the cost of" --> Recursion
    Operator -- "isolates in" --> Workspace
    Workspace -- "holds" --> Writer
    Writer -- "reused at every link giving" --> SameAgent
    Writer -- "appends to" --> Log
    Operator -- "records" --> P1
    Operator -- "records" --> P2
    Operator -- "records" --> P3
    Writer -- "invoked with" --> Target4
    Target4 -- "produced" --> Three
    Three -- "confirms" --> P1
    Three -- "accompanied by" --> NoPartial
    Three -- "explained by" --> ToolGone
    ToolGone -- "surfaces as" --> Exit0
    ToolGone -- "was reported correctly, giving" --> Accurate
    Accurate -- "refutes" --> P2
    Accurate -- "recorded as" --> P2Fails
    P2Fails -- "leaves the habit intact as" --> StillCheck
    Log -- "is what settled it, not the summary, hence" --> StillCheck
    EnvVar -- "run after" --> Cleared
    Cleared -- "makes new lines a direct measure for" --> One
    EnvVar -- "produced" --> One
    One -- "demonstrates" --> Moves
    One -- "with depth 1 alive, demonstrates" --> NotOff
    Moves -- "while preserving" --> SameShape
    Exit0 -- "matched by" --> SameShape
    P3 -- "confirmed by" --> NotOff
    Moves -- "gives the operator" --> Known
    SameShape -- "warns that a blocked level is silent, so" --> Known
    Recursion -- "is why the brake came first for" --> Known

    class Workspace,Log,Three,One datastore
    class Budget,Writer,Target4,EnvVar,Cleared service
    class WholeRun,Recursion,SameAgent,P1,P2,P3,NoPartial,ToolGone,Exit0,Accurate,P2Fails,StillCheck,Moves,NotOff,SameShape event
    class Operator,Known io
```

The diagram shows the topology and data flow of the system as built. The captured evidence for this run, including screenshots of what the tools actually returned, lives in [`documents/subagent-nesting-wall.md`](./documents/subagent-nesting-wall.md).

## Implementation

This system is built across **6 phases**:

1. Understanding the Experiment
2. Forming Predictions Before Running the Chain
3. Crossing the Nesting Wall and Reading the Evidence
4. Testing the Environment Variable
5. Proving the Wall Exists Both Ways
6. Going Further: Concurrent vs. Nesting Limits

For the run as it happened, with the screenshots taken at each step, see [`documents/subagent-nesting-wall.md`](./documents/subagent-nesting-wall.md).

## Validation

Each phase below has its evidence in [`documents/subagent-nesting-wall.md`](./documents/subagent-nesting-wall.md), with the screenshots, configuration, and observed results recorded as the run happened:

- ✅ Understanding the Experiment
- ✅ Forming Predictions Before Running the Chain
- ✅ Crossing the Nesting Wall and Reading the Evidence
- ✅ Testing the Environment Variable
- ✅ Proving the Wall Exists Both Ways
- ✅ Going Further: Concurrent vs. Nesting Limits
