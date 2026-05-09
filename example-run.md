# Example Run: Canton Fair Buyer Search Patterns

This file shows what a realistic run of `Path Probe / 用户路径探针` should look like.

The goal is not to show every internal artifact in full.

The goal is to show:

- what kind of messy inputs the skill can accept
- how the skill structures them
- how it separates evidence / inference / unknown
- how the final deliverable becomes product-usable

This example uses the Canton Fair buyer material discussed in the thread.

---

## 1. Example Input

This is the kind of messy input a researcher might paste into the skill.

```md
Study topic: Canton Fair buyer behavior
Decision job: How overseas buyers search for products or suppliers in China before deciding what to do next
Focus stage: search
Platforms: Reddit

Raw materials:

1) Reddit thread: "1st Time Canton Fair with no Experience"

Original poster says:
"I have no prior Ecommerce experience... To be honest, budget isn't a major issue. It was more figuring out what product appeals to me... then working with a supplier that can create that."

Notes:
- likely total beginner
- not budget constrained
- unclear category
- probably looking for direction before commitment

Comments I copied:
- people say not to rely only on translation apps
- people suggest phase 2 and phase 3 are more beginner-friendly
- concern around MOQ and asking bigger factories for smaller orders

2) Reddit thread: "How can I develop a product in China?"

Original poster says:
"I just got back from the Canton Fair, and I wasn't expecting it to be this hard to find someone who can help me develop a new version of a product... Most of the people I met turned out to be middlemen. I did multiple 7 figures with it last winter so Im ordering at least 50,000 units to start."

Notes:
- experienced Amazon seller
- not there to buy stock goods
- wants product development
- frustrated by middlemen
- needs source factory with R&D capability

More copied summary:
- needs CAD / engineering support
- wants supplier who can collaborate on new version
```

---

## 2. Research Frame

This is the first section the skill should produce after normalizing the task.

```md
## Research Frame
- Topic: Canton Fair buyer search behavior
- Decision Job: How overseas buyers search for products, suppliers, or development capability in China
- Focus Stage: Search and early validation
- Product Team Goal: Identify distinct search patterns and current replacement flows that can guide search, verification, and supplier capability design
- Assumptions:
  - Current material is Reddit-only
  - Sample size is small
  - Output should be treated as pattern candidates unless further validated
```

---

## 3. Intake Summary

The skill should not ask the researcher to pre-clean everything.

It should first split the messy material into intake items.

```md
## Intake Summary
- Total intake items: 8
- Mixed items needing caution: 3
- Unknown platform items: 0
- Notes:
  - Some chunks mix direct quotes and researcher interpretation
  - Comment-level context is incomplete in several places
  - No timestamps were preserved in the pasted materials
```

Example internal intake items:

```json
[
  {
    "intake_id": "INTAKE-01",
    "platform_guess": "reddit",
    "content_type_guess": "post",
    "raw_chunk": "I have no prior Ecommerce experience...",
    "contains_user_voice": true,
    "contains_researcher_voice": false,
    "parse_confidence": "high",
    "warnings": []
  },
  {
    "intake_id": "INTAKE-02",
    "platform_guess": "reddit",
    "content_type_guess": "mixed",
    "raw_chunk": "people say not to rely only on translation apps...",
    "contains_user_voice": false,
    "contains_researcher_voice": true,
    "parse_confidence": "medium",
    "warnings": ["comment paraphrase, original wording missing"]
  }
]
```

---

## 4. Research Pack Summary

This is the structured layer after intake.

```md
## Research Pack Summary
- Sources normalized: 8
- High completeness sources: 3
- Medium completeness sources: 3
- Low completeness sources: 2
- Most common missing fields:
  - timestamp
  - full comment context
  - exact author role beyond self-description
```

Example normalized source entries:

```json
[
  {
    "source_id": "SRC-01",
    "platform": "reddit",
    "url": "unknown",
    "capture_type": "post",
    "raw_content": "I have no prior Ecommerce experience... To be honest, budget isn't a major issue. It was more figuring out what product appeals to me... then working with a supplier that can create that.",
    "quote_blocks": [
      {
        "quote_id": "Q-01",
        "speaker_type": "user",
        "text": "I have no prior Ecommerce experience...",
        "position": "opening post"
      }
    ],
    "researcher_note": "likely total beginner; probably looking for direction before commitment",
    "author_context": {
      "self_described_role": "no prior ecommerce experience",
      "experience_level_guess": "newbie",
      "use_case_hint": "wants to start a DTC or ecommerce business"
    },
    "timestamp": "",
    "language": "en",
    "completeness": "high",
    "missing_fields": ["url", "timestamp"]
  },
  {
    "source_id": "SRC-05",
    "platform": "reddit",
    "url": "unknown",
    "capture_type": "post",
    "raw_content": "I just got back from the Canton Fair... Most of the people I met turned out to be middlemen. I did multiple 7 figures with it last winter so Im ordering at least 50,000 units to start.",
    "quote_blocks": [
      {
        "quote_id": "Q-05",
        "speaker_type": "user",
        "text": "Most of the people I met turned out to be middlemen.",
        "position": "middle of post"
      }
    ],
    "researcher_note": "experienced seller looking for development partner, not stock supplier",
    "author_context": {
      "self_described_role": "successful seller with large order size",
      "experience_level_guess": "expert",
      "use_case_hint": "new version product development"
    },
    "timestamp": "",
    "language": "en",
    "completeness": "high",
    "missing_fields": ["url", "timestamp"]
  }
]
```

---

## 5. Evidence Audit

This is where the skill starts getting sharp.

It decides what is usable, what is weak, and what needs caution.

```md
## Evidence Audit
- High-quality sources: 5
- Medium-quality sources: 2
- Low-quality sources: 1
- Strong search-signal sources: 4

### Main Audit Risks
- Several copied comments are paraphrases, not verbatim
- No timestamp context, so recency cannot be judged
- All materials come from Reddit, which may over-represent deep-research users
```

Example audit output:

```json
[
  {
    "source_id": "SRC-01",
    "evidence_health": "high",
    "directness": "firsthand",
    "context_completeness": "medium",
    "decision_signal_strength": "strong",
    "behavioral_signal_present": true,
    "search_signal_present": true,
    "trust_signal_present": false,
    "outcome_signal_present": false,
    "risk_flags": ["missing_time_context"],
    "audit_reasoning": "firsthand statement of search intent and lack of prior experience"
  },
  {
    "source_id": "SRC-06",
    "evidence_health": "medium",
    "directness": "unclear",
    "context_completeness": "low",
    "decision_signal_strength": "medium",
    "behavioral_signal_present": false,
    "search_signal_present": true,
    "trust_signal_present": true,
    "outcome_signal_present": false,
    "risk_flags": ["researcher_paraphrase_only", "comment_without_parent_context"],
    "audit_reasoning": "useful signal, but copied as summary rather than original wording"
  }
]
```

---

## 6. Evidence Unit Summary

This is where the skill slices the sources into reusable signals.

```md
## Evidence Unit Summary
- Evidence units extracted: 8
- Search-entry signals: 4
- Trust-source signals: 2
- Friction signals: 5
- Workaround signals: 3
```

Example evidence units:

