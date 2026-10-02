---
name: umbraco-add-composition
description: >
  Add one single-concern composition to an Umbraco 17+ site and apply it to the document types
  that need it: SEO fields, sharing fields, page details, visibility and the like. The change is
  written as a requirements doc, approved, applied through the Umbraco Developer MCP or a manual
  backoffice walkthrough, and read back.
  Use this whenever the user asks to "add a composition", "create an SEO composition", "add open
  graph fields to every page", "share these fields between page types", "add meta title and
  description to our pages", "move these fields into a composition", or "which pages should get
  this composition".
  SKIP: non-Umbraco projects or Umbraco < 17; adding or removing fields on one existing type (use
  umbraco-change-document-type); settings compositions for block settings models (use
  umbraco-add-element-type); adding a page type (use umbraco-add-page-type); questions about
  naming only (use umbraco-content-model-conventions).
---

# Add Composition

Adds **one composition for document types**: a reusable set of fields that does one job, placed
in `Compositions/`, and applied to the page types that need that job. It works
requirements-first: write the change down, stop for the user's approval, apply it, then read it
back.

This skill decides only what is specific to a composition. The rules come from
[`umbraco-content-model-conventions`](../umbraco-content-model-conventions/SKILL.md), and the
requirements doc format and
[the six-step workflow](../umbraco-content-requirements-documentation/references/change-workflow.md)
from
[`umbraco-content-requirements-documentation`](../umbraco-content-requirements-documentation/SKILL.md);
both must be installed alongside this one. If a link into either cannot be read, stop and say so.

## Steps

| Step | What happens | Reference |
|---|---|---|
| 1. Inspect | Read the existing compositions and every type that would take the new one | [workflow.md](references/workflow.md#1-inspect) |
| 2. Decide the concern | One job, its name, tab, group and fields | [standard-compositions.md](references/standard-compositions.md) |
| 3. Decide where it goes | Which types take it, and which must not | [when-to-apply.md](references/when-to-apply.md) |
| 4. Write and lint | The composition, an Update for each target type, a changeset | [workflow.md](references/workflow.md#3-write-the-requirements-doc) |
| 5. Approve | **Stop.** The user approves the changeset | [workflow.md](references/workflow.md#4-stop-for-approval) |
| 6. Apply | Through the MCP if connected, otherwise a manual walkthrough | [workflow.md](references/workflow.md#5-apply) |
| 7. Verify | Read everything back, set the status lines, report | [workflow.md](references/workflow.md#6-verify-and-report) |

### How to decide between MCP and manual

One approach, two mechanisms: the Umbraco Developer MCP when it is connected, otherwise a manual
walkthrough generated from the same requirements doc; see
[change-workflow.md](../umbraco-content-requirements-documentation/references/change-workflow.md#5-apply).

## Version compatibility

Targets **Umbraco 17+**. MCP tool names were checked as described in
[`umbraco-content-requirements-documentation`](../umbraco-content-requirements-documentation/SKILL.md#version-compatibility);
the connected tool list is the authority.

## Best practices

- **One concern, flat, fields only.** The composition rules are in
  [compositions.md](../umbraco-content-model-conventions/references/compositions.md#rules).
- **Applying is editing existing types.** Every target is an existing type, often with content,
  changed by a full-body replace. Read it, modify it, write it, and compare the property count.
- **A clashing alias blocks the composition.** A type that already has a property with one of the
  composition's aliases cannot take it. Moving such a field into a composition deletes the stored
  values unless they are migrated first, so stop and say so; never remove a property to make room.
- **"Just add it to every page" is not approval.** It is a request for the composition; the user
  still approves the exact list of targets at
  [the approval gate](../umbraco-content-requirements-documentation/references/change-workflow.md#4-stop-for-approval).

## Validation

Objective assertions live in [`evals/evals.json`](evals/evals.json); run them with
`umbraco-skill-evaluator`. Coverage tier: **Documented**. This skill ships no code and no assets.
The requirements docs it writes are checked by the linter in
`umbraco-content-requirements-documentation`, and an eval graded the guidance. Nothing ran against
a live site.
