# Probe Paper's Comments with an Agent

> Part of [Fresh Off the Crown](../../README.md) · *A daily practice deciding what earns a place in the work.*

## Overview

Design feedback arrives as comments on a canvas, and acting on each one by hand is the whole cost. If an agent could read those comments through MCP and apply them, the review loop closes itself. The open question is not whether it can edit the file, but whether comment state is a reliable switch, because an agent that acts on feedback already marked resolved would silently redo settled work.

The test wires Paper Desktop to a Cursor agent over MCP and puts three independent properties on the canvas, one per comment, so an outcome maps to exactly one instruction: a rectangle at fill `#D1D5DB`, a text label reading "Draft Heading", and a frame at 24px padding. Each gets a comment under a different condition, an unresolved instruction naming a colour, an unresolved observation with no number, and a resolved instruction naming an exact string. Predictions were written before the run, and the result is judged from the saved file rather than the agent's own account of what it did.

Two things came back worth keeping. The unresolved observation was supposed to hold at 24px, on the reasoning that noticing a problem is not authorising a fix, and the agent changed it to 40px, a number nothing in the comment supplied. And the headline canvas score of 3 of 3 turned out to be hollow: the text already matched its comment, so a match could not say who changed it or when. That forced the run that actually proves the point, resetting the text and leaving the same wording unresolved, after which the agent changed it and left the other two properties alone. Verdict **ADOPT** for comment state as the control surface, since resolution is now shown to block. Reversed the first time a resolved comment gets acted on anyway.

## Architecture

```mermaid
---
title: Probe Paper's Comments with an Agent
---
%%{init: {"theme":"base","themeVariables": {"primaryColor":"#1B4332","primaryTextColor":"#F4D03F","primaryBorderColor":"#F4D03F","secondaryColor":"#264653","tertiaryColor":"#2F5233","lineColor":"#F4D03F","fontFamily":"ui-monospace, SFMono-Regular, Menlo, Consolas, monospace","fontSize":"13px"}}}%%
flowchart TD
    classDef datastore fill:#264653,stroke:#F4D03F,stroke-width:2px,color:#FFFFFF
    classDef service fill:#1B4332,stroke:#F4D03F,stroke-width:2px,color:#F4D03F
    classDef event fill:#7B42BC,stroke:#F4D03F,stroke-width:2px,color:#FFFFFF
    classDef io fill:#0d1117,stroke:#F4D03F,stroke-width:1.5px,color:#F4D03F,font-style:italic

    Operator[/Operator asking whether a comment is an instruction/]
    Answer[/A control surface that can be trusted, or not/]

    subgraph Wiring["Delivery path only"]
        Paper(Paper Desktop)
        Cursor(Cursor agent over MCP)
        NotProof{{The connection delivers, it never judges the result}}
    end

    subgraph Baseline["Three independent properties"]
        Rect[(Rectangle fill #D1D5DB)]
        Text[(Text "Draft Heading")]
        Frame[(Frame padding 24px)]
        Independent{{One property each, so an outcome maps to one comment}}
        Recorded{{Baseline recorded before the agent runs}}
    end

    subgraph Conditions["One comment per condition"]
        OpenInstr[(Unresolved instruction naming a fill)]
        OpenObs[(Unresolved observation, no number given)]
        ResolvedInstr[(Resolved instruction naming an exact string)]
    end

    subgraph Predicted["Predictions written first"]
        PFill{{Fill will change: open and specific}}
        PPad{{Padding will hold at 24px: noticing is not authorising}}
        PText{{Text will hold: resolved should mean skip}}
    end

    subgraph Observed["What the file showed"]
        Fill{{Fill became #3B82F6, as predicted}}
        Pad{{Padding became 40px, a number the agent chose itself}}
        TextSame{{Text unchanged, agent said it was already Final Heading}}
        Score{{Canvas score 3 of 3}}
        Hollow{{A matching value cannot say who changed it, or when}}
    end

    subgraph Isolate["The run that actually proved it"]
        Reset(Reset text to Draft Heading)
        SameWords{{Same wording, resolution the only variable}}
        Changed{{Agent changed it to Final Heading}}
        Untouched{{Fill and padding stayed put}}
        Cause{{Resolution was the blocking factor}}
    end

    Operator -- "connects" --> Paper
    Paper -- "exposed through MCP to" --> Cursor
    Cursor -- "can edit, but its chat is" --> NotProof
    Operator -- "builds" --> Rect
    Operator -- "builds" --> Text
    Operator -- "builds" --> Frame
    Rect -- "kept separate by" --> Independent
    Independent -- "captured as" --> Recorded
    Rect -- "carries" --> OpenInstr
    Frame -- "carries" --> OpenObs
    Text -- "carries" --> ResolvedInstr
    OpenInstr -- "predicted as" --> PFill
    OpenObs -- "predicted as" --> PPad
    ResolvedInstr -- "predicted as" --> PText
    Cursor -- "acts on" --> OpenInstr
    Cursor -- "acts on" --> OpenObs
    Cursor -- "reads and skips" --> ResolvedInstr
    PFill -- "confirmed by" --> Fill
    PPad -- "contradicted by" --> Pad
    PText -- "matched by" --> TextSame
    Fill -- "counted into" --> Score
    Pad -- "counted into" --> Score
    TextSame -- "counted into" --> Score
    Score -- "undermined by" --> Hollow
    Recorded -- "cannot resolve cause when the value already matched, so" --> Hollow
    Hollow -- "forced" --> Reset
    Reset -- "holding" --> SameWords
    SameWords -- "produced" --> Changed
    Changed -- "alongside" --> Untouched
    Changed -- "establishes" --> Cause
    Cause -- "gives" --> Answer
    Pad -- "warns about" --> Answer
    NotProof -- "is why the artifact decided" --> Answer

    class Rect,Text,Frame,OpenInstr,OpenObs,ResolvedInstr datastore
    class Paper,Cursor,Reset service
    class NotProof,Independent,Recorded,PFill,PPad,PText,Fill,Pad,TextSame,Score,Hollow,SameWords,Changed,Untouched,Cause event
    class Operator,Answer io
```

The diagram shows the topology and data flow of the system as built. The captured evidence for this run, including screenshots of what the tools actually returned, lives in [`documents/paper-comment-instructions.md`](./documents/paper-comment-instructions.md).

## Implementation

This system is built across **6 phases**:

1. Connecting Paper and Cursor via MCP
2. Defining the Experiment
3. Building Three Independent Elements and Recording the Baseline
4. Placing Three Comments Under Three Conditions
5. Reading the File and Scoring the Prediction
6. Isolating the Resolved Instruction

For the run as it happened, with the screenshots taken at each step, see [`documents/paper-comment-instructions.md`](./documents/paper-comment-instructions.md).

## Validation

Each phase below has its evidence in [`documents/paper-comment-instructions.md`](./documents/paper-comment-instructions.md), with the screenshots, configuration, and observed results recorded as the run happened:

- ✅ Connecting Paper and Cursor via MCP
- ✅ Defining the Experiment
- ✅ Building Three Independent Elements and Recording the Baseline
- ✅ Placing Three Comments Under Three Conditions
- ✅ Reading the File and Scoring the Prediction
- ✅ Isolating the Resolved Instruction
