---
name: umbraco-add-page-type
description: >
  Add one routable page type to an Umbraco 17+ site: the document type and its template, named,
  placed, composed and hung under the right parent. The change is written as a requirements doc,
  approved, applied through the Umbraco Developer MCP or a manual backoffice walkthrough, and read
  back.
  Use this whenever the user asks to "add a page type", "create a document type for a page",
  "add a contact page", "scaffold a content page", "create a home page type", "add a search page
  type", "add a new kind of page to Umbraco", or "just add a page type so editors can make pages".
  SKIP: non-Umbraco projects or Umbraco < 17; changing a page type that already exists (use
  umbraco-change-document-type); a listing page with its item type (use
  umbraco-add-listing-page); compositions (use umbraco-add-composition); data folders and other
  non-routable types (use umbraco-add-data-folder); writing the Razor markup or a controller for
  the page.
---

# Add Page Type

Adds **one routable page type plus its template**: the kind of page, its name and alias, where it
sits, which compositions it takes, what it holds, which parent allows it and what it allows in
turn. It works requirements-first: write the change down, stop for the user's approval, apply it,
then read it back.

This skill decides only what is specific to a page type. The rules come from
[`umbraco-content-model-conventions`](../umbraco-content-model-conventions/SKILL.md), and the
requirements doc format and the six-step workflow from
[`umbraco-content-requirements-documentation`](../umbraco-content-requirements-documentation/SKILL.md)
([change-workflow.md](../umbraco-content-requirements-documentation/references/change-workflow.md));
both must be installed alongside this one. If a link into either cannot be read, stop and say so.

## Steps

| Step | What happens | Reference |
|---|---|---|
| 1. Inspect | Read the site: home page, master template, compositions, languages | [workflow.md](references/workflow.md#1-inspect) |
| 2. Decide the kind | Root, Content or Programmatic; Listing is handed off | [page-kinds.md](references/page-kinds.md) |
| 3. Decide the details | Name, alias, icon, compositions, own properties, template, parent, children | [workflow.md](references/workflow.md#2-decide) |
| 4. Write and lint | The page type, its template, the parent update and a changeset | [workflow.md](references/workflow.md#3-write-the-requirements-doc) |
| 5. Approve | **Stop.** The user approves the changeset | [workflow.md](references/workflow.md#4-stop-for-approval) |
| 6. Apply | Through the MCP if connected, otherwise a manual walkthrough | [workflow.md](references/workflow.md#5-apply) |
| 7. Verify | Read everything back, set the status lines, report | [workflow.md](references/workflow.md#6-verify-and-report) |

When the user asks for "a page type" and gives nothing more, propose the default in
[minimum-viable-page.md](references/minimum-viable-page.md) rather than asking a list of questions.

### How to decide between MCP and manual

MCP when connected, otherwise a manual walkthrough from the same requirements doc; see
[change-workflow.md](../umbraco-content-requirements-documentation/references/change-workflow.md#5-apply).

## Version compatibility

Targets **Umbraco 17+**. MCP tool names were checked as described in
[`umbraco-content-requirements-documentation`](../umbraco-content-requirements-documentation/SKILL.md#version-compatibility);
the connected tool list is the authority.

## Best practices

- **Editing the parent is the riskiest call in the change.** It is an existing type with content
  on it, updated by a full-body replace. Read it, modify it, write it, and compare the property
  count afterwards.
- **No page other than the home page goes at root.** A new page becomes creatable by joining a
  parent's allowed children, which makes the parent part of the change; see
  [allowed-children-and-root.md](../umbraco-content-model-conventions/references/allowed-children-and-root.md#allowed-at-root).
- **Routable means a template**, programmatic pages included; see
  [templates.md](../umbraco-content-model-conventions/references/templates.md). What the template
  renders is implementation work, not schema.
- **A page type is mostly compositions and behaviour.** Give it only the fields no composition
  supplies; a page with no properties of its own is normal, see
  [compositions.md](../umbraco-content-model-conventions/references/compositions.md).
- **Type the alias by hand**, never Umbraco's generated one for a name with an acronym; see
  [naming.md](../umbraco-content-model-conventions/references/naming.md#casing).
- **"Just create it" is a request for the type, not approval** of a requirements doc the user has
  not yet seen; the gate is in
  [change-workflow.md](../umbraco-content-requirements-documentation/references/change-workflow.md#4-stop-for-approval).

## Validation

Objective assertions live in [`evals/evals.json`](evals/evals.json); run them with
`umbraco-skill-evaluator`. Coverage tier: **Documented**. This skill ships no assets. Its only
code is the two-line Razor layout stub in [workflow.md](references/workflow.md#5-apply), which is
uncompiled context that no host builds; for real templates see the
[Umbraco templates docs](https://docs.umbraco.com/umbraco-cms/fundamentals/design/templates.md)
or the implementation plugin. The requirements docs it writes are checked by the linter in
`umbraco-content-requirements-documentation`, and an eval graded the guidance. Nothing ran against
a live site.
