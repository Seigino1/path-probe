# Traceability Guide

This document uses three evidence labels:

- `E:` Direct evidence from collected source materials
- `I:` Inference derived from one or more evidence items
- `U:` Important unknown that still needs more data

## How To Trace A Conclusion

1. Find the inline evidence IDs next to the conclusion, for example `[E:E05,E06]`.
2. Go to the `Evidence Index`.
3. Find the matching `Evidence ID`.
4. Use the linked `Source ID` to locate the matching entry in `research-pack.json`.
5. Review the original excerpt and source note.

## Important Cautions

- Some evidence items may come from paraphrased copied comments rather than verbatim text.
- Those items should be treated as weaker support than direct quoted material.
- If a conclusion depends heavily on paraphrased material, keep it as a hypothesis or candidate pattern.

## Lookup Chain

Use this lookup chain when auditing any claim:

`Conclusion -> Evidence ID -> Source ID -> Research Pack entry -> Raw excerpt`

## Underlying Data Files

- `research-pack.json`
- `evidence-audit.json`
- `decision-patterns.json`
- `flow-maps.json`
