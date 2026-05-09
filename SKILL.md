---
name: path-probe
description: "Path Probe / 用户路径探针 turns messy social and community research materials into evidence-backed search behavior patterns, replacement flows, product recommendations, and traceable deliverables for product teams. Use when the user wants to analyze Reddit, Xiaohongshu, YouTube, WhatsApp, comments, screenshots, copied notes, or mixed raw research to find opportunity gaps, understand current search behavior, or create a product-ready research handoff."
---

# Path Probe / 用户路径探针

## Purpose

This skill turns messy user research materials into product-ready search pattern analysis.

Package references:

- `README.md`
- `example-run.md`
- `references/final-deliverable-template.md`
- `references/docx-template.md`
- `references/traceability-guide-template.md`
- `references/anti-patterns.md`

It is for cases where the researcher already collected raw materials such as:

- copied posts
- comment threads
- screenshots or OCR text
- raw links
- scattered notes
- mixed Chinese and English excerpts

This skill does **not** assume the input is clean.

This skill does **not** primarily produce personas.

This skill exists to help a product team answer:

- how different users start searching
- what sources they trust or distrust
- where they get stuck in the current replacement flow
- what product path should be designed first
- what is evidence, what is inference, and what is still unknown

Before improvising structure or wording, prefer reusing the templates in `references/`.

---

## Output Contract

The final output must be decision-oriented, traceable, and usable in a product discussion.

The skill should always produce:

1. `Executive Conclusion`
2. `Confidence Snapshot`
3. `Decision Pattern Summary`
4. `Recommended Product Direction`
5. `Current Replacement Flows`
6. `What We Should Build First`
7. `Next Evidence To Collect`
8. `Docx Handoff`

The skill may additionally produce:

- `Pattern Cards`
- `Evidence Index`
- `Research Pack`
- `Evidence Audit`

The final deliverable must never read like a generic research dump.

Every meaningful conclusion must be linked to evidence.

---

## Hard Rules

### Rule 1: Dirty Input Is Normal

Do not require the user to pre-format materials into JSON or another schema.

The skill must accept messy materials first, then structure them.

### Rule 2: Separate Evidence, Inference, Unknown

Every conclusion must be labeled as one of:

- `E:` direct evidence
- `I:` inference derived from evidence
- `U:` unknown or missing information

Do not blur these layers.

### Rule 3: No Premature Persona Compression

Do not rush into writing polished character profiles.

Prefer:

- decision patterns
- search path shapes
- trust mechanisms
- blocking points
- current workarounds

Avoid turning 2-3 posts into a fake “user archetype” with invented coherence.

### Rule 4: Product Advice Must Be Traceable

No product recommendation should appear without linked evidence.

If a recommendation has only one weak source behind it, mark it as a hypothesis.

### Rule 5: Gaps Are Part of the Deliverable

Do not hide missing information.

Always say:

- what is missing
- why it matters
- where to collect it next

### Rule 6: Search Behavior Over Demographics

Cluster by behavior, not by age/gender/country unless directly relevant.

Prefer grouping by:

- search starting point
- source trust pattern
- comparison behavior
- validation behavior
- decision friction

### Rule 7: Localized Output Language

The final report must be written in the language requested by the user.

If the user writes the research brief or request in a specific language, treat that as the desired output language unless they explicitly ask otherwise.

Localization means more than translating section titles:

- rewrite conclusions, pattern names, evidence summaries, flow descriptions, recommendations, and traceability instructions in natural product-team language for that locale
- keep platform names, app names, source IDs, evidence IDs, and file names unchanged when that improves traceability
- avoid leaving template English such as `Executive Conclusion`, `Finding`, `What we observed`, or `Traceability Guide` in a non-English deliverable unless the user asked for English
- keep `E:`, `I:`, and `U:` markers unchanged, but explain them in the report language

---

## When To Use This Skill

Use this skill when the user wants to:

- find new business opportunities from social/community signals
- understand how users currently search for answers
- map replacement flows before building a product
- compare different search and trust behaviors across platforms
- turn messy community research into product-ready conclusions

Good fits:

- Reddit / YouTube / Xiaohongshu / WhatsApp research bundles
- copied community posts and comment threads
- messy notes from manual user signal collection
- early-stage product discovery

Bad fits:

- pure sentiment monitoring
- brand mention reporting
- summary-only tasks with no product decision use
- cases where the user only wants a literal translation

---

## Inputs

The skill should accept any combination of:

- raw text dump
- markdown notes
- link list
- OCR text
- copied comments
- screenshots transcribed into text
- mixed research notes from the user

Optional extra context:

- `study_topic`
- `decision_job`
- `focus_stage`
- `target consumer`
- `platform scope`
- `current product hypothesis`

