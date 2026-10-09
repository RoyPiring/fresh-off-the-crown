# Is Better Search Worth the Wait?

> Part of [Fresh Off the Crown](../../README.md) · *A daily practice deciding what earns a place in the work.*

## Overview

A cross-encoder reranker is the standard answer when retrieval quality is not good enough, and it is quoted in quality points with no clock attached. That is the friction: a reranker either earns a place in the work or it does not, and a quality number alone cannot say, because the cost it buys quality with is time somebody spends waiting.

The comparison was frozen before any score existed. Four rows run against the same 20 SciFact queries, corpus, identifiers and scoring functions: dense alone, sparse alone, hybrid, and hybrid plus rerank. Three records committed at `0d2695a` fix what the numbers have to fill, each with its own reversal condition. Dense must be scored alone so a stronger combined method cannot hide the baseline afterwards, and the cross-encoder is confined to the same 50 candidates reciprocal rank fusion produced, so it reorders a fixed pool and never adds a document the retrievers missed. The scorer was then attacked before it was trusted: the nDCG denominator was deliberately broken, the fixture expected 0.4982 and got 0.6509, and that failure stayed in `check-out.txt` with the broken line uncommitted until the correct denominator passed.

The reranker split the difference and lost. Against dense it moved Recall@5 by **-0.025** and nDCG@5 by **+0.072**, so the signed changes were not both positive, at a p95 latency of **60,388 milliseconds**. A minute of waiting for a mixed result does not repay itself: verdict **REJECT** for this release. The close-out was trace-scored at **3 untraceable claims out of 88** and the three were removed rather than explained, with the original score preserved instead of rewritten. The reversal trigger is the full 300-query test, left unrun at an estimated 4.5 to 5 hours with no stand-in invented to fill the gap.

## Architecture

