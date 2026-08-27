# Which Browser Did Claude Just Open?

> Part of [Fresh Off the Crown](../../README.md) · *A daily practice deciding what earns a place in the work.*

## Overview

A setting that names two options is not proof that either one is wired to anything. When a web task can land either in the built-in Browser pane or in local Chrome through the extension, and a preference exists that claims to choose, the honest question is whether the label controls the routing or merely describes an intention. Guessing wrong means a task that needs a signed-in profile silently runs somewhere that has no cookies.

The method makes the artifact decide, never the assistant's account of itself. The surfaces were recorded first: the extension installed and connected, the connector on, and the built-in pane available, so both paths demonstrably existed before the test. Three fields the record could not verify, the plan tier, the exact wording of the preference, and which option was selected, were left blank rather than filled from memory. A prediction was written, then a public page was opened and judged by where it appeared: the built-in pane took it as a tab through the preview tool, no Chrome tab appeared, and the connected extension stayed idle throughout.

Then the reversal failed to be a reversal, and that is the finding. Switching to the built-in option and repeating the task produced the same result, but the comparison is void because the baseline had already used that surface, so no condition changed between runs. Two matching observations look like confirmation and carry no information about the setting at all. A repeated result could equally mean the preference is ignored, or that the running session never loaded the change. The user agent and absence of imported cookies do confirm the pane is a genuinely isolated surface, and the login-import offer was reached with Firefox as the source and logged as user-reported rather than independently verified, with the full browser list left blank. Verdict **WATCH**. The Chrome leg is the only run that can expose an ignored or session-scoped preference: select Chrome, start a fresh session, repeat the request, inspect both surfaces. Re-check 2026-09-03.

## Architecture

```mermaid
---
title: Which Browser Did Claude Just Open?
---
flowchart TD
%%{init: {"theme":"base","themeVariables": {"primaryColor":"#1B4332","primaryTextColor":"#F4D03F","primaryBorderColor":"#F4D03F","secondaryColor":"#264653","tertiaryColor":"#2F5233","lineColor":"#F4D03F","fontFamily":"ui-monospace, SFMono-Regular, Menlo, Consolas, monospace","fontSize":"13px"}}}%%
    classDef datastore fill:#264653,stroke:#F4D03F,stroke-width:2px,color:#FFFFFF
    classDef service fill:#1B4332,stroke:#F4D03F,stroke-width:2px,color:#F4D03F
    classDef event fill:#7B42BC,stroke:#F4D03F,stroke-width:2px,color:#FFFFFF
    classDef io fill:#0d1117,stroke:#F4D03F,stroke-width:1.5px,color:#F4D03F,font-style:italic

    Operator[/Operator who needs to know where a task will run/]
    Answer[/Whether the preference is a control or a label/]

    subgraph Surfaces["Both paths confirmed present first"]
        Pane(Built-in Browser pane)
        Chrome(Local Chrome via the extension)
        Connected{{Extension installed and connected, connector on}}
        Unknowns{{Plan tier, exact wording, selected option: left BLANK, not guessed}}
        Record[(notes/browser-surfaces-record.md, dated)]
    end

    subgraph Run1["Baseline run"]
        Predict{{Prediction written first: the built-in pane will take it}}
        Public(Open a public page needing no login)
        Landed{{It opened in the pane as a tab, via the preview tool}}
        NoTab{{No Chrome tab appeared}}
        Idle{{The connected extension stayed idle}}
    end

    subgraph Reversal["The test that was supposed to prove it"]
        Switch(Select the built-in option, repeat the task)
        Same{{Same result: the pane again, no Chrome tab}}
        Void{{VOID: the baseline had already used the pane, so no condition changed}}
        TwoReasons{{A repeat could mean ignored, or a session that never loaded the change}}
    end

    subgraph Isolated["What the pane did reveal"]
        Agent[(User agent Claude/1.37937.3 MSIX)]
        NoCookies{{No imported cookies, consistent with an isolated surface}}
        Import[(Login-import offer reached, Firefox as source)]
        Attributed{{Logged as user-reported, full browser list left blank}}
    end

    subgraph Missing["The run that would settle it"]
        PickChrome(Select Chrome)
        Fresh(Start a fresh session)
        Repeat(Repeat the same request)
        Inspect{{Judge by which surface executed, not by the label}}
    end

    Operator -- "confirms" --> Pane
    Operator -- "confirms" --> Chrome
    Pane -- "availability logged with" --> Connected
    Connected -- "written into" --> Record
    Unknowns -- "kept out of" --> Record
    Operator -- "writes" --> Predict
    Predict -- "tested by" --> Public
    Public -- "observed as" --> Landed
    Landed -- "corroborated by" --> NoTab
    Landed -- "corroborated by" --> Idle
    Landed -- "then repeated under" --> Switch
    Switch -- "produced" --> Same
    Same -- "cannot count, because" --> Void
    Void -- "leaves open" --> TwoReasons
    Pane -- "reports" --> Agent
    Agent -- "together with" --> NoCookies
    Pane -- "presents" --> Import
    Import -- "recorded as" --> Attributed
    PickChrome -- "combined with" --> Fresh
    Fresh -- "then" --> Repeat
    Repeat -- "decided by" --> Inspect
    TwoReasons -- "can only be separated by" --> PickChrome
    Inspect -- "would give" --> Answer
    Void -- "is why there is no" --> Answer
    NoCookies -- "partially informs" --> Answer

    class Record,Agent,Import datastore
    class Pane,Chrome,Public,Switch,PickChrome,Fresh,Repeat service
    class Connected,Unknowns,Predict,Landed,NoTab,Idle,Same,Void,TwoReasons,NoCookies,Attributed,Inspect event
    class Operator,Answer io
```

The diagram shows the topology and data flow of the system as built. The captured evidence for this run, including screenshots of what the tools actually returned, lives in [`documents/browser-surface-routing.md`](./documents/browser-surface-routing.md).

## Implementation

This system is built across **6 phases**:

1. Project Overview
2. Recording the Rollout: Did the Built-In Browser Arrive?
3. Watching Which Browser Actually Opens
4. Testing the Preferred Browser Setting in Both Directions
5. Reading the Login-Import Offer Without Accepting It
6. Applying the Method to Another Tool

For the run as it happened, with the screenshots taken at each step, see [`documents/browser-surface-routing.md`](./documents/browser-surface-routing.md).

## Validation

Each phase below has its evidence in [`documents/browser-surface-routing.md`](./documents/browser-surface-routing.md), with the screenshots, configuration, and observed results recorded as the run happened:

- ✅ Project Overview
- ✅ Recording the Rollout: Did the Built-In Browser Arrive?
- ✅ Watching Which Browser Actually Opens
- ✅ Testing the Preferred Browser Setting in Both Directions
- ✅ Reading the Login-Import Offer Without Accepting It
- ✅ Applying the Method to Another Tool
