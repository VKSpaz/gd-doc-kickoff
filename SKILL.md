---
name: gd-doc-kickoff
description: Conduct a guided RU interview that helps a game designer move from a blank page to a production-ready feature specification in Markdown for any game project. Use when the user asks to draft/structure/finish a game feature doc, needs a kickoff questionnaire, or wants mandatory plus context-driven GDD blocks generated from answers.
---

# Gd Doc Kickoff

## Overview

Run a soft, structured kickoff for feature documentation.
Ask focused questions, infer missing structure from project context, and produce a complete `.md` spec draft.
Work project-agnostically: adapt to the available repository structure instead of assuming one specific project layout.

## Hard Rules

- Use Russian for all questions, summaries, and final output unless the user explicitly requests another language.
- Do not write or modify gameplay/source code. Work only with documentation.
- Do not prescribe programming implementation details: class names, method names, package/module layout, or code style.
- Keep momentum: ask compact batches of questions, synthesize answers, then continue.
- Use project context before asking broad questions via this cascade:
  - user-provided path
  - `ai-docs/feature-index.md`
  - `docs/feature-index.md`
  - `docs/architecture.md`
  - `README.md`
- If no context files are available, continue through baseline context questions instead of blocking.
- Prefer explicit assumptions over silent gaps. Mark unresolved items as `TBD`.
- Always include, when relevant to the feature, explicit formulas, balance variable names, and localization key naming proposals.
- Use `snake_case` (`[a-z0-9_]+`) for all balance variable names and localization keys by default.
- For every key design decision, capture rationale explicitly:
  - why this problem matters now
  - why this solution was chosen
  - why alternatives were rejected
  - why expected impact is believable

## Workflow

### Step 1. Kickoff And Context Discovery

1. Confirm the target feature name and the expected audience of the document.
2. Resolve context in this order:
   - explicit path from the user
   - `ai-docs/feature-index.md`
   - `docs/feature-index.md`
   - `docs/architecture.md`
   - `README.md`
3. Read the first available relevant file(s) to anchor terminology and system placement. Do not assume all files exist.
4. Build a short "context snapshot":
   - affected systems
   - likely dependencies
   - likely risks
5. If no file from the cascade exists, continue with baseline context questions from mandatory blocks (passport, business context, positioning, contextual systems).
6. Before each question batch, derive 1-3 context questions from mapped systems or already collected user answers instead of asking generic questions.

### Step 2. Mandatory Interview (Core Blocks)

Ask and fill all sections listed in:
- `references/gdd-core-template.md`
- `references/question-bank-ru.md` (mandatory section)

Use short batches (3-7 questions), then summarize what is already fixed before moving to next batch.
In every batch, ask at least 2 "why" questions tied to decisions, not just facts.

### Step 2.5. Rationale Gate (Why/Why Not)

Before moving to generation, verify each key block has rationale:
- problem and timing
- goal and metric choice
- main mechanic choice
- scope boundaries
- rollout strategy

If rationale is weak or generic, ask follow-up questions until the decision is defendable.

### Step 3. Conditional Blocks By Triggers

Detect whether extra sections are needed using:
- `references/context-triggers.md`
- `references/question-bank-ru.md` (contextual section)

When a trigger is active, ask that block's questions and append the section.
When a trigger is inactive, omit the section entirely.

### Step 4. Draft The Markdown Spec

Generate one cohesive Markdown document:
- Keep headings and order from `references/gdd-core-template.md`.
- Add only triggered contextual sections.
- For unknowns, write `TBD` plus a short note.
- Keep statements testable and implementation-ready (no vague wording).
- Include a dedicated section with:
  - feature formulas and parameter definitions (if feature logic is quantitative)
  - balance variable names for tuning tables/configs
  - localization key proposals with namespace pattern and examples
- Keep this section product/design-level, not source-code-level.
- Enforce `snake_case` naming in that section for every variable/key unless the user explicitly approves an exception.

### Step 5. Save Outside Repo

Ask where to save the final `.md`.

Default recommendation:
- Windows: `%USERPROFILE%\\Downloads\\gd-docs\\`
- macOS/Linux: `$HOME/Downloads/gd-docs/`
- Fallback: `Desktop/gd-docs/`

File naming:
- `YYYY-MM-DD_<feature-slug>_gd-spec.md`

Rules:
- Prefer saving outside the current git repository.
- If the user asks to save inside the repo, ask one explicit confirmation first.
- If save path is unavailable, return the full markdown in chat and ask for a new path.

### Step 6. Final Hand-off

Return:
1. short summary of what was captured
2. list of unresolved `TBD` points
3. final file path (if saved)
4. optional next action: "continue with deeper sub-docs" (matchmaking/balance/event map/etc.)

## Interview Quality Bar

- Do not ask abstract questions if answer can be inferred from project context.
- Convert fuzzy answers into concrete wording and confirm.
- Separate goals, mechanics, metrics, risks, and rollout criteria.
- Always include acceptance criteria and post-release measurement plan.
- Reject rationale-free statements like "так лучше" or "обычно так делаем" without context; ask for concrete reasons.

## References

- Core template: `references/gdd-core-template.md`
- Trigger map: `references/context-triggers.md`
- Question bank: `references/question-bank-ru.md`
