# Copilot / AI Agent Instructions — S-Engine-Physics-API

Purpose: Help an AI agent be immediately productive editing prompts, generating lessons, and making safe, schema-preserving changes.

## Quick summary ✅
- This repo is a lesson-slide generator for UK GCSE Physics. The authoritative spec is `prompts/prompts_s-engine.md` (the non-negotiable JSON output schema and pedagogical rules).
- Core engineering artefacts (build/test scripts, CI) were NOT discovered — ask maintainers before adding runtime behaviour.

## Where to look 🔎
- `prompts/prompts_s-engine.md` — primary, canonical rules (slide order, JSON schema, LaTeX and inline notation rules, teacher voice).
- `README.md` — project overview and goals.
- `prompts.md`, `Assets/Images/*` — supplementary materials and example assets.

## Hard constraints (do not change) ⚠️
- Output must be valid JSON exactly matching the schema described in `prompts/prompts_s-engine.md` (lesson_metadata + slides array).
- Do NOT add fields to the JSON output unless the prompt explicitly permits it.
- Slide counts: lessons are either 11 slides (no core equation) or 13 slides (with core equation + triangle).
- Core equation slide (when present) MUST use LaTeX with `\frac{...}{...}` for division and define every symbol + unit.
- After Slide 5 (core equation), subsequent uses of the equation must be inline notation (e.g., `ρ = m/V`).
- The "Do Now" slide must contain exactly 5 questions with the specified distribution (retrieval, How Science Works, link back).
- The 5-step modelled example format is mandatory when a worked example is included.
- Language / voice: British English, neutral professional teacher voice; default exam board = AQA GCSE Higher if none specified.

## Examples (follow these closely) 💡
- lesson_metadata snippet:

```json
"lesson_metadata": {
  "title": "Density and Mass",
  "exam_board": "AQA",
  "tier": "Higher",
  "target_grades": "4-6",
  "has_core_equation": true,
  "total_slides": 13
}
```

- Core equation slide (required format):
```json
{
 "slide_number": 5,
 "slide_type": "core_equation",
 "content": {
   "core_equation": {"latex": "\\rho = \\frac{m}{V}"},
   "symbols": [{"symbol":"ρ (rho)","quantity":"density","unit":"kg/m³"}, ...]
 }
}
```

## Style & content rules (from the prompt) ✍️
- Use GCSE command words in outcomes (describe, explain, calculate, compare, state).
- Slides 3 (definitions) must use exam-board-approved wording and 3–5 definitions only.
- No A-level content or advanced terminology; avoid extended essay-style questions.
- Equation triangle only for simple, 3-variable relationships with no constants/squared terms.

## Validation & editing workflow 🔧
- Before committing prompt edits that affect schema, run a JSON validation pass against a representative sample of generated outputs.
- If adding new slide types or fields, open an issue and get agreement from maintainers first — document the change in a short changelog header in the prompt file.

## Unknowns / recommendations ❗
- No `package.json`, `Makefile`, test scripts, or CI config found: do not add automated publish/build changes without confirming the intended runtime and release process.
- If you need to add integration tests, add them under `tests/` and include a simple validation that generated JSON matches the schema.

## If you're uncertain — ask this first 📬
- Confirm the default exam board if changing wording that depends on the specification.
- Confirm whether new slide fields should be visible to teachers or only for internal use.

---

If you'd like, I can: (1) open a small PR adding this file, (2) add a basic JSON schema file for automated validation, or (3) extract the slide types into a compact reference table. Which should I do next?