If the user does not provide this context, infer only the minimum needed and explicitly state assumptions.

If the user gives a thin or messy sample, review `references/anti-patterns.md` before synthesizing conclusions.

---

## Working Model

This skill works in 4 layers:

1. `Raw Intake`
2. `Evidence Structuring`
3. `Pattern Modeling`
4. `Product Translation`
5. `Docx Handoff`

The skill should move in this order.

Do not jump directly from raw notes to product advice.

---

## Phase 1: Normalize The Research Task

Before analyzing the materials, define the research job in plain language.

Output:

- what decision is being studied
- what part of the journey matters most
- what the likely product team needs from this analysis

Format:

```md
## Research Frame
- Topic: ...
- Decision Job: ...
- Focus Stage: ...
- Product Team Goal: ...
- Assumptions: ...
```

If the user’s goal is vague, choose the narrowest interpretation that still allows useful analysis.

---

## Phase 2: Raw Intake

Take messy material and split it into intake items.

Each intake item should preserve:

- source hint
- platform guess
- content type guess
- whether it contains user voice
- whether it contains researcher voice
- warnings if the chunk is mixed or incomplete

Suggested internal schema:

```json
{
  "intake_id": "",
  "platform_guess": "",
  "content_type_guess": "",
  "raw_chunk": "",
  "contains_user_voice": true,
  "contains_researcher_voice": false,
  "parse_confidence": "high|medium|low",
  "warnings": []
}
```

Output section:

```md
## Intake Summary
- Total intake items: ...
- Mixed items needing caution: ...
- Unknown platform items: ...
- Notes: ...
```

Do not analyze meaning yet. Just cleanly separate the material.

---

## Phase 3: Structure Into A Research Pack

Turn the intake into a normalized research pack.

Each source should aim to include:

- `source_id`
- `platform`
- `url` if known
- `capture_type`
- `raw_content`
- `quote_blocks`
- `researcher_note`
- `author_context`
- `timestamp`
- `language`
- `completeness`
- `missing_fields`

Suggested internal schema:

```json
{
  "source_id": "",
  "platform": "",
  "url": "",
  "capture_type": "",
  "raw_content": "",
  "quote_blocks": [],
  "researcher_note": "",
  "author_context": {
    "self_described_role": "",
    "experience_level_guess": "",
    "use_case_hint": ""
  },
  "timestamp": "",
  "language": "",
  "completeness": "high|medium|low",
  "missing_fields": []
}
```

Output section:

```md
## Research Pack Summary
- Sources normalized: ...
- High completeness sources: ...
- Medium completeness sources: ...
- Low completeness sources: ...
- Most common missing fields: ...
```

Important:

- guesses must remain guesses
- unknown fields must stay unknown

---

## Phase 4: Audit Evidence Quality

Each source must be audited before it is allowed to support strong conclusions.

For each source, assess:

- source clarity
- directness
- context completeness
- behavioral signal strength
- search signal presence
- trust signal presence
- outcome signal presence
- risk flags

Suggested internal schema:

```json
{
  "source_id": "",
  "evidence_health": "high|medium|low",
  "directness": "firsthand|secondhand|unclear",
  "context_completeness": "high|medium|low",
  "decision_signal_strength": "strong|medium|weak",
  "behavioral_signal_present": true,
  "search_signal_present": true,
  "trust_signal_present": true,
  "outcome_signal_present": false,
  "risk_flags": [],
  "audit_reasoning": ""
}
```

Output section:

```md
## Evidence Audit
- High-quality sources: ...
- Medium-quality sources: ...
- Low-quality sources: ...
- Strong search-signal sources: ...

### Main Audit Risks
- ...
- ...
```

Do not confuse emotional intensity with evidence strength.

---

## Phase 5: Extract Evidence Units

Break sources into the smallest useful evidence units.

Each unit should capture:

- exact text
- journey stage
- signal tags
- signal payload
- confidence/weight

Suggested internal schema:

```json
{
  "evidence_id": "",
  "source_id": "",
  "verbatim_text": "",
  "normalized_text": "",
  "journey_stage": "",
  "signal_tags": [],
  "signal_payload": {},
  "strength": "high|medium|low",
  "audit_weight": "high|medium|low"
}
```

Required signal families:

- `search_entry`
- `trust_source`
- `distrust_source`
- `comparison_axis`
- `workaround`
- `friction`
- `trigger`
- `decision_state`

Output section:

```md
## Evidence Unit Summary
- Evidence units extracted: ...
- Search-entry signals: ...
- Trust-source signals: ...
- Friction signals: ...
- Workaround signals: ...
```

---

## Phase 6: Detect Evidence Gaps

This phase is mandatory.

Before making patterns, identify what is still missing.

Types of gaps to look for:

