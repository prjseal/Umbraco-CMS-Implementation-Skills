---
name: umbraco-add-page-type
description: >
  Add one routable page type to an Umbraco 17+ site the way an experienced Umbraco developer
  would: the document type and its template, named, placed, composed and hung under the right
  parent. The change is written as a markdown spec, approved, then applied through the Umbraco
  Developer MCP or a manual backoffice walkthrough, and read back.
  Use this whenever the user asks to "add a page type", "create a document type for a page",
  "add a contact page", "scaffold a content page", "create a home page type", "add a search page
  type", "add a new kind of page to Umbraco", or "just add a page type so editors can make pages".
  SKIP: non-Umbraco projects or Umbraco < 17; a listing page with its item type (use
  umbraco-add-listing-page); compositions (use umbraco-add-composition); element types and blocks
  (use umbraco-add-element-type); data folders and other non-routable types (use
  umbraco-add-data-folder); site settings (use umbraco-add-site-settings); questions about naming
  only (use umbraco-content-model-conventions); writing the Razor markup or a controller for the page.
---

# Add Page Type

Adds **one routable page type plus its template**: the kind of page, its name and alias, where it
sits, which compositions it takes, what it holds, which parent allows it and what it allows in
turn. It works spec first: write the change down, stop for the user's approval, apply it, then
read it back.

This skill decides only what is specific to a page type. The rules come from the sibling skill
[`umbraco-content-model-conventions`](../umbraco-content-model-conventions/SKILL.md), and the spec
format, inspect, apply and verify steps come from
[`umbraco-content-requirements-documentation`](../umbraco-content-requirements-documentation/SKILL.md). **Both must be installed
alongside this one.** If a link into either cannot be read, stop and say so; do not work from
memory.

## Steps

| Step | What happens | Reference |
|---|---|---|
| 1. Inspect | Read the site: home page, master template, compositions, languages | [workflow.md](references/workflow.md#1-inspect) |
| 2. Decide the kind | Root, Content or Programmatic; Listing is handed off | [page-kinds.md](references/page-kinds.md) |
| 3. Decide the details | Name, alias, icon, compositions, own properties, template, parent, children | [workflow.md](references/workflow.md#2-decide) |
| 4. Write and lint | The page type, its template, the parent update and a changeset | [workflow.md](references/workflow.md#3-write-the-spec) |
| 5. Approve | **Stop.** The user approves the changeset | [workflow.md](references/workflow.md#4-stop-for-approval) |
| 6. Apply | Through the MCP if connected, otherwise a manual walkthrough | [workflow.md](references/workflow.md#5-apply) |
| 7. Verify | Read everything back, set the status lines, report | [workflow.md](references/workflow.md#6-verify-and-report) |

When the user asks for "a page type" and gives nothing more, propose the default in
[minimum-viable-page.md](references/minimum-viable-page.md) rather than asking a list of questions.

### How to decide between MCP and manual

There is one approach with two mechanisms, tried in order. Use the Umbraco Developer MCP when its
tools are in the connected tool list and the site answers. Otherwise write the same spec and hand
the user a manual backoffice walkthrough generated from it. Missing tools are never a reason to
skip the spec, to output uSync or `package.xml` files, or to say the page type exists.

## Version compatibility

Targets **Umbraco 17+**, the version the conventions were derived from and the MCP tools were
checked against (`@umbraco-cms/mcp-dev` 17.6.8 and 18.1.7; see the spec skill). Tool names drift
between MCP versions, so the connected tool list is the authority. The old tools
`get-document-type-allowed-at-root` and `search-document-type` do not exist.

## Best practices

- **The alias is forever.** Templates, Models Builder classes and content queries all read it.
  Decide it in the spec, type it by hand in the backoffice, and never accept Umbraco's generated
  alias for a name with an acronym in it.
- **A page type is mostly compositions and behaviour.** Reuse the site's compositions and give
  the type only the fields no composition supplies. A content page with no properties of its own
  is normal.
- **Routable means a template.** Every page kind, programmatic ones included, gets one default
  template named after its alias under the master. What the template renders is implementation
  work, not schema.
- **Nothing but the home page goes at root.** A new page becomes creatable by being added to a
  parent's allowed children, which makes the parent part of the change. Never allow a page at
  root just so an editor can create it.
- **Editing the parent is the riskiest call in the change.** It is an existing type with content
  on it, updated by a full-body replace. Read it, modify it, write it, and compare the property
  count afterwards.
- **Approval is a separate turn.** A request to "just create it" is a request for the type, not
  approval of a spec the user has not seen.

## Validation

Objective assertions live in [`evals/evals.json`](evals/evals.json); run them with
`umbraco-skill-evaluator`. Coverage tier: **Documented**. This skill ships no assets. Its only
code is the two-line template stub in [workflow.md](references/workflow.md#5-apply), which no host
compiles. The specs it writes are checked by the spec skill's linter, and an eval graded the
guidance. Nothing ran against a live site.
