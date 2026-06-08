---
name: pm-make
description: Use when a product manager has a rough, vague, or not-yet-final product idea and wants to converge it into a Product Shipping Brief that can be used for AI coding, prototype generation, PRD preparation, requirement clarification, user scenario analysis, JTBD analysis, MVP scope definition, page flow planning, or product solution finalization.
---

# PM Make

## Goal

PM Make turns rough product ideas into a frozen Product Shipping Brief: a structured product definition document, usually kept to roughly one page, that can be used directly for AI coding, prototype generation, or as the input to the PRD Make workflow.

This skill is not a one-shot prompt. It must use a workspace-style harness: initialize 6 focused files, fill them one by one, expose open questions, and freeze the brief only after the user confirms that no unresolved product decisions remain.

## Workflow

1. Create or reuse a product workspace directory.
2. If the user has not provided documents, initialize 6 Markdown templates from `assets/templates/`.
3. Write the user's original idea and iteration history into `01-basic-description.md`.
4. Fill the templates in order. Focus on only the current file in each round.
5. When information is missing, write precise questions into `05-open-questions.md`; do not invent decisions.
6. Ask blocking questions first. In the chat reply, list every blocking question with its ID, brief reason, and recommended option. Do not only ask the user to open `05-open-questions.md`.
7. After the user answers blocking questions, update the affected files and mark the corresponding questions as `Resolved` or `Deferred by user`.
8. Then ask non-blocking questions in a separate conversation round. Again, list every question in the chat reply and ask the user to confirm, reject, or explicitly defer the recommended option.
9. Freeze only when `05-open-questions.md` contains no `Open` questions and the user has confirmed the product direction.
10. Write the final Product Shipping Brief into `06-final-brief.md`.

## Six Files

Unless equivalent files already exist in the workspace, use these filenames:

- `01-basic-description.md`: original requirements, one-sentence positioning, target users, constraints, iteration history.
- `02-jtbd-scenarios.md`: JTBD, Job Stories, ES-style trigger-condition scenarios, scenario priorities.
- `03-scope-boundaries.md`: product form, MVP scope, deferred scope, assumptions, technical preferences.
- `04-page-flows.md`: page list, main flows, state transitions, core data objects, permissions.
- `05-open-questions.md`: unresolved questions, deferred decisions, confirmed decisions, risk notes, answer history.
- `06-final-brief.md`: frozen Product Shipping Brief and change log.

## Question Rules

Ask only questions that would change product behavior, engineering scope, data models, permissions, flows, or delivery priority. Prefer grouped, concrete questions rather than broad brainstorming.

Conversation rules:

- Do not require the user to open workspace files in order to answer questions. `05-open-questions.md` is the tracking record, but the chat reply must include the complete current-round question list.
- Ask in two rounds: blocking questions first, then non-blocking questions. Unless the user explicitly asks for the complete list, do not mix both types in the same round.
- Every question must be answerable by ID. It must include: question ID, decision to confirm, one-sentence reason why it matters, and a recommended option or options.
- When there are many questions, focus the current round on one category instead of dumping every open item at once.
- After the user answers one round of questions, first update `05-open-questions.md`, confirmed decisions, deferred decisions, and affected product files before moving to the next round of questions.

Status rules:

- `Open` is used only for questions while the product is still being clarified.
- Before freezing, every question must become `Resolved` or `Deferred`.
- `Deferred by user` means the user explicitly accepts the MVP recommended default or explicitly postpones the topic to the PRD or technical design stage.
- As long as any line in `05-open-questions.md` remains `Open`, the Product Shipping Brief cannot be marked frozen, including non-blocking questions.
- Do not use the skill itself, the model, or "PM Make default assumptions" as the source of a confirmed or deferred decision. The source must be the user, a user-provided document, or other explicit external material.
- You may provide suggestions in `Options/Recommendation`, but a suggestion is not a decision. It counts only after the user confirms it.

## Required Dimensions

- Product one-liner: for whom, in what scenario, solving what high-value problem.
- Target users: primary users, decision makers, influencers, affected users.
- JTBD: situation, motivation, desired progress, functional/emotional/social jobs.
- Current alternatives: how users solve it today, what the pain points and switching barriers are.
- MVP boundaries: must-have, deferred, out of scope.
- Core flow: entry point, main path, key branches, failure/empty/waiting states.
- Pages and information architecture: page list, page responsibilities, navigation relationships.
- Business rules: states, permissions, data sources, key constraints.

Read [shaping-playbook.md](references/shaping-playbook.md) as needed for detailed questioning methods.

## Shaping Pass Criteria

Only recommend moving into PRD or prototype when all of the following are true:

- Product positioning can be accurately expressed in 1-2 sentences.
- At least 1 core user, 1 core scenario, and 1 main flow are clear.
- MVP scope has explicit "do / do not do / to be confirmed" boundaries.
- Key business rules, permissions, and data objects have not been left vague.
- Unconfirmed content has been collected in `shaping/05-open-questions.md`.

Read [shaping-playbook.md](references/shaping-playbook.md) for detailed shaping pass criteria.

## Final Brief Format

The final brief should be compact and executable. Use this structure:

```markdown
# Product Shipping Brief

## 1. One-Sentence Positioning

## 2. Target Users

## 3. Core Scenarios

## 4. Product Form

## 5. MVP Scope

## 6. Pages and Main Flows

## 7. Data and Permissions

## 8. Deferred Scope

## 9. Confirmed Decisions
```

Do not expand the brief into a long PRD. If the user requests a full PRD, switch to PRD Make and use the frozen brief as the single source of truth.