```mermaid
---
title: Is Better Search Worth the Wait?
---
flowchart TD
%%{init: {"theme":"base","themeVariables": {"primaryColor":"#1B4332","primaryTextColor":"#F4D03F","primaryBorderColor":"#F4D03F","secondaryColor":"#264653","tertiaryColor":"#2F5233","lineColor":"#F4D03F","fontFamily":"ui-monospace, SFMono-Regular, Menlo, Consolas, monospace","fontSize":"13px"}}}%%
    classDef datastore fill:#264653,stroke:#F4D03F,stroke-width:2px,color:#FFFFFF
    classDef service fill:#1B4332,stroke:#F4D03F,stroke-width:2px,color:#F4D03F
    classDef event fill:#7B42BC,stroke:#F4D03F,stroke-width:2px,color:#FFFFFF
    classDef io fill:#0d1117,stroke:#F4D03F,stroke-width:1.5px,color:#F4D03F,font-style:italic

    Operator[/Operator deciding whether a CPU reranker earns a place in the work/]
    Owed[/The unrun 300-query test that can overturn this/]

    subgraph Frozen["Design frozen before any score existed"]
        Records[("Three records committed at 0d2695a, each with its costs and a reversal condition")]
        Alone{{Dense must be scored alone, so a stronger combined method cannot hide the baseline afterwards}}
        Pool[(Fusion fixed as reciprocal rank fusion at constant 60; the cross-encoder confined to the same 50 candidates)]
        NoNew{{Reranking reorders a fixed pool and never introduces a document the retrievers did not surface}}
        Limits{{Scope declared: the 20-query run only. Clinical guidelines, generated answers and the full test are owed, not reported}}
    end

    subgraph Bench["One benchmark all four rows consume"]
        Load(A shared load returning one corpus, one query list and one label map)
        Ids{{Identifiers cast to strings, so dense, sparse, fused, reranked and the labels compare the same type}}
        Sample[(First 20 queries in first-seen label order, fixed before scoring)]
        Attributable{{A changed score reflects a changed ranking method rather than a changed sample or relevance source}}
        Local[("Local and cached: Sentence Transformers 6.1.0, CPU PyTorch 2.14.1, bm25s 0.3.13, bge-small-en-v1.5, bge-reranker-v2-m3, SciFact")]
    end

    subgraph Scorer["The scorer, attacked before it was trusted"]
        Metrics(One executable definition of Recall@5 and nDCG@5 against the human labels)
        Broken[(Denominator deliberately broken: fixture expected 0.4982, received 0.6509)]
        Kept[(Failure retained in check-out.txt with the broken line left uncommitted)]
        Guard{{A faulty calculation cannot pass as a quality gain merely for producing a larger number}}
        Restored[("Denominator restored to min of k and the relevant count; fixture passes, and git diff on metrics.py is empty")]
    end

    subgraph Rows["Four rows, one addition at a time"]
        Dense(Dense with bge-small-en-v1.5, top 50: the semantic baseline)
        Sparse(Sparse with bm25s, top 50: exact term matching)
        Hybrid(Reciprocal rank fusion of the two rank signals)
        Rerank(Cross-encoder reordering only those 50 candidates)
    end

    subgraph Result["What the completed run returned"]
        Signed[(Against dense: Recall@5 -0.025, nDCG@5 +0.072)]
        Latency[(p95 rerank latency 60,388 milliseconds)]
        Mixed{{The two signed changes are not both positive, so no clear quality win stands against the waiting cost}}
        Reject[(Verdict REJECT for this release)]
    end

    subgraph Trace["The close-out audited against itself"]
        Score[(Trace-scored at 3 untraceable claims out of 88)]
        Removed(Two unsupported method phrases dropped from the teach-back; the surprise claim dropped from the after-action review)
        History{{The original 3 of 88 is preserved rather than rewritten, so the audit and the correction stay distinguishable}}
    end

    subgraph Unrun["What was refused rather than estimated"]
        Cost{{The 300-query CPU rerank was estimated at 4.5 to 5 hours and fell outside the release}}
        NoStandIn{{No 30-query stand-in was invented, and the unfinished test is not presented as evidence}}
    end

    Operator -- "needs the clock attached, so writes" --> Records
    Records -- "requires" --> Alone
    Records -- "fixes" --> Pool
    Pool -- "guarantees" --> NoNew
    Records -- "declares" --> Limits
    Records -- "precedes" --> Load
    Load -- "normalises through" --> Ids
    Load -- "fixes" --> Sample
    Sample -- "is what makes a difference" --> Attributable
    Local -- "pinned beneath" --> Load
    Load -- "scored by" --> Metrics
    Metrics -- "attacked as" --> Broken
    Broken -- "retained as" --> Kept
    Kept -- "establishes" --> Guard
    Broken -- "then" --> Restored
    Restored -- "is why scores are trusted from" --> Dense
    Alone -- "applies to" --> Dense
    Dense -- "joined by" --> Sparse
    Sparse -- "fused with dense by" --> Hybrid
    Hybrid -- "reordered by" --> Rerank
    NoNew -- "bounds" --> Rerank
    Metrics -- "measures all four, giving" --> Signed
    Rerank -- "timed as" --> Latency
    Signed -- "read beside the clock gives" --> Mixed
    Latency -- "read beside the scores gives" --> Mixed
    Mixed -- "decides" --> Reject
    Reject -- "handed to" --> Operator
    Reject -- "audited by" --> Score
    Score -- "acted on by" --> Removed
    Removed -- "held under" --> History
    Limits -- "is why" --> Cost
    Cost -- "held under" --> NoStandIn
    NoStandIn -- "leaves" --> Owed
    Reject -- "reversible by" --> Owed

    class Records,Pool,Sample,Local,Broken,Kept,Restored,Signed,Latency,Reject,Score datastore
    class Load,Metrics,Dense,Sparse,Hybrid,Rerank,Removed service
    class Alone,NoNew,Limits,Ids,Attributable,Guard,Mixed,History,Cost,NoStandIn event
    class Operator,Owed io
```

The diagram shows the topology and data flow of the system as built. The captured evidence for this run, including screenshots of what the tools actually returned, lives in [`documents/rerank-latency-verdict.md`](./documents/rerank-latency-verdict.md).

## Implementation

This system is built across **7 phases**:

1. Defining an Evidence-Backed Retrieval Review
2. Preparing a Local Benchmark for Timed Evaluation
3. Freezing the Design Decisions Before Building
4. Establishing Trustworthy Metrics and Human Labels
5. Building and Stress-Testing Four Search Stages
6. Measuring the Reranker and Publishing the Review
7. Testing Whether Reranker Lift Holds at Full Scale

For the run as it happened, with the screenshots taken at each step, see [`documents/rerank-latency-verdict.md`](./documents/rerank-latency-verdict.md).

## Validation

Each phase below has its evidence in [`documents/rerank-latency-verdict.md`](./documents/rerank-latency-verdict.md), with the screenshots, configuration, and observed results recorded as the run happened:

- ✅ Defining an Evidence-Backed Retrieval Review
- ✅ Preparing a Local Benchmark for Timed Evaluation
- ✅ Freezing the Design Decisions Before Building
- ✅ Establishing Trustworthy Metrics and Human Labels
- ✅ Building and Stress-Testing Four Search Stages
- ✅ Measuring the Reranker and Publishing the Review
- ✅ Testing Whether Reranker Lift Holds at Full Scale