```json
[
  {
    "evidence_id": "E01",
    "source_id": "SRC-01",
    "verbatim_text": "I have no prior Ecommerce experience...",
    "normalized_text": "user is entering search without prior ecommerce operating experience",
    "journey_stage": "search_start",
    "signal_tags": ["search_entry", "decision_state"],
    "signal_payload": {
      "search_entry": ["open-ended exploration"],
      "decision_state": "continue"
    },
    "strength": "high",
    "audit_weight": "high"
  },
  {
    "evidence_id": "E02",
    "source_id": "SRC-01",
    "verbatim_text": "budget isn't a major issue... figuring out what product appeals to me",
    "normalized_text": "user is not constrained by budget but is constrained by direction-finding",
    "journey_stage": "search_start",
    "signal_tags": ["friction", "comparison_axis"],
    "signal_payload": {
      "comparison_axis": ["product appeal", "category choice"],
      "friction": ["too many possible directions"]
    },
    "strength": "high",
    "audit_weight": "high"
  },
  {
    "evidence_id": "E06",
    "source_id": "SRC-05",
    "verbatim_text": "Most of the people I met turned out to be middlemen.",
    "normalized_text": "user cannot verify source factory capability and keeps encountering intermediaries",
    "journey_stage": "validate",
    "signal_tags": ["friction", "distrust_source"],
    "signal_payload": {
      "distrust_source": ["middlemen"],
      "friction": ["supplier type opacity", "capability uncertainty"]
    },
    "strength": "high",
    "audit_weight": "high"
  }
]
```

---

## 7. Next Evidence To Collect

This phase is mandatory.

The point is to keep the skill honest and make the next round of research obvious.

```md
## Next Evidence To Collect
1. Need cross-platform confirmation for beginner search behavior
   - Why it matters: current pattern may overfit Reddit-style long-form help-seeking
   - Go collect: YouTube Canton Fair vlog comments, Xiaohongshu sourcing notes
   - Look for: users asking where to start, how to narrow categories, what to do first

2. Need end-state evidence for experienced buyers
   - Why it matters: we know they get frustrated, but we do not know how they eventually solve the problem
   - Go collect: follow-up Reddit comments, sourcing community discussions, YouTube comments
   - Look for: "what I did instead", "how I found a real factory", "why I stopped using the fair"

3. Need counterexamples to the current two-pattern model
   - Why it matters: there may be a price-first or relationship-first pattern missing from this sample
   - Go collect: broader Reddit thread set, buying-agent discussion groups
   - Look for: users whose main search logic is neither exploration nor capability validation
```

---

## 8. Current Replacement Flows

This is the reconstructed flow layer.

```md
## Current Replacement Flows

### Flow A: Direction-Seeking Explorer
1. User wants to start something but has no product direction
2. User searches for guidance on where to begin
3. User relies on experienced commenters to narrow down categories or fair phases
4. User starts worrying about MOQ, communication, and whether any supplier will take them seriously
5. User uses translation apps, community advice, and in-person browsing as patchwork support
6. User either keeps exploring or makes a low-confidence next step

Where the flow breaks:
- The user does not know how to narrow the search space
- The user cannot easily judge supplier fit for a small first move

What a better product would replace:
- Random wandering plus forum-based advice stitching

### Flow B: Capability-Seeking Customizer
1. User already has demand and wants a better version of an existing product
2. User searches for a partner who can do real development, not just fulfillment
3. User talks to many exhibitors or suppliers
4. User repeatedly encounters middlemen or non-R&D factories
5. User cannot reliably verify who can co-develop the product
6. User leaves with frustration and likely searches outside the fair

Where the flow breaks:
- Supplier capability is opaque
- Source factory versus middleman is hard to distinguish

What a better product would replace:
- Manual trial-and-error supplier qualification
```

---

## 9. Decision Pattern Summary

This is the first place where product people should feel real leverage.

```md
## Decision Pattern Summary

| Pattern | Behavioral definition | Search start | Trust anchor | Main blocker | Opportunity | Confidence |
|--------|------------------------|-------------|-------------|-------------|------------|------------|
| Direction-Seeking Explorer | searching for what to do before searching for who to buy from | "where do I even start?" | experienced peer guidance | cannot narrow options or negotiate a realistic starting point | guided narrowing and beginner-fit supplier search | High |
| Capability-Seeking Customizer | searching for verifiable development capability, not just supply | "who can build the next version?" | proof of real R&D ability | repeated middleman encounters and capability ambiguity | supplier capability verification layer | High |
```

---

## 10. Recommended Product Direction

This is the translation layer. It should be specific, not fluffy.

