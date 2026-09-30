---
name: umbraco-add-listing-page
description: >
  Add a listing section to an Umbraco 17+ site: the listing page type, the item page type it
  lists, the listing's own collection view data type and the allowed-children pairing between
  them, with both templates, hung under the right parent. The change is written as a markdown
  spec, approved, then applied through the Umbraco Developer MCP or a manual backoffice
  walkthrough, and read back.
  Use this whenever the user asks to "add a blog", "add a news section", "create a listing page",
  "add an events listing", "add an articles section with a list of articles", "set up a list view
  for the news items", or "configure the collection view for this listing".
  SKIP: non-Umbraco projects or Umbraco < 17; one standalone page type (use umbraco-add-page-type);
  non-routable data folders and items such as authors or categories (use umbraco-add-data-folder);
  compositions (use umbraco-add-composition); rendering the list, paging or filtering in Razor or
  C#; questions about naming only (use umbraco-content-model-conventions).
---

# Add Listing Page

Adds **a listing pair**: a routable listing page (`articleListingPage`) and the routable item it
lists (`articlePage`), made together because neither is useful alone. The listing gets its own
collection view so editors manage items as a table, and the two types allow each other the way
the conventions say. It works spec first: write the change down, stop for the user's approval,
apply it, then read it back.

One standalone page belongs to
[`umbraco-add-page-type`](../umbraco-add-page-type/SKILL.md); this skill reuses its decisions for
each of the two pages and adds only what the pairing needs.

The rules come from the sibling skill
[`umbraco-content-model-conventions`](../umbraco-content-model-conventions/SKILL.md), and the spec
format, inspect, apply and verify steps come from
[`umbraco-content-model-spec`](../umbraco-content-model-spec/SKILL.md). **Both must be installed
alongside this one.** If a link into either cannot be read, stop and say so; do not work from
memory.

## Steps

| Step | What happens | Reference |
|---|---|---|
| 1. Inspect | Home page, compositions, master template, existing listings and items | [workflow.md](references/workflow.md#1-inspect) |
| 2. Decide the pair | Names, compositions, fields, templates, parent and children | [workflow.md](references/workflow.md#2-decide) |
| 3. Decide the collection view | Columns, ordering, page size, layouts | [collection-view.md](references/collection-view.md) |
| 4. Write and lint | Collection view, two templates, two page types, the parent update, a changeset | [workflow.md](references/workflow.md#3-write-the-spec) |
| 5. Approve | **Stop.** The user approves the changeset | [workflow.md](references/workflow.md#4-stop-for-approval) |
| 6. Apply | Through the MCP if connected, otherwise a manual walkthrough | [workflow.md](references/workflow.md#5-apply) |
| 7. Verify | Read everything back, set the status lines, report | [workflow.md](references/workflow.md#6-verify-and-report) |

### How to decide between MCP and manual

There is one approach with two mechanisms, tried in order. Use the Umbraco Developer MCP when its
tools are in the connected tool list and the site answers. Otherwise write the same spec and hand
the user a manual backoffice walkthrough generated from it. Missing tools are never a reason to
skip the spec, to output uSync or `package.xml` files, or to say the section exists.

## Version compatibility

Targets **Umbraco 17+**. The collection view uses `Umbraco.ListView` with the editor UI
`Umb.PropertyEditorUi.Collection`, read from the Umbraco 17.5.3 packages. The MCP tools were
checked against `@umbraco-cms/mcp-dev` 17.6.8 and 18.1.7 (see the spec skill); the connected tool
list is the authority.

## Best practices

- **Always the pair.** A listing without its item type cannot list anything, and an item type with
  nowhere to live cannot be created. Specify both, and the allowed children between them, in one
  changeset.
- **Name them together.** `<noun>ListingPage` lists `<noun>Page`: `articleListingPage` and
  `articlePage`, `eventListingPage` and `eventPage`.
- **Each listing has its own collection view.** Columns and ordering differ per listing; a shared
  collection view changes every listing at once.
- **The listing is not the item's data.** Fields that describe an item (a date, a summary, an
  author) live on the item, or a composition; the listing shows them as columns.
- **Order by something the item holds.** Newest first by a date on the item is usually right; the
  create date is a fallback when the item has none.
- **Approval is a separate turn.** "Just add a blog" is a request for the section, not approval of
  a spec the user has not seen.

## Validation

Objective assertions live in [`evals/evals.json`](evals/evals.json); run them with
`umbraco-skill-evaluator`. Coverage tier: **Documented**. This skill ships no assets; its only
code is the template stub borrowed from `umbraco-add-page-type`, which no host compiles. The specs
it writes are checked by the spec skill's linter, and an eval graded the guidance. Nothing ran
against a live site.
