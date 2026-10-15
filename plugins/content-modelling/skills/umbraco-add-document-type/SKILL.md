---
name: umbraco-add-document-type
description: >
  Specify the schema of one Umbraco 17+ document type that is not an element type: its name,
  alias, folder, icon, description, tabs, properties, compositions and data types. It never adds a
  template: its nodes may still have URLs (the Delivery API), but a type visitors land on is a
  page. Other skills use it for each document type they add. Written as a requirements doc,
  approved, applied through the Umbraco Developer MCP or a manual backoffice walkthrough, and read
  back.
  Use this whenever the user asks to "add a document type", "create a document type with these
  fields", "define the tabs and properties for a new type", or "add a document type with no
  template".
  SKIP: Umbraco < 17; a page visitors land on (use umbraco-add-page); element types and blocks
  (use umbraco-add-element-type or umbraco-add-block); compositions (use
  umbraco-add-composition); data folders (use umbraco-add-data-folder); site settings (use
  umbraco-add-site-settings); changing an existing type (use umbraco-change-document-type).
---

# Add Document Type

Specifies **the schema of one document type**: what it is called, where it sits in the tree, and
the tabs, groups, properties and compositions that make up its structure. It never adds a
template. It works requirements-first: write the change down, stop for the user's approval, apply
it, then read it back.

A document type is the schema of something. A **page** is a document type visitors are meant to
land on: it has a template, takes the SEO composition and sits under a parent that allows it, and
[`umbraco-add-page`](../umbraco-add-page/SKILL.md) adds those on top of the schema this skill
decides. Any content node can have a URL, so whether a type is a page is a question of intent, not
of mechanics.

The rules come from [`umbraco-content-model-conventions`](../umbraco-content-model-conventions/SKILL.md),
and the requirements doc format and the six-step workflow from
[`umbraco-content-requirements-documentation`](../umbraco-content-requirements-documentation/SKILL.md)
([change-workflow.md](../umbraco-content-requirements-documentation/references/change-workflow.md));
both must be installed alongside this one. If a link into either cannot be read, stop and say so.

## Steps

| Step | What happens | Reference |
|---|---|---|
| 1. Settle the role | Page, composition, data, site settings, or a plain type; hand a page to add-page | [type-roles.md](references/type-roles.md) |
| 2. Inspect | Existing types, compositions, folders, data types, languages | [workflow.md](references/workflow.md#1-inspect) |
| 3. Decide the schema | Name, alias, folder, icon, description, tabs, properties, compositions | [workflow.md](references/workflow.md#2-decide) |
| 4. Write and lint | The type, any new data type, a changeset | [workflow.md](references/workflow.md#3-write-the-requirements-doc) |
| 5. Approve | **Stop.** The user approves the changeset | [workflow.md](references/workflow.md#4-stop-for-approval) |
| 6. Apply | Through the MCP if connected, otherwise a manual walkthrough | [workflow.md](references/workflow.md#5-apply) |
| 7. Verify | Read everything back, set the status lines, report | [workflow.md](references/workflow.md#6-verify-and-report) |

### How to decide between MCP and manual

MCP when connected, otherwise a manual walkthrough from the same requirements doc; see
[change-workflow.md](../umbraco-content-requirements-documentation/references/change-workflow.md#5-apply).

## Used by another skill

When another skill (such as `umbraco-add-page`, `umbraco-add-listing-page`,
`umbraco-add-data-folder` or `umbraco-add-site-settings`) calls this one, it names the role; run
inspect, decide and write only, and hand back the type's requirements page and its checklist rows.
The calling skill fills in the behaviour rows (template, allowed at root, allowed children,
collection) and owns the changeset, the single approval and the apply order, so do not stop for
approval separately.

## Version compatibility

Targets **Umbraco 17+**. MCP tool names were checked as described in
[`umbraco-content-requirements-documentation`](../umbraco-content-requirements-documentation/SKILL.md#version-compatibility);
the connected tool list is the authority.

## Best practices

- **No template, ever.** A type that needs one is a page; hand it to
  [`umbraco-add-page`](../umbraco-add-page/SKILL.md).
- **Ask about intent when it is unclear.** "Will visitors land on this?" decides page or document
  type. A type read only through the Delivery API, or shown only inside other pages, is not a page.
- **The folder follows the role.** The Document Types root is for pages and `siteSettings`; a plain
  type goes in `Data/`; see [type-roles.md](references/type-roles.md).
- **Fields only where no composition gives them**, and reuse a data type whenever its configuration
  fits; see
  [compositions.md](../umbraco-content-model-conventions/references/compositions.md).
- **Type the alias by hand**, never Umbraco's generated one for a name with an acronym; see
  [naming.md](../umbraco-content-model-conventions/references/naming.md#casing).
- "Just create it" is a request for the type, not approval of a requirements doc the user has not
  yet seen; the gate is in
  [change-workflow.md](../umbraco-content-requirements-documentation/references/change-workflow.md#4-stop-for-approval).

## Validation

Objective assertions live in [`evals/evals.json`](evals/evals.json); run them with
`umbraco-skill-evaluator`. Coverage tier: **Documented**. This skill ships no code and no assets.
The requirements docs it writes are checked by the linter in
`umbraco-content-requirements-documentation`, and an eval graded the guidance. Nothing ran against
a live site.
