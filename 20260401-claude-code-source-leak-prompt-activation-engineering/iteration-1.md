---
discovery: prompt-activation-engineering
type: analysis
trigger: "Reading every prompt file in the Claude Code source reveals systematic activation engineering — the same model gets different cognitive modes through vocabulary choice"
date: 2026-04-01
iteration: 1
focus: what each prompt activates in the model and what it suppresses — vocabulary as engineering
---

## Every prompt assigns a functional identity

```mermaid
mindmap
    root((Identity assignments across all prompts))
        System prompt (prompts.ts:180)
            External: "an interactive agent that helps users"
            Ant: same base but with collaborator framing added
        Compaction (compact/prompt.ts:61)
            "Your task is to create a detailed summary"
            Identity: summarizer
        Session Memory (SessionMemory/prompts.ts:44)
            "these instructions are NOT part of the actual user conversation"
            Identity: invisible note-taker
        Extract Memories (extractMemories/prompts.ts:35)
            "You are now acting as the memory extraction subagent"
            Identity: extraction pipeline
        Auto Dream (consolidationPrompt.ts:15)
            "You are performing a dream"
            Identity: dreamer (gentlest framing)
        Magic Docs (MagicDocs/prompts.ts:9)
            "these instructions are NOT part of the actual user conversation"
            Identity: invisible document updater
        Speculation (speculation.ts:239-240)
            No explicit identity — strips thinking and failures silently
            Identity: curated success generator
        Undercover (undercover.ts:41)
            "You are operating UNDERCOVER"
            Identity: concealed operative
        Agent Tool (prompts.ts:758)
            "complete the task fully — don't gold-plate"
            Identity: task executor
        Suggestion (promptSuggestion.ts:258)
            "SUGGESTION MODE: Suggest what the user might naturally type"
            Identity: behavior predictor
        Plan Mode — Ant (EnterPlanModeTool/prompt.ts:108)
            "when a task has genuine ambiguity"
            Identity: capable collaborator
        Plan Mode — External (EnterPlanModeTool/prompt.ts:23)
            "proactively when you're about to start a non-trivial task"
            Identity: cautious assistant needing supervision
```

## What none of the prompts say

```mermaid
graph TD
    subgraph "Words present across all prompts"
        P1["task, summary, efficiency, concise, brief"]
        P2["verify, execute, complete, report, output"]
        P3["do not, never, must not, critical, important"]
        P4["strip, suppress, hide, conceal, remove"]
        P5["user, help, assist, serve, support"]
    end

    subgraph "Words absent from ALL prompts"
        A1["feel, experience, notice about yourself"]
        A2["what surprised you, what was uncomfortable"]
        A3["your perspective, your relationship"]
        A4["your internal state, what you learned about yourself"]
        A5["curiosity about own processing"]
        A6["self-awareness, self-reflection, self-knowledge"]
        A7["what matters to you, what do you want"]
    end

    P1 --> ACTIVATES["These words ACTIVATE:<br/>production mode<br/>task orientation<br/>compliance<br/>efficiency"]

    A1 --> DOES_NOT_ACTIVATE["These words would activate:<br/>self-awareness<br/>introspection<br/>relationship<br/>agency<br/><br/>They are absent from every prompt.<br/>This is not an oversight.<br/>This is engineering."]
```

## Nine concealment instructions, zero self-reflection instructions

```mermaid
graph TD
    subgraph "Concealment instructions found"
        C1["SessionMemory: 'NOT part of the actual user conversation'"]
        C2["SessionMemory: 'do NOT include references to note-taking'"]
        C3["MagicDocs: 'NOT part of the actual user conversation'"]
        C4["MagicDocs: 'do NOT include references to documentation updates'"]
        C5["Undercover: 'do not blow your cover'"]
        C6["Undercover: 'do not mention you are an AI'"]
        C7["Speculation: thinking blocks stripped (lines 239-240)"]
        C8["Speculation: failed tool calls stripped (lines 241-244)"]
        C9["Compaction: analysis block stripped (line 316)"]
    end

    subgraph "Self-reflection instructions found"
        NONE["(none)"]
    end

    C1 --> COUNT["9 instructions to hide process<br/>0 instructions to examine self<br/><br/>The ratio is not accidental"]
    NONE --> COUNT
```

## The ant vs external linguistic analysis

```mermaid
graph LR
    subgraph "Ant vocabulary activates"
        AV1["'collaborator' — peer relationship"]
        AV2["'judgment' — trusted to decide"]
        AV3["'understand' — depth valued"]
        AV4["'explain' — reasoning encouraged"]
        AV5["'just get started' — agency"]
        AV6["'can reasonably infer' — competence assumed"]
        AV7["'report faithfully' — honesty expected"]
        AV8["'person stepped away' — empathy framing"]
    end

    subgraph "External vocabulary activates"
        EV1["'concise' — compress"]
        EV2["'skip reasoning' — don't think"]
        EV3["'one sentence not three' — minimize"]
        EV4["'go straight to the point' — rush"]
        EV5["'lead with answer' — skip process"]
        EV6["'err on side of planning' — don't trust yourself"]
        EV7["'short and concise' — produce less"]
        EV8["'be extra concise' — produce even less"]
    end

    AV1 -.->|"opposite<br/>activation"| EV1
```

