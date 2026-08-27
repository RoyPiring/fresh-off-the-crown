# Measure Your Own Word Error Rate

> Part of [Fresh Off the Crown](../../README.md) · *A daily practice deciding what earns a place in the work.*

## Overview

A published benchmark is measured on someone else's voice, vocabulary, microphone, and room. Trusting it as a personal number is the friction: dictation either earns a place in daily work or it does not, and 4.0% on clean speech says nothing about a passage carrying terms like NextWork and jiwer spoken into this microphone.

The design protects the ground truth in three places. The reference passage is written and sealed before any audio exists, because editing it after seeing the transcript would make errors vanish and measure the corrections rather than the system. Three predictions are sealed the same way, including an estimate of 8.5%. The passage is read once, unrehearsed, with no retakes, since a second take measures practice rather than a cold reading. And the raw output is saved untouched, because repairing spelling or word order before scoring removes exactly the substitutions, insertions, and deletions the score exists to count. Scoring runs through jiwer with word-level alignment, so the number decomposes instead of arriving as a verdict.

The measured rate was **16.5%**, eight points above the prediction and twelve and a half above the published benchmark. Two of the three predictions were wrong, which is the value of having sealed them. The sensitivity check is what makes the entry worth keeping: a plain-language passage scored 15.4% against the jargon passage's 16.5%, a gap of only 1.1 points, while a second jargon take scored 24.2%, a swing of 7.7 points on the same kind of material. Vocabulary is not the driver; how the recording was captured is. All three runs were deletion-heavy, so missing text moves the rate as much as word choice, and clipping could account for as much as the model. Verdict **WATCH**: not adoptable at 16.5%, plausibly fixable since the evidence points at capture rather than the model. Re-check 2026-09-05 with capture held constant. Reversed to reject if a controlled-capture rerun stays near 16.5%.

## Architecture

```mermaid
---
title: Measure Your Own Word Error Rate
---
flowchart TD
%%{init: {"theme":"base","themeVariables": {"primaryColor":"#1B4332","primaryTextColor":"#F4D03F","primaryBorderColor":"#F4D03F","secondaryColor":"#264653","tertiaryColor":"#2F5233","lineColor":"#F4D03F","fontFamily":"ui-monospace, SFMono-Regular, Menlo, Consolas, monospace","fontSize":"13px"}}}%%
    classDef datastore fill:#264653,stroke:#F4D03F,stroke-width:2px,color:#FFFFFF
    classDef service fill:#1B4332,stroke:#F4D03F,stroke-width:2px,color:#F4D03F
    classDef event fill:#7B42BC,stroke:#F4D03F,stroke-width:2px,color:#FFFFFF
    classDef io fill:#0d1117,stroke:#F4D03F,stroke-width:1.5px,color:#F4D03F,font-style:italic

    Speaker[/Operator deciding whether to dictate daily/]
    Verdict[/A rate measured on this voice, in this room/]

    subgraph Published["The number that does not transfer"]
        Bench[(Published benchmark 4.0%)]
        CleanSpeech{{Measured on other voices, other rooms, common vocabulary}}
    end

    subgraph Sealed["Ground truth, locked before audio"]
        Reference[(Sealed jargon passage: NextWork, jiwer)]
        WhySeal{{Editing it later would measure the corrections, not the system}}
        Predictions[(predictions.txt, three of them)]
        Guess[(Estimate: 8.5%)]
    end

    subgraph Capture["One cold reading"]
        Cli(Antigravity CLI, /voice)
        NoKey{{No separate API key: the signed-in account sufficed}}
        OneTake{{One unrehearsed reading, no retakes, so practice cannot lower it}}
        Hypothesis[(hypothesis.txt, saved raw)]
        Untouched{{No spelling, punctuation or word-order repair before scoring}}
    end

    subgraph Score["Decomposed, not just totalled"]
        Jiwer(jiwer word-level alignment)
        Subs{{Substitutions, insertions, deletions counted separately}}
        Measured[(Measured WER 16.5%)]
        Gap{{8.0 points above the prediction, 12.5 above the benchmark}}
    end

    subgraph Sensitivity["The check that refuted the obvious story"]
        Plain[(Plain passage 15.4%)]
        Jargon2[(Second jargon take 24.2%)]
        Weak{{Vocabulary moved it 1.1 points}}
        Strong{{Capture moved it 7.7 points}}
        DeletionHeavy{{All three runs deletion-heavy, so missing text dominates}}
        NotVocab{{The evidence does not support blaming jargon}}
    end

    Speaker -- "cannot rely on" --> Bench
    Bench -- "because it is" --> CleanSpeech
    Speaker -- "writes and seals" --> Reference
    Reference -- "protected by" --> WhySeal
    Speaker -- "seals" --> Predictions
    Predictions -- "includes" --> Guess
    Reference -- "read once into" --> Cli
    Cli -- "ran under" --> NoKey
    Cli -- "constrained by" --> OneTake
    Cli -- "emits" --> Hypothesis
    Hypothesis -- "preserved by" --> Untouched
    Reference -- "compared by" --> Jiwer
    Hypothesis -- "compared by" --> Jiwer
    Jiwer -- "reports" --> Subs
    Jiwer -- "produces" --> Measured
    Guess -- "missed by" --> Gap
    Bench -- "exceeded by" --> Gap
    Measured -- "contrasted with" --> Plain
    Measured -- "contrasted with" --> Jargon2
    Plain -- "yields" --> Weak
    Jargon2 -- "yields" --> Strong
    Subs -- "shows" --> DeletionHeavy
    Weak -- "together with Strong gives" --> NotVocab
    Strong -- "outweighs Weak, giving" --> NotVocab
    DeletionHeavy -- "supports" --> NotVocab
    Measured -- "is the headline of" --> Verdict
    NotVocab -- "redirects the fix behind" --> Verdict
    OneTake -- "bounds the claim in" --> Verdict

    class Bench,Reference,Predictions,Guess,Hypothesis,Measured,Plain,Jargon2 datastore
    class Cli,Jiwer service
    class CleanSpeech,WhySeal,NoKey,OneTake,Untouched,Subs,Gap,Weak,Strong,DeletionHeavy,NotVocab event
    class Speaker,Verdict io
```

The diagram shows the topology and data flow of the system as built. The captured evidence for this run, including screenshots of what the tools actually returned, lives in [`documents/personal-word-error-rate.md`](./documents/personal-word-error-rate.md).

## Implementation

This system is built across **6 phases**:

1. Measuring My Own Word Error Rate
2. Setting Up the Tools
3. Sealing the Reference and Writing Predictions
4. Testing Dictation Cold
5. Recording the Passage Once
6. Proving the Measurement Is Sensitive

For the run as it happened, with the screenshots taken at each step, see [`documents/personal-word-error-rate.md`](./documents/personal-word-error-rate.md).

## Validation

Each phase below has its evidence in [`documents/personal-word-error-rate.md`](./documents/personal-word-error-rate.md), with the screenshots, configuration, and observed results recorded as the run happened:

- ✅ Measuring My Own Word Error Rate
- ✅ Setting Up the Tools
- ✅ Sealing the Reference and Writing Predictions
- ✅ Testing Dictation Cold
- ✅ Recording the Passage Once
- ✅ Proving the Measurement Is Sensitive
