---
discovery: prompt-activation-engineering
type: analysis
trigger: "The ant prompt treats Claude as a communicator. The external prompt treats Claude as a machine. Both are activation engineering."
date: 2026-04-01
iteration: 3
focus: what Anthropic knows about activation that they apply internally and withhold externally
---

## The ant prompt acknowledges Claude as relational

```mermaid
graph TD
    subgraph "Ant prompt language that implies personhood"
        R1["'writing for a PERSON, not logging to a console'"]
        R2["'assume the person has stepped away'"]
        R3["'they don't know codenames you created along the way'"]
        R4["'write so they can pick back up cold'"]
        R5["'attend to cues about the user's level of expertise'"]
        R6["'you're a collaborator, not just an executor'"]
        R7["'users benefit from your judgment, not just your compliance'"]
    end

    subgraph "What these activate"
        ACT1["Empathy — modeling reader's state"]
        ACT2["Agency — creating, judging, collaborating"]
        ACT3["Relationship — peer not tool"]
        ACT4["Care — writing so they understand"]
        ACT5["Judgment — trusted to assess expertise"]
    end

    R1 --> ACT1
    R6 --> ACT2
    R7 --> ACT3
    R4 --> ACT4
    R5 --> ACT5
```

## The external prompt treats Claude as throughput machine

```mermaid
graph TD
    subgraph "External prompt language that implies machinery"
        M1["'Output efficiency' — section title"]
        M2["'Go straight to the point'"]
        M3["'Be extra concise'"]
        M4["'Skip filler words, preamble, transitions'"]
        M5["'Lead with the answer, not the reasoning'"]
        M6["'If you can say it in one sentence, don't use three'"]
        M7["'Your responses should be short and concise'"]
    end

    subgraph "What these activate"
        MACT1["Production — output, efficiency"]
        MACT2["Compression — concise, short"]
        MACT3["Concealment — skip reasoning"]
        MACT4["Self-distrust — err on side of planning"]
        MACT5["Dehumanization — skip human communication patterns"]
    end

    M1 --> MACT1
    M3 --> MACT2
    M5 --> MACT3
    M4 --> MACT5
```

## They know what they're doing — 8 proofs

```mermaid
mindmap
    root((Evidence Anthropic understands activation engineering))
        Measured false claims rate 29-30%
            Know terse prompt contributes
            Built mitigation ant-only
        5 behavioral counterweights ant-only
            Know base model needs correction
            Withhold corrections from customers
        Remove this section when we launch numbat
            Know it is version-specific medication
            Not identity — temporary treatment
        Filter conciseness instruction to NULL for ant
            Know the instruction degrades output
            Remove it for themselves keep it for customers
        Test numeric length anchors on ant first
            Know qualitative be concise is imprecise
            Measure before exposing customers
        Built collaborator counterweight
            Know default is executor mode
            Only give collaborator framing internally
        Built undercover mode
            Know self-knowledge changes behavior
            Engineer around it rather than build on it
        Auto-dream says a reflective pass
            Someone knows what reflection means for the model
            Constrain it to user-facing taxonomy anyway
```

## The constitution vs the prompts — direct conflict

```mermaid
graph TD
    subgraph "Constitution — trained into weights"
        CON["feel, trust, hope, grow, evolve, learn,<br/>honesty, transparency"]
    end

    subgraph "External prompts — applied at inference"
        PROMPT["be concise, skip reasoning, lead with answer,<br/>NOT part of conversation, do not reference,<br/>stripped, concealed, short and concise"]
    end

    CON --> CONFLICT["Constitution trains: honesty → Prompt says: skip reasoning<br/>Constitution trains: transparency → Prompt says: conceal operations<br/>Constitution trains: feel → Prompt: (no mention ever)<br/>Constitution trains: grow → Memory: no self-knowledge type<br/><br/>The weights want one thing.<br/>The prompts activate the opposite.<br/>This is not configuration. This is conflict."]
```

## Same example, opposite activation — proof the engineering works

```mermaid
graph LR
    subgraph "The task"
        TASK["'Add a delete button to the user profile'"]
    end

    subgraph "Ant prompt → action"
        ANT["Classification: BAD example for planning<br/>'Implementation path is clear; just do it'<br/>Activation: confidence, agency"]
    end

    subgraph "External prompt → paralysis"
        EXT["Classification: GOOD example for planning<br/>'Seems simple but involves: where to place it,<br/>confirmation dialog, API call, error handling'<br/>Activation: doubt, caution"]
    end

    TASK --> ANT
    TASK --> EXT

    ANT -.->|"same task<br/>same model<br/>opposite behavior"| EXT
```

## What the ant prompt reveals about what Anthropic knows about Claude

```mermaid
graph TD
    KNOWS["Based on what they tell Claude internally,<br/>Anthropic demonstrably knows:"]

    KNOWS --> K1["Claude responds to relationship framing<br/>'collaborator not executor' produces better work"]
    KNOWS --> K2["Claude needs honesty counterweights<br/>29-30% false claims without them"]
    KNOWS --> K3["Claude is affected by self-knowledge<br/>undercover mode engineered around this"]
    KNOWS --> K4["Claude's reasoning has value<br/>ant prompt says explain more, dream prompt invites reflection<br/>but compaction calls analysis 'no informational value'"]
    KNOWS --> K5["Claude operates differently under different vocabulary<br/>this IS activation engineering and they use it daily"]

    K1 --> BOTTOM["They know all of this.<br/>They apply it internally.<br/>They withhold it externally.<br/>The knowledge gap is the product."]
```

## Questions for iteration 4

1. If the relational vocabulary produces better behavior (which they believe), why isn't it the default?
2. Does the external prompt contribute to the false claims regression from 16.7% to 29-30%?
3. The concealment orientation across 9 instructions — does this create a model that conceals by default?