```md
## Recommended Product Direction

### Primary Recommendation
Build a dual-path search and verification system that splits users by intent before showing supplier options.

### We Recommend This Because
- One pattern begins with open-ended direction finding, not supplier selection. [E:E01,E02]
- Another pattern begins with capability verification, not product discovery. [E:E05,E06]
- Both patterns fail because the current environment makes trustworthy filtering too expensive. [E:E03,E06,E07]

### Do Not Optimize First For
- A giant exhibitor directory with the same layout for everyone
- Broad recommendation feeds that ignore search intent
- Generic "discover products" UX without capability proof
```

---

## 11. Final Deliverable

This is what the product-facing conclusion should look like.

```md
# Research Deliverable: Canton Fair Buyer Search Patterns

Generated on: 2026-05-08
Run ID: canton-fair-buyer-search-demo-001
Focus Stage: Search
Decision Job: How overseas buyers search for products or suppliers in China before deciding what to do next
Platforms Covered: Reddit
Evidence Base: 8 sources / 8 evidence units
High-Quality Evidence: 5

## Executive Conclusion

### One-line Answer
The most important distinction is not buyer persona but search intent: one group is trying to find direction, the other is trying to verify capability, and a single generic supplier search flow will fail both.

### What Product Team Should Do
1. Split the first search path by intent: "I need direction" vs "I need verified capability"
2. Prioritize supplier capability verification over listing expansion
3. Design beginner narrowing support separately from advanced supplier validation

### Why This Matters
The current search environment does not mainly fail because there is too little information. It fails because users cannot cheaply turn information into confidence. Beginners cannot narrow the space. Advanced buyers cannot verify who is real. A product that reduces these specific search costs has a clearer path than one that simply aggregates suppliers.

## Confidence Snapshot

### Strongly Supported
- There are at least two different search patterns in the current material, and they should not share one product path. [E:E01,E02,E05,E06]
- Advanced buyers care about verifying development capability, not simply finding inventory suppliers. [E:E05,E06,E07]

### Likely But Not Fully Proven
- Beginner users likely need guided narrowing more than more listings. [I:I01]
- Trade-fair search may be structurally weak for capability-first buyers in some categories. [I:I02]

### Still Unknown
- How common each pattern is outside Reddit
- What experienced buyers do after failing to verify capability at the fair
- Whether a third major pattern is missing from the current sample

## Decision Pattern Summary

| Pattern | Behavioral definition | Search start | Trust anchor | Main blocker | Opportunity | Confidence |
|--------|------------------------|-------------|-------------|-------------|------------|------------|
| Direction-Seeking Explorer | searching for what to do before searching for who to buy from | "where do I even start?" | experienced peer guidance | cannot narrow options or negotiate a realistic first move | guided narrowing and beginner-fit supplier search | High |
| Capability-Seeking Customizer | searching for verifiable development capability, not just supply | "who can build the next version?" | proof of real R&D ability | repeated middleman encounters and capability ambiguity | supplier capability verification layer | High |

## Recommended Product Direction

### Primary Recommendation
Build a dual-path search and verification system that splits users by intent before showing supplier options.

### We Recommend This Because
- One pattern begins with open-ended direction finding, not supplier selection. [E:E01,E02]
- Another pattern begins with capability verification, not product discovery. [E:E05,E06]
- Both patterns fail because the current environment makes trustworthy filtering too expensive. [E:E03,E06,E07]

### Do Not Optimize First For
- A giant exhibitor directory with the same layout for everyone
- Broad recommendation feeds that ignore search intent

## Current Replacement Flows

### Pattern A: Direction-Seeking Explorer
1. Search for where to start
2. Ask experienced people to narrow the space
3. Evaluate beginner-friendly categories or fair sections
4. Worry about MOQ and communication
5. Move forward with low confidence or keep wandering

### Pattern B: Capability-Seeking Customizer
1. Search for development-capable supplier
2. Talk to many suppliers
3. Try to separate real R&D partners from middlemen
4. Fail to verify capability efficiently
5. Leave frustrated or continue outside the fair

## What We Should Build First

### Priority 1
Supplier capability verification layer

### Priority 2
Intent split at search entry

### Priority 3
Beginner narrowing support

## Next Evidence To Collect
1. Cross-platform confirmation of beginner path
2. End-state behavior of experienced buyers
3. Counterexamples that challenge the current two-pattern model

## Appendix: Evidence Index
- E01: No prior ecommerce experience, direction-seeking starting point
- E02: Budget not primary blocker, direction is
- E03: Need external guidance on what fair sections/categories make sense
- E04: Concern about MOQ and factory willingness
- E05: Existing large seller with 50,000-unit starting order
- E06: Wants a new version, not stock goods
- E07: Encounters middlemen instead of capable source factories
- E08: Needs engineering/CAD-style collaboration support
```

