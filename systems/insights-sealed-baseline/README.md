# Audit Claude Code Insights

> Part of [Fresh Off the Crown](../../README.md) · *A daily practice deciding what earns a place in the work.*

## Overview

Reviewing my own working friction runs on memory, which is slow and unreliable. A report generated from 1,745 messages across 71 sessions could replace it, but only if it finds what I already know is wrong, and any report feels accurate when read after the fact. So the question was whether it earns a place in the regular review, and answering that honestly meant fixing what I knew before the report existed.

The build is that ordering, enforced by timestamps. A numbered ten-item friction list was written from memory and sealed at 23:37:36. The report file was created at 23:45:11, nearly eight minutes later, and its count-bearing header was captured at 23:46:42 before any finding was transcribed. Nine report frictions were then compared one at a time against one frozen item each, marked Confirmed or New with a stated reason, while every sealed item the report never raised was counted as unraised. Method and verdict rules were committed first, so no result could set its own acceptance standard.

The first pass returned 4 confirmed, 5 new, and 6 unraised, reconciled by 4+5=9 against the rows and 4+6=10 against the list. A close-match audit then rejected the strongest link in it, where a broad report category had been matched to a narrower remembered cause, moving that row to New and returning its item to unraised for a corrected **3 confirmed, 6 new, and 7 unraised**. The correction cut against the report. Verdict **WATCH**: six findings memory missed keep it clear of PARK, which the declared rules place at 2 or fewer confirmed, but a report raising 3 of 10 known frictions cannot replace the review it was meant to replace. Two limits stay attached rather than smoothed away. I read one paragraph of the summary before closing the scope record, and the report's own 49-versus-71 session figures leave the denominator unsettled, which is what a later report has to resolve.

## Architecture

