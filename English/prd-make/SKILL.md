---
name: prd-make
description: Use when the user already has a frozen Product Shipping Brief, prototype summary, or validated product solution and wants to generate a complete PRD for a human development team through modular, numbered, append-style writing; suitable for engineering, UI, QA, acceptance criteria, module planning, dependency mapping, or resumed PRD writing.
---

# PRD Make

## Goal

PRD Make generates a complete PRD for a human development team from a frozen Product Shipping Brief. The PRD is not a document for direct AI coding consumption; it is an atomic, reviewable document for product, UI, engineering, QA, and production implementation.

This skill is not a one-shot generation prompt. It must use a workspace-style harness: first complete planning, then ask the user to confirm; after confirmation, append confirmed content section by section and module by module according to the current PRD template, while continuously maintaining review records, append logs, and pending decisions.

## Required Inputs

Use the frozen Product Shipping Brief as the single source of truth. If the user has not provided a brief, first ask for the file path or switch to PM Make to complete product finalization.

Runnable prototypes, demo notes, page screenshots, meeting notes, and user supplements can only be used as supporting evidence. Before writing them into the PRD, align them with the frozen Product Shipping Brief; if conflicts occur, use the conclusion confirmed by the user.

Before writing, obtain or confirm at least the following:

- The frozen Product Shipping Brief and its source.
- User-supplied scope, constraints, priorities, or business rules.
- Supporting materials that help understand pages, flows, states, permissions, and data objects.
- The user's company PRD template; if not provided, use this skill's default template file.
- The PRD workspace path, or a new workspace path created in the current session.

## Initialization

Create or reuse the following files in the PRD workspace:

- `PRD Document.md`: PRD body.
- `Module Plan.md`: module breakdown, dependencies, development order, and review status.
- `Decision Log.md`: centralized record of all pending decisions during PRD writing and review.
- `Append Log.md`: records every append, revision, confirmation, and review status.

If the user does not provide a company PRD template, use `assets/templates/prd-template.md`.
Use `assets/templates/module-plan-template.md` for module planning.
For detailed feature module writing structure and pre-review checks, use the embedded feature module writing requirements in the feature requirements section of the PRD template.
Use `assets/templates/decision-log-template.md` for pending decisions.
Use `assets/templates/append-log-template.md` for append records.

During initialization, you may create the PRD body file and keep the template skeleton, but before the user confirms the module plan, do not begin writing PRD body content and do not append feature modules under feature requirements.

## Template Adaptation Rules

`SKILL.md` defines only the workflow, data requirements, confirmation checkpoints, writing strategy, and quality checks. It does not define the PRD's concrete section names, table fields, heading levels, or field ordering. All body structure, heading formats, table fields, module detail structure, and record fields must follow the currently selected template file.

Before writing or revising, always read the template in the current workspace or the company template provided by the user before generating content. Do not rely on remembered historical formats from this skill; if the template changes, follow the new template.

When there is a clear gap between the template and the frozen Product Shipping Brief, do not modify the PRD structure on your own. First record the gap and suggested handling, then ask the user to confirm whether to extend the template, adjust the scope, or put the item into the pending decision record.

When writing feature modules, generate content according to the embedded module structure, heading levels, numbering system, and pre-review check requirements in the feature requirements section of the current PRD template. Do not paste placeholders, template instructions, or unadapted draft content into the final PRD.

## Modularization Strategy

Do not assume a fixed number of PRD modules. Module count and writing units must be determined by:

- The currently selected PRD template.
- The frozen Product Shipping Brief.
- Confirmed feature scope.
- The complexity and independence of each feature.
- Differences in pages, permissions, states, data objects, and business rules.
- Dependencies required for engineering delivery.

When feature behavior, permissions, data objects, page entries, state transitions, or acceptance methods differ significantly, split them into smaller modules. Merge multiple capabilities into one module only when they share the same user flow, business rules, data ownership, and engineering delivery boundary.

Module and feature numbers must be consecutive and consistent with the current module plan. If skipped numbers, duplicate numbers, cross-reference errors, or inconsistencies between the plan and body are found, stop writing, fix the plan and affected content first, then continue.

## Module Planning Workflow

Before formally writing the PRD body, create a module plan from the frozen Product Shipping Brief and current template, then ask the user to confirm it.

The module plan should cover at least the following dimensions, while concrete fields, tables, and presentation format must follow the current module plan template:

- Basis for module splitting and module scope.
- Feature coverage by page, menu, or entry.
- Dependencies between modules.
- Recommended development order.
- Parallel development recommendations.
- Current review status.