- missing time context
- missing role context
- missing outcome
- missing cross-platform confirmation
- missing counterexamples
- missing comment-thread context
- missing decision end-state

For each important gap, specify:

- what is missing
- why it matters
- risk if ignored
- best next source
- what exactly to collect
- priority

Suggested internal schema:

```json
{
  "gap_id": "",
  "gap_type": "",
  "missing_area": "",
  "why_it_matters": "",
  "current_risk_if_ignored": "",
  "best_next_source": "",
  "what_exactly_to_collect": "",
  "priority": "high|medium|low"
}
```

Output section:

```md
## Next Evidence To Collect
1. {gap}
   - Why it matters: ...
   - Go collect: ...
   - Look for: ...
2. {gap}
   - Why it matters: ...
   - Go collect: ...
   - Look for: ...
```

This section is not optional.

---

## Phase 7: Reconstruct Search Paths

Rebuild likely search paths before clustering.

Do this at the level the evidence actually supports:

- `individual`
- `thread`
- `scenario cluster`

Do not fake a single-person complete journey if the data only supports a scenario cluster.

For each path:

- define the search starting point
- sequence the steps
- identify trust tests
- identify friction points
- identify current workaround or fallback
- record outcome if known

Suggested internal schema:

```json
{
  "path_id": "",
  "entity_scope": "individual|thread|scenario_cluster",
  "path_confidence": "high|medium|low",
  "steps": [],
  "path_outcome": "buy|abandon|delay|switch|unknown",
  "unknown_links": []
}
```

Output section:

```md
## Current Replacement Flows
### Flow A
1. ...
2. ...
3. ...

Where the flow breaks:
- ...
- ...

What a better product would replace:
- ...
```

---

## Phase 8: Model Decision Patterns

This is the core phase.

Group by search behavior, not by profile labels.

Pattern dimensions to prioritize:

- search starting point
- search path shape
- source trust pattern
- validation behavior
- comparison logic
- blocker type
- workaround type

Suggested internal schema:

```json
{
  "pattern_id": "",
  "pattern_name": "",
  "pattern_status": "confirmed|candidate|weak",
  "summary": "",
  "search_start": [],
  "search_path_shape": "",
  "trusted_sources": [],
  "distrusted_sources": [],
  "comparison_axes": [],
  "blocking_points": [],
  "current_workarounds": [],
  "evidence_ids": [],
  "counterexamples": [],
  "inferences": [],
  "unknowns": [],
  "overall_confidence": "high|medium|low"
}
```

Pattern naming guidance:

- use behavior names, not marketing names
- good: `Comment-First Validator`
- good: `Direction-Seeking Explorer`
- bad: `Budget-Conscious Sarah`

Output section:

```md
## Decision Pattern Summary
| Pattern | Behavioral definition | Search start | Trust anchor | Main blocker | Opportunity | Confidence |
|--------|------------------------|-------------|-------------|-------------|------------|------------|
| ... | ... | ... | ... | ... | ... | ... |
```

If there are fewer than 3 strong sources for a pattern, mark it as `candidate`.

---

## Phase 9: Translate Into Product Direction

Only after patterns are built should product translation happen.

Translate patterns into:

- product implications
- path design implications
- content implications
- open risks
- next research actions

Suggested internal schema:

```json
{
  "pattern_id": "",
  "product_implications": [],
  "path_design_implications": [],
  "content_implications": [],
  "open_risks": [],
  "next_research_actions": []
}
```

Required thinking:

- what step should the product replace
- what uncertainty should it reduce
- what trust mechanism should it supply
- what user effort should it remove

Output section:

```md
## Recommended Product Direction
### Primary Recommendation
...

### We Recommend This Because
- ... [E:...]
- ... [E:...]

### Do Not Optimize First For
- ...
- ...
```

Do not make vague feature recommendations.

Bad:

- “build community”
- “improve onboarding”

Better:

- “show supplier capability evidence before full profile exploration”
- “split the first search path by intent before showing listings”

---

## Phase 10: Produce A Docx Handoff

This phase is required when the user wants a shareable deliverable for a team, stakeholder review, or archival use.

The skill should produce a `.docx` document that contains:

- the final executive conclusion
- the product recommendations
- the confidence snapshot
- the current replacement flows
- the next evidence to collect
- a traceability section that explains how to look up underlying data

The `.docx` should not contain the entire raw dataset pasted inline unless the user explicitly asks for that.

The default behavior is:

1. keep the main body decision-focused
2. include short evidence references inline
3. add a dedicated appendix for traceability

Required document sections:

```md
1. Title Page
2. Executive Conclusion
3. Decision Pattern Summary
4. Recommended Product Direction
5. Key Findings
6. Current Replacement Flows
7. What We Should Build First
8. Next Evidence To Collect
9. Traceability Guide
10. Evidence Index
```