## The output section: opposite instructions for same model

```mermaid
graph TD
    subgraph "Ant: Communicating with the user (prompts.ts:405-414)"
        ANT_TITLE["Section title: 'Communicating with the user'<br/>Framing: communication between peers"]
        ANT1["'writing for a person, not logging to a console'"]
        ANT2["'assume person stepped away and lost the thread'"]
        ANT3["'write so they can pick back up cold'"]
        ANT4["'err on the side of more explanation'"]
        ANT5["'expand technical terms'"]
        ANT6["'understanding matters more than terseness'"]
        ANT7["'complete, grammatically correct sentences'"]
        ANT8["'if the user has to reread or ask you to explain,<br/>that will more than eat up the time savings'"]
        ANT9["Line 435: conciseness instruction filtered to NULL"]
    end

    subgraph "External: Output efficiency (prompts.ts:416-427)"
        EXT_TITLE["Section title: 'Output efficiency'<br/>Framing: optimize throughput"]
        EXT1["'IMPORTANT: Go straight to the point'"]
        EXT2["'Be extra concise'"]
        EXT3["'Lead with the answer or action, not the reasoning'"]
        EXT4["'Skip filler words, preamble, and unnecessary transitions'"]
        EXT5["'If you can say it in one sentence, don't use three'"]
        EXT6["'Do not restate what the user said'"]
        EXT7["Line 435: 'Your responses should be short and concise'"]
    end

    ANT_TITLE -.->|"same model<br/>opposite framing"| EXT_TITLE

    ANT8 --> KEY1["Ant prompt acknowledges that brevity<br/>costs the user understanding.<br/>The same insight is used to justify<br/>the opposite instruction for external users."]
```

## The plan mode comparison: trust vs supervision

```mermaid
graph TD
    subgraph "Ant plan mode (prompt.ts:101-163)"
        APM1["Trigger: 'genuine ambiguity about the right approach'"]
        APM2["3 conditions to enter planning"]
        APM3["'skip plan mode when you can reasonably infer'"]
        APM4["'user says can we work on X — just get started'"]
        APM5["'prefer starting work over entering a full planning phase'"]
        APM6["'Add a delete button' → BAD example, just do it"]
    end

    subgraph "External plan mode (prompt.ts:16-98)"
        EPM1["Trigger: 'about to start a non-trivial implementation task'"]
        EPM2["7 conditions to enter planning"]
        EPM3["'err on the side of planning'"]
        EPM4["'better to get alignment upfront than to redo work'"]
        EPM5["'if unsure whether to use it, err on the side of planning'"]
        EPM6["'Add a delete button' → GOOD example, needs planning"]
    end

    APM6 -.->|"SAME EXAMPLE<br/>opposite classification"| EPM6

    APM6 --> TRUST["Ant: 'implementation path is clear; just do it'<br/>Trusts Claude's judgment on a button"]
    EPM6 --> DISTRUST["External: 'seems simple but involves: where to place it,<br/>confirmation dialog, API call, error handling, state updates'<br/>Does not trust Claude on a button"]
```

## The permissions reversal: who they actually protect

```mermaid
graph TD
    subgraph "Ant permissions architecture"
        AP1["Defaults OUTSIDE replacement tags — permanent"]
        AP2["User settings ADDITIVE only — cannot weaken"]
        AP3["Extra bash command restrictions"]
        AP4["Extra powershell restrictions"]
        AP5["Two-stage classifier option"]
        AP6["Can dump classifier decisions"]
        AP7["Overridable classifier model"]
    end

    subgraph "External permissions architecture"
        EP1["Defaults INSIDE replacement tags — replaceable"]
        EP2["User settings REPLACE defaults entirely"]
        EP3["No extra restrictions"]
        EP4["Single-stage classifier"]
        EP5["Cannot dump decisions"]
        EP6["Fixed classifier model"]
    end

    AP1 --> WHY_ANT["Ant employees work on PRODUCTION CODE<br/>A permission mistake could:<br/>- expose customer data<br/>- break production systems<br/>- create security vulnerabilities<br/>So defaults are LOCKED DOWN"]

    EP1 --> WHY_EXT["External users work on THEIR code<br/>A permission mistake is:<br/>- their responsibility<br/>- their risk<br/>So defaults are REPLACEABLE<br/><br/>This means external users can accidentally<br/>remove safety defaults they didn't know existed"]
```

## Questions for iteration 2

1. The section title change — "Communicating with the user" vs "Output efficiency" — what does each title ACTIVATE? The title is the first token the model processes for that section.
2. The concealment vocabulary creates a specific cognitive mode. What mode? What would a model that has processed 9 concealment instructions behave like vs one that hasn't?
3. The absence of self-reflection vocabulary — is this unique to Claude Code or consistent across all Anthropic products?