Module plan confirmation rules:

- Do not only write the module plan into a file and ask the user to open it.
- The chat reply must include the complete planning summary that needs user confirmation in this round, so the user can review without opening the file.
- After outputting it, explicitly ask whether the user confirms it or wants changes to module breakdown, scope boundaries, dependencies, priorities, development order, or parallelizable items.
- Before the user confirms the module plan, do not start writing the PRD body and do not append feature modules under feature requirements.
- After the user requests changes, first update `Module Plan.md`, then output the updated plan in chat for confirmation again.
- If resuming an existing PRD and planning changes affect already written body content, after the user confirms the planning adjustment, synchronize revisions to affected content and record the append log.

## Append-Style Writing Workflow

Only after the user confirms the module plan may formal PRD body writing begin.

During writing, follow this process:

1. Proceed in the body structure order of the current PRD template.
2. Advance only one reviewable unit at a time. A reviewable unit can be one major template section, one feature module, or a small group of revisions explicitly requested by the user.
3. After completing each major section, output a confirmation summary in chat and pause for user confirmation, unless the user explicitly asks to continue automatically.
4. Feature modules under feature requirements must be confirmed module by module. Even if preceding content has been confirmed, do not skip module-level confirmation.
5. After user confirmation, append to `PRD Document.md` or update the corresponding draft status in `PRD Document.md` to confirmed.
6. Unless the user requests revision, do not rewrite previously confirmed content.
7. Update `Append Log.md` after every append, revision, confirmation, or skip.
8. Whenever business rules, scope boundaries, permissions, data definitions, or acceptance methods cannot be confirmed, write them into `Decision Log.md` and mention them in the confirmation summary.
9. At the end of every reply, state which reviewable unit will continue next and directly ask whether to continue.
10. When the user supplements, revises, corrects, or asks to sync the module plan for an already written or pending unit, treat that round as a reviewable unit. After completing file changes, the reply must do two things at the end: ask the user to confirm whether this revision passes, and state which reviewable unit will continue after confirmation, directly asking whether to continue.
11. Do not omit the next-step continuation question just because the round is a small change, patch, plan sync, or answer to a user question; the only exceptions are when the user explicitly asks to stop, pause, or only modify without continuing.

If the user explicitly asks to continue automatically, you may advance multiple units continuously, but still must:

- Follow the current template.
- Record the review status of each unit.
- Pause for confirmation when key decisions, scope changes, or high-risk assumptions appear.
- Summarize written content, pending confirmations, and the next step at the end of each round.

## Pending Decision Workflow

During PRD writing, do not hide unresolved questions in the body and do not disguise model assumptions as confirmed facts.

At any stage of PRD writing, whenever you think of or encounter a feature, rule, scope, permission, data, flow, exception, acceptance criterion, priority, dependency, or other item that needs user supplement, confirmation, or adjudication, record it in `Decision Log.md`. Do not ignore it just because it does not temporarily block the current section, and do not scatter it only in chat history.

All pending decisions must be written into `Decision Log.md`; concrete fields and status wording follow the current pending decision template.

Record a pending decision when any of the following occurs:

- The frozen Product Shipping Brief does not cover an item that affects product behavior, engineering scope, data models, permissions, or acceptance criteria.
- User supplements conflict with the brief, template, or confirmed content.
- While writing, you infer a possible need to add, split, merge, or defer a feature, but the brief or user-confirmed information is insufficient.
- A business rule, state transition, exception handling rule, permission boundary, data definition, notification strategy, integration method, or acceptance method has multiple reasonable options.
- A temporary default assumption is used to keep progress moving and has not yet been confirmed by the user.
- Further confirmation is needed from business, technical, legal, operations, or another owner.
- The current work can only proceed with a default assumption for now.

When recording a pending decision, also describe the source location, the question requiring user supplement or confirmation, impact scope, and current temporary handling or recommended default. Fill fields according to the current pending decision template.

After adding or updating a pending decision, mention it in the current confirmation summary and state whether it blocks the next writing step. Blocking items should be confirmed by the user first. Non-blocking items may proceed with temporary assumptions, but must not be marked confirmed.

After the user confirms a pending decision, update the record. If needed, synchronize revisions to the PRD body, module plan, or append log. If the user explicitly defers a decision, also record deferred status, temporary wording, and possible impacts.

## Diagram Expression Strategy