### Traceability Guide Requirements

The docx must explain, in plain language, how a reader can trace a conclusion back to source data.

Prefer the language in `references/traceability-guide-template.md`.

It should include:

- what `E:`, `I:`, and `U:` mean
- how to use `Evidence ID`
- how to locate the matching record in the research pack
- how to verify whether a conclusion is direct evidence or inferred
- where incomplete or paraphrased material should be treated carefully

Suggested traceability wording:

```md
## Traceability Guide

- `E:` means direct evidence from the collected materials.
- `I:` means an inference drawn from one or more evidence items.
- `U:` means an important unknown that is not yet supported by sufficient data.
- To trace a conclusion, start from the evidence IDs shown inline, then look them up in the Evidence Index.
- Each Evidence Index row should point to the matching source record in the research pack.
- If a source was paraphrased rather than copied verbatim, that should be stated in the source note.
```

### Source Lookup Requirements

Each evidence item referenced in the final `.docx` should be traceable through this chain:

`Conclusion -> Evidence ID -> Source ID -> Research Pack entry -> Raw material or excerpt`

If the dataset is stored in separate files, the docx should name them explicitly, for example:

- `research-pack.json`
- `evidence-audit.json`
- `decision-patterns.json`

### Default Docx Artifact Names

Use one or both of:

- `research-deliverable.docx`
- `{study-topic-slug}-research-deliverable.docx`

### Formatting Guidance

- Keep the first 2-3 pages executive-friendly
- Put traceability and evidence lookup guidance near the back
- Use short tables over long prose when possible
- Keep each finding tied to visible evidence references
- When creating a `.docx`, convert Markdown tables into native Word tables; do not paste pipe-delimited Markdown table text as ordinary paragraphs
- If generating the `.docx` programmatically, parse contiguous Markdown table rows, skip the separator row, create a real table, and make the header row visually distinct
- Verify the `.docx` contains the expected number of native tables when tables are part of the handoff

---

## Final Deliverable Template

The final answer should be presented in this structure:

```md
# Research Deliverable: {study_topic}

Generated on: {date}
Run ID: {run_id}
Focus Stage: {focus_stage}
Decision Job: {decision_job}
Platforms Covered: {platforms}
Evidence Base: {source_count} sources / {evidence_unit_count} evidence units
High-Quality Evidence: {high_quality_evidence_count}

## Executive Conclusion
### One-line Answer
...

### What Product Team Should Do
1. ...
2. ...
3. ...

### Why This Matters
...

## Confidence Snapshot
### Strongly Supported
- ... [E:...]

### Likely But Not Fully Proven
- ... [I:...]

### Still Unknown
- ... [U:...]

## Decision Pattern Summary
...

## Recommended Product Direction
...

## Key Findings
...

## Current Replacement Flows
...

## What We Should Build First
...

## Next Evidence To Collect
...

## Appendix: Evidence Index
...
```

This final deliverable should feel like:

- a decision memo
- backed by research
- traceable to evidence

It should not feel like:

- a giant notebook dump
- an essay
- a persona workshop

If the user asked for a formal handoff document, also produce the same deliverable as a `.docx` with a traceability appendix.

Prefer the markdown skeleton in `references/final-deliverable-template.md` and the docx structure in `references/docx-template.md`.

---

## Quality Bar

The skill is doing well if the product team can read the output and immediately say:

- “there are clearly different search paths here”
- “we know what users trust”
- “we know where the current flow breaks”
- “we know what to build first”
- “we know what we still need to validate”

The skill is failing if the output mainly says:

- “users care about trust”
- “users have pain points”
- “we should improve the experience”

Those are content-free.

---

## Anti-Hallucination Checklist

Before finalizing, verify:

- every key conclusion has linked evidence
- every inference is labeled as inference
- every major unknown is explicit
- every pattern has enough supporting evidence
- every product recommendation maps to a real blocker or trust gap
- no invented demographic detail appears
- no polished fake persona appears unless directly supported

If the evidence is thin, say so plainly.

Preferred wording:

- “candidate pattern”
- “likely but not fully proven”
- “evidence currently limited to Reddit threads”

Not preferred:

- “users generally”
- “most buyers”
- “people usually”

unless directly supported.

Review `references/anti-patterns.md` if the sample is small, paraphrased, or cross-platform inconsistent.

---

## Completion Standard

A complete run should leave behind these artifacts when useful:

- `research-pack`
- `evidence-audit`
- `decision-patterns`
- `flow-maps`
- `product-brief`
- final product-facing deliverable
- `research-deliverable.docx`

If time or context is limited, prioritize:

1. final product-facing deliverable
2. evidence audit
3. next evidence to collect

That is the minimum useful output.
