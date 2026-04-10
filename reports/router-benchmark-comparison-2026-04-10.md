# Router Benchmark Comparison 2026-04-10

## Setup

- benchmark cases: 10
- baseline model: gemini-3-flash-preview
- comparison model: gpt-5.4 via openai-codex

## Summary

| Metric | Gemini 3 Flash Preview | GPT-5.4 |
|---|---:|---:|
| Successful calls | 9 | 10 |
| Must-match cases | 9 | 10 |
| Allowed-match cases | 5 | 9 |
| Errors | 1 | 0 |
| Must-match rate | 0.9 | 1.0 |
| Allowed-match rate | 0.5 | 0.9 |

## Main takeaways

- GPT-5.4 was better on every measured metric.
- The biggest gap was over-creation control: allowed-match rate improved from 0.5 to 0.9.
- GPT-5.4 had no parsing/runtime failures in this run.
- Gemini hit the main bucket often, but created extra targets too frequently and had one failed case.

## Notable model behavior

### Gemini 3 Flash Preview
- Good at finding the main target family.
- More likely to over-create outside the allowed set.
- Produced one failed benchmark case.

### GPT-5.4
- More disciplined target selection.
- Stronger adherence to the benchmark contract.
- One mildly imperfect case remained: `comparison-gpt-vs-gemini`, where it added `mind/queries` in addition to the expected comparison target.
- Also revealed a validator gap because one output duplicated a `world/concepts` target family. That gap has now been closed by rejecting duplicate create targets in the validator.

## Recommendation

Use GPT-5.4 as the default router for this wiki starter.

Keep Gemini as a secondary option for cheaper triage or exploratory experiments, but not as the main routing default unless benchmark results improve.