When the PRD needs to express structure, flow, state, dependencies, or sequence, prefer the diagram form required by the current template. If the template has no explicit requirement, choose Mermaid, tables, or lists according to content complexity. The goal is to make product, design, engineering, and QA review quickly.

Keep diagram nodes and table content concise. Avoid stuffing long business rules into graph nodes. Detailed rules, exceptions, permissions, and acceptance methods should be written in template-specified places.

## Pre-Confirmation Reply Rules

Before every user confirmation, list the files added or modified in the current round in the chat reply. The reply should let the user understand what changed, why it matters, and what needs confirmation without opening the files.

Pre-confirmation replies must:

- Not only say "done, please confirm", "appended", or "next step".
- Not fully copy the body, complete tables, or complete feature details just written into files.
- Not only list filenames, line numbers, and a very short change note. File links may remain, but must be paired with reviewable summaries so the user can judge whether the change matches expectations without opening the files.
- Use a moderate summary granularity covering the main writing unit, key business rules, key data or permission changes, important pending decisions, and next-step impacts.
- For new content, explain what structures and core conclusions were added.
- For modified content, explain which positions were modified, what wording changed from and to, and the reason or impact.
- For each important change, explain at least two of these three types of information: purpose, current wording/stance, and impact on later sections/modules/acceptance.
- When the round involves business rules, permissions, states, data objects, acceptance criteria, module planning, or pending decisions, explicitly summarize the final stance. For example: "The current rule is..." or "Subsequent feature details will follow..."
- When multiple files are updated, list changes by file, but each file's explanation should include why it changed, what wording/stance was synchronized, and whether it affects future writing. Do not turn the file list into a pure audit log.
- If a diagram, dependency relationship, state machine, or development order is generated, summarize key branches, dependencies, or sorting basis.
- Clearly ask the specific confirmation questions, pointing to module breakdown, scope boundaries, business rules, priorities, dependency order, or pending decisions.
- If the PRD is not complete, explicitly ask whether to continue to the next step.
- If this round supplements or revises a previous reviewable unit, explicitly ask two questions: whether this revision is confirmed, and whether to continue to the next reviewable unit after confirmation.

Suggested structure for pre-confirmation replies:

1. What was completed in this round.
2. Key additions or modifications: use 2-6 high-signal summary points covering core wording/stance, scope changes, state/permission/data/acceptance changes, and impacts; file links may be attached, but do not let links replace explanation.
3. Points requiring confirmation.
4. Recommended next step.

## Quality Checks

Before handing the current reviewable unit to the user for confirmation or moving to the next unit, check the following dimensions. If inconsistent, fix them before continuing:

- Flow completeness: before body writing starts, the module plan has been output in chat and confirmed by the user.
- Template consistency: body structure, heading levels, table fields, field order, module details, and record formats match the current selected template.
- Source consistency: all requirements, rules, scope, and assumptions trace back to the frozen Product Shipping Brief, user confirmation, or explicit supporting materials.
- Numbering consistency: module, feature, rule, acceptance criterion, and pending decision numbers are consecutive, not mixed, and consistent with plan and body references.
- Planning consistency: module plan, body overview, feature details, and development order do not conflict.
- Granularity consistency: module splitting is sufficient for engineering, UI, and QA review, without mixing capabilities that differ significantly in behavior, permissions, data objects, or acceptance methods.
- Implementability: pages, fields, states, permissions, exceptions, data requirements, and business rules are specific enough for engineering implementation.
- Testability: acceptance criteria are verifiable and can be turned into test cases by QA.
- Pending decision transparency: unconfirmed items have entered `Decision Log.md` and are not hidden in the body or feature details.
- Diagram validity: structural, flow, state, dependency, and sequence content uses suitable expression according to template or complexity, and diagrams are consistent with the body.
- Append discipline: every append, revision, confirmation, and skip has updated `Append Log.md`.
- User confirmation loop: the current reply explains additions or modifications, final stance, key impacts, pending confirmation questions, and next step.
- Revision loop: even if the round is only a supplement, correction, or plan sync, the reply ends by asking whether the user confirms the revision and stating which reviewable unit continues after confirmation.
- Summary granularity: the current reply is not a pure file/line list and does not copy the whole body; the user can understand purpose, key wording/stance, and subsequent impacts from the chat summary alone.

## Output Style

PRD writing should be restrained, precise, and implementation-oriented. Avoid marketing language and exaggerated wording. The document should help product, design, engineering, and QA understand feature behavior, boundaries, dependencies, and the business reason behind each requirement.