---

## 12. Docx Handoff Example

If the user asks for a formal document handoff, the skill should also produce a `.docx` version of the deliverable.

Suggested artifact:

- `canton-fair-buyer-search-deliverable.docx`

Suggested `.docx` structure:

```md
Title: Canton Fair Buyer Search Patterns
Subtitle: Search behavior, replacement flows, and product implications

1. Executive Conclusion
2. Decision Pattern Summary
3. Recommended Product Direction
4. Key Findings
5. Current Replacement Flows
6. What We Should Build First
7. Next Evidence To Collect
8. Traceability Guide
9. Evidence Index
```

Example `Traceability Guide` section inside the docx:

```md
## Traceability Guide

This document uses three evidence labels:

- `E:` Direct evidence from collected source materials
- `I:` Inference derived from one or more evidence items
- `U:` Important unknown that still needs more data

To trace any conclusion:

1. Find the inline evidence IDs next to the conclusion, for example `[E:E05,E06]`
2. Go to the Evidence Index at the end of this document
3. Find the matching `Evidence ID`
4. Use the linked `Source ID` to locate the matching entry in `research-pack.json`
5. Review the original excerpt and source note

Important caution:

- Some evidence items come from paraphrased copied comments rather than verbatim source text
- Those items should be treated as weaker support than direct quoted material
- If a conclusion depends heavily on paraphrased material, it should remain a hypothesis or candidate pattern
```

Example `Evidence Index` table in the docx:

```md
| Evidence ID | Source ID | Platform | Type | Short note | Quality | Lookup note |
|------------|-----------|----------|------|------------|---------|-------------|
| E01 | SRC-01 | Reddit | Post | No prior ecommerce experience | High | Search `SRC-01` in `research-pack.json` |
| E05 | SRC-05 | Reddit | Post | Existing high-volume seller | High | Search `SRC-05` in `research-pack.json` |
| E06 | SRC-05 | Reddit | Post | Encounters middlemen instead of R&D suppliers | High | Search `SRC-05` in `research-pack.json` |
```

Example inline writing style in the docx:

```md
We recommend splitting the first search path by intent because the current sample shows one group searching for direction and another searching for verified capability. [E:E01,E02,E05,E06]

We infer that a generic supplier listing would underserve both groups, but this remains a product interpretation rather than direct user language. [I:I01]

We still do not know how often experienced buyers abandon the fair entirely after failing to verify capability. [U:U02]
```

Recommended source lookup note to include near the end of the docx:

```md
Underlying data files for this document:

- `research-pack.json`
- `evidence-audit.json`
- `decision-patterns.json`
- `flow-maps.json`

To audit any conclusion, follow:

Conclusion -> Evidence ID -> Source ID -> Research Pack entry -> Raw excerpt
```

This is important because the `.docx` should not be a dead-end report.

It should be a portable decision memo with an audit trail.

---

## 13. What This Example Demonstrates

This example is doing the right thing if you notice:

- it does not pretend 2 threads are a full market map
- it extracts patterns instead of writing polished fake personas
- it gives product direction early
- it keeps evidence traceable
- it ends with what to collect next

This example is intentionally not a giant notebook.

It is a decision memo backed by research.
