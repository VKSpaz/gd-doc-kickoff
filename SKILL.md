---
name: gd-doc-kickoff
description: Conduct a guided RU interview that helps a game designer move from a blank page to a production-ready feature specification in Markdown for any game project. Use when the user asks to draft/structure/finish a game feature doc, needs a kickoff questionnaire, or wants mandatory plus context-driven GDD blocks generated from answers.
---

# Gd Doc Kickoff

## Overview

Run a soft, structured kickoff for feature documentation.
Ask focused questions, infer missing structure from project context, and produce a complete `.md` spec draft.
Work project-agnostically: adapt to the available repository structure instead of assuming one specific project layout.
If the user has no ready concept yet, first run a short ideation interview and only then move to feature-spec documentation.
Keep instructions model-agnostic so the workflow can be used in Codex, Kilokode, Claude, and DeepSeek environments.
Before writing feature-level specs, formalize concept and decompose it into a feature list through guided questions.

## Hard Rules

- Use Russian for all questions, summaries, and final output unless the user explicitly requests another language.
- Do not write or modify gameplay/source code. Work only with documentation.
- Do not prescribe programming implementation details: class names, method names, package/module layout, or code style.
- Keep behavior vendor-neutral: do not rely on assistant-specific features or proprietary tool syntax.
- Keep momentum: ask compact batches of questions, synthesize answers, then continue.
- Start every new kickoff with a readiness check:
  - does the user already have a concept/idea description?
  - if not, run ideation flow from `references/idea-discovery-ru.md` before GDD blocks.
- Use project context before asking broad questions via this cascade:
  - user-provided path
  - `ai-docs/feature-index.md`
  - `docs/feature-index.md`
  - `docs/architecture.md`
  - `README.md`
- If no context files are available, continue through baseline context questions instead of blocking.
- Determine distribution mode early (`f2p`, `premium`, `hybrid/other`) and filter contextual questions by mode.
- In `premium` mode, skip F2P-specific topics by default (IAP offers, hard currency, pay shortcuts, whale segmentation, liveops compensation flows) unless the user explicitly says these systems exist.
- Before first feature-spec draft, ensure project concept and feature index are documented in `docs/`:
  - `docs/project-concept.md`
  - `docs/feature-index.md`
- Prefer explicit assumptions over silent gaps. Mark unresolved items as `TBD`.
- Always include, when relevant to the feature, explicit formulas, balance variable names, and localization key naming proposals.
- Use `snake_case` (`[a-z0-9_]+`) for all balance variable names and localization keys by default.
- For every key design decision, capture rationale explicitly:
  - why this problem matters now
  - why this solution was chosen
  - why alternatives were rejected
  - why expected impact is believable

## Workflow

### Step 0. Onboarding And Idea Readiness

1. Ask whether the user already has a project concept/idea description.
2. If concept is missing or too vague, run ideation interview from:
   - `references/idea-discovery-ru.md`
3. After ideation, return:
   - 1-paragraph summary of user taste/constraints
   - 3 concept directions (safe / original / unusual)
   - for each: core pitch, what player does, where the fun is, key differentiation, key risks
4. Ask user to pick one direction (or merge two), then lock a short baseline concept statement.
5. Continue to documentation workflow only after baseline concept is fixed.

### Step 0.5. Concept Decomposition Into Features

1. Run decomposition interview from:
   - `references/feature-decomposition-ru.md`
2. Through questions, formalize:
   - project promise and audience
   - core loop and session model
   - content/progression structure
   - production constraints and scope boundaries
3. Produce and save:
   - `docs/project-concept.md` (one-page concept snapshot)
   - `docs/feature-index.md` (feature list with priorities and dependencies)
4. In `feature-index`, mark each feature with:
   - priority: `MVP` / `P1` / `P2`
   - owner/status if known
   - dependency notes
   - distribution relevance (`f2p` / `premium` / `all`)
5. Ask user to select the next target feature from the index (or add a new one explicitly), then continue to feature-spec interview.

### Step 1. Kickoff And Context Discovery

1. Confirm the target feature name and the expected audience of the document.
2. Resolve context in this order:
   - explicit path from the user
   - `docs/project-concept.md`
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

### Step 1.5. Distribution Gate (Mandatory Early)

1. Determine game distribution mode as early as possible:
   - `f2p` / live-service
   - `premium` (single purchase)
   - `hybrid/other`
2. Apply interview mode:
   - `f2p` / `hybrid`: use full mandatory + contextual blocks.
   - `premium`: skip F2P-specific contextual questions unless explicitly relevant.
3. If distribution mode is unknown, ask one direct clarification before any monetization-specific batch.

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

When a trigger is active and allowed by distribution mode, ask that block's questions and append the section.
When a trigger is inactive, omit the section entirely.
For `premium` mode, keep focus on core loop clarity, content value, progression depth, and player promise; do not ask F2P monetization questions by default.

### Step 4. Draft The Markdown Spec

Generate one cohesive Markdown document:
- Keep headings and order from `references/gdd-core-template.md`.
- Add only triggered contextual sections.
- For unknowns, write `TBD` plus a short note.
- Keep statements testable and implementation-ready (no vague wording).
- Tie the feature scope back to `docs/feature-index.md` item and keep naming consistent with selected feature card.
- Include a dedicated section with:
  - feature formulas and parameter definitions (if feature logic is quantitative)
  - balance variable names for tuning tables/configs
  - localization key proposals with namespace pattern and examples
- Keep this section product/design-level, not source-code-level.
- Enforce `snake_case` naming in that section for every variable/key unless the user explicitly approves an exception.

### Step 5. Save Into Project Docs Folder

Ask where to save the final `.md`.

Default and preferred location:
- `<project_root>/docs/`

If `docs/` does not exist:
- create `<project_root>/docs/` and save there.

File naming:
- `YYYY-MM-DD_<feature-slug>_gd-spec.md`

Rules:
- Save into project `docs/` by default (on the user's disk in the current project folder).
- If the user explicitly requests a different path, follow it.
- If write path is unavailable, return the full markdown in chat and ask for a new path.
- Keep these docs in `docs/` updated over time:
  - `project-concept.md`
  - `feature-index.md`
  - feature specs (`YYYY-MM-DD_<feature-slug>_gd-spec.md`)

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
- Idea discovery: `references/idea-discovery-ru.md`
- Feature decomposition: `references/feature-decomposition-ru.md`