```mermaid
---
title: Audit Claude Code Insights
---
%%{init: {"theme":"base","themeVariables": {"primaryColor":"#1B4332","primaryTextColor":"#F4D03F","primaryBorderColor":"#F4D03F","secondaryColor":"#264653","tertiaryColor":"#2F5233","lineColor":"#F4D03F","fontFamily":"ui-monospace, SFMono-Regular, Menlo, Consolas, monospace","fontSize":"13px"}}}%%
flowchart TD
    classDef datastore fill:#264653,stroke:#F4D03F,stroke-width:2px,color:#FFFFFF
    classDef service fill:#1B4332,stroke:#F4D03F,stroke-width:2px,color:#F4D03F
    classDef event fill:#7B42BC,stroke:#F4D03F,stroke-width:2px,color:#FFFFFF
    classDef io fill:#0d1117,stroke:#F4D03F,stroke-width:1.5px,color:#F4D03F,font-style:italic

    Operator[/Operator reviewing working friction from memory/]
    Decision[/Whether the report replaces that review/]

    subgraph Sealed["Baseline, sealed at 23:37:36"]
        TenItems[("Ten numbered frictions, written from memory")]
        Frozen{{"After the seal, no item can be added or removed"}}
        Rules{{"Classification method and verdict rules committed before any evidence"}}
        NoPrediction{{"No verdict predicted, so the outcome must come from the measurements"}}
    end

    subgraph Setup["What the run cost"]
        Existing("Existing Windows client, already installed")
        NoAdds{{"No account, no API key, no card, no dependency, no config file"}}
        StillMeters{{"Bounded claim: it still draws on the existing subscription allowance"}}
    end

    subgraph Report["The generated artifact, 23:45:11"]
        File[("The report file, created eight minutes after the seal")]
        Header[("Header captured at 23:46:42: 1,745 messages, 71 sessions")]
        ScopeFirst{{"Scope recorded before findings, so no friction shapes the baseline"}}
        Leak{{"Recorded exposure: one summary paragraph read before the scope record closed"}}
    end

    subgraph Compare["One row against one item"]
        NineRows[("Nine transcribed frictions, worded to be found again")]
        Confirmed("Marked Confirmed when a frozen item matches")
        New("Marked New with a stated reason no item fits")
        Unraised("Sealed items the report never raised, counted")
    end

    subgraph First["First pass"]
        Pass1[("4 confirmed, 5 new, 6 unraised")]
        Recon{{"Reconciled 4+5=9 against the rows and 4+6=10 against the list"}}
        Weak{{"One generous match left visible in its row rather than absorbed"}}
    end

    subgraph Audit["The close-match audit"]
        Reject("Rejected the broad-category to narrow-cause link")
        Moved{{"That row moved to New, its item returned to unraised"}}
        Pass2[("3 confirmed, 6 new, 7 unraised")]
        Against{{"The correction weakened the report, not the reviewer"}}
    end

    subgraph Verdict["The decision and its edges"]
        Watch{{"WATCH: useful findings, insufficient coverage"}}
        WhyNotAdopt{{"3 of 10 known frictions raised is too little to replace the review"}}
        WhyNotPark{{"6 new findings sit above the declared PARK line of 2 or fewer"}}
        Denominator{{"Open: report figures of 49 and 71 sessions leave the denominator unsettled"}}
    end

    Operator -- "writes and seals" --> TenItems
    TenItems -- "protected by" --> Frozen
    Operator -- "commits" --> Rules
    Rules -- "constrained by" --> NoPrediction
    Operator -- "runs the command on" --> Existing
    Existing -- "required" --> NoAdds
    NoAdds -- "bounded by" --> StillMeters
    Existing -- "produced" --> File
    Frozen -- "predates" --> File
    File -- "read for" --> Header
    Header -- "captured under" --> ScopeFirst
    ScopeFirst -- "qualified by" --> Leak
    File -- "transcribed into" --> NineRows
    NineRows -- "each tested against the sealed list, giving" --> Confirmed
    NineRows -- "each tested against the sealed list, giving" --> New
    TenItems -- "supplies the items for" --> Confirmed
    TenItems -- "unmatched items become" --> Unraised
    Confirmed -- "totals into" --> Pass1
    New -- "totals into" --> Pass1
    Unraised -- "totals into" --> Pass1
    Pass1 -- "checked by" --> Recon
    Pass1 -- "left one row flagged as" --> Weak
    Weak -- "re-examined by" --> Reject
    Reject -- "means" --> Moved
    Moved -- "corrects the totals to" --> Pass2
    Frozen -- "kept intact throughout" --> Reject
    Pass2 -- "moves the result" --> Against
    Pass2 -- "supports" --> Watch
    Pass2 -- "is why not adopt, per" --> WhyNotAdopt
    Pass2 -- "is why not park, per" --> WhyNotPark
    Header -- "leaves open" --> Denominator
    Leak -- "recorded as a limit on" --> Watch
    Denominator -- "is what a later report must settle for" --> Watch
    Watch -- "is" --> Decision
    NoPrediction -- "is why the verdict is traceable for" --> Decision

    class TenItems,File,Header,NineRows,Pass1,Pass2 datastore
    class Existing,Confirmed,New,Unraised,Reject service
    class Frozen,Rules,NoPrediction,NoAdds,StillMeters,ScopeFirst,Leak,Recon,Weak,Moved,Against,Watch,WhyNotAdopt,WhyNotPark,Denominator event
    class Operator,Decision io
```

The diagram shows the topology and data flow of the system as built. The captured evidence for this run, including screenshots of what the tools actually returned, lives in [`documents/insights-sealed-baseline.md`](./documents/insights-sealed-baseline.md).

## Implementation

This system is built across **6 phases**:

1. Reaching an Evidence-Based Routine Verdict
2. Establishing a Zero-Setup Evaluation Path
3. Sealing a Pre-Run Decision Record
4. Capturing the Report Scope Before Findings
5. Turning First Impressions Into Traceable Findings
6. Auditing the Most Ambiguous Match

For the run as it happened, with the screenshots taken at each step, see [`documents/insights-sealed-baseline.md`](./documents/insights-sealed-baseline.md).

## Validation

Each phase below has its evidence in [`documents/insights-sealed-baseline.md`](./documents/insights-sealed-baseline.md), with the screenshots, configuration, and observed results recorded as the run happened:

- ✅ Reaching an Evidence-Based Routine Verdict
- ✅ Establishing a Zero-Setup Evaluation Path
- ✅ Sealing a Pre-Run Decision Record
- ✅ Capturing the Report Scope Before Findings
- ✅ Turning First Impressions Into Traceable Findings
- ✅ Auditing the Most Ambiguous Match
