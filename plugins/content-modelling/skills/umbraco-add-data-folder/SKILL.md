---
name: umbraco-add-data-folder
description: >
  Add shared, non-routable content to an Umbraco 17+ site: a data folder document type allowed at
  the content root and the data item it holds (authors, categories, locations, reusable content),
  plus the picker data type pages use to choose items, including content-driven taxonomy. The
  change is written as a markdown spec, approved, then applied through the Umbraco Developer MCP or
  a manual backoffice walkthrough, and read back.
  Use this whenever the user asks to "add authors", "add categories", "set up a taxonomy", "store
  reusable content", "add a data folder", "create a folder of locations editors can pick from",
  "let articles pick an author", or "where should shared data live in the content tree".
  SKIP: non-Umbraco projects or Umbraco < 17; routable pages or listing sections (use
  umbraco-add-page-type or umbraco-add-listing-page); the site settings singleton (use
  umbraco-add-site-settings); document type folders in Settings, which are organisation only
  (see umbraco-content-model-conventions); media folders; free-form tags (use umbraco-add-data-type).
---

# Add Data Folder

Adds **shared data that belongs to no page**: a folder document type editors create at the root of
the content tree (`authorFolder`) and the item type it holds (`author`). Pages then pick items from
it instead of retyping the same name on every page. It works spec first: write the change down,
stop for the user's approval, apply it, then read it back.

"Folder" here is a **document type** that holds content nodes, not the organisational folders in
the Settings tree.

The rules come from the sibling skill
[`umbraco-content-model-conventions`](../umbraco-content-model-conventions/SKILL.md), and the spec
format, inspect, apply and verify steps come from
[`umbraco-content-model-spec`](../umbraco-content-model-spec/SKILL.md). **Both must be installed
alongside this one.** If a link into either cannot be read, stop and say so; do not work from
memory.

## Steps

| Step | What happens | Reference |
|---|---|---|
| 1. Inspect | Existing data folders and items, what is allowed at root, the `Data/` folder | [workflow.md](references/workflow.md#1-inspect) |
| 2. Decide | Folder and item names, fields, nesting, the picker and who uses it | [workflow.md](references/workflow.md#2-decide) |
| 3. Taxonomy | Categories and similar controlled lists: data items or tags | [taxonomy.md](references/taxonomy.md) |
| 4. Write and lint | Folder, item, picker data type, the types that gain a picker, a changeset | [workflow.md](references/workflow.md#3-write-the-spec) |
| 5. Approve | **Stop.** The user approves the changeset | [workflow.md](references/workflow.md#4-stop-for-approval) |
| 6. Apply | Through the MCP if connected, otherwise a manual walkthrough | [workflow.md](references/workflow.md#5-apply) |
| 7. Verify | Read everything back, set the status lines, report | [workflow.md](references/workflow.md#6-verify-and-report) |

### How to decide between MCP and manual

There is one approach with two mechanisms, tried in order. Use the Umbraco Developer MCP when its
tools are in the connected tool list and the site answers. Otherwise write the same spec and hand
the user a manual backoffice walkthrough generated from it. Missing tools are never a reason to
skip the spec, to output uSync or `package.xml` files, or to say the data folder exists.

## Version compatibility

Targets **Umbraco 17+**. The picker uses `Umbraco.MultiNodeTreePicker` with the editor UI
`Umb.PropertyEditorUi.ContentPicker`, read from the Umbraco 17.5.3 packages. The MCP tools were
checked against `@umbraco-cms/mcp-dev` 17.6.8 and 18.1.7 (see the spec skill); the connected tool
list is the authority.

## Best practices

- **Data is not a page.** Folders and items have no template, no SEO compositions and no URL an
  editor should care about. They live in `Data/` in the Document Types tree and at the content
  root beside the home page.
- **The folder is at root; the item never is.** A data folder is one of the three kinds of type
  allowed at root. The item is created inside it.
- **Pick, do not retype.** A page refers to an item through a picker restricted to that folder and
  item type, so renaming an author changes it everywhere.
- **One picker data type per use.** "Pick one author" and "pick up to three categories" are
  different configurations, and each is its own data type.
- **Approval is a separate turn.** "Just add categories" is a request for the change, not approval
  of a spec the user has not seen.

## Validation

Objective assertions live in [`evals/evals.json`](evals/evals.json); run them with
`umbraco-skill-evaluator`. Coverage tier: **Documented**. This skill ships no code and no assets.
The specs it writes are checked by the spec skill's linter, and an eval graded the guidance.
Nothing ran against a live site.
