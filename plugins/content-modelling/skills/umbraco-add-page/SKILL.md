---
name: umbraco-add-page
description: >
  Add one page to an Umbraco 17+ site: a document type visitors land on, with its template (the
  template is what makes it a page), the SEO composition every page takes, and the parent that
  allows it, plus any blocks or block area the page needs. Every part goes into one requirements
  doc and one changeset, approved once, applied through the Umbraco Developer MCP or a manual
  backoffice walkthrough, and read back.
  Use this whenever the user asks to "add a page", "add a page type", "create a document type for
  a page", "add a contact page", "add a landing page with blocks", "scaffold a content page",
  "create a home page type", "add a search page type", or "just add a page type so editors can
  make pages".
  SKIP: Umbraco < 17; changing a page type that already exists (use
  umbraco-change-document-type); a listing page with its item type (use
  umbraco-add-listing-page); a document type with no template (use umbraco-add-document-type);
  data folders (use umbraco-add-data-folder); Razor markup or a controller for the page.
---

# Add Page

Adds **one page**: something visitors land on. A page is a document type plus the things that make
it a page: a template, the SEO composition and a parent that allows it. It works requirements-first:
write the change down, stop for the user's approval, apply it, then read it back.

| Concept | What it is | Who decides it |
|---|---|---|
| Page | A document type visitors land on: template, SEO, parent | This skill |
| Document type | The schema: name, folder, tabs, properties, compositions | [`umbraco-add-document-type`](../umbraco-add-document-type/SKILL.md) |
| Block | A content element registered in a block area, with an optional settings element that styles it | [`umbraco-add-block`](../umbraco-add-block/SKILL.md) |

This skill orchestrates. It decides what is specific to a page (its kind, template, SEO and place
in the tree) and calls the peer skills for everything else, gathering every part into **one
changeset with one approval**. The peers are not part of this skill: each also runs on its own and
in other flows.

The rules come from
[`umbraco-content-model-conventions`](../umbraco-content-model-conventions/SKILL.md), and the
requirements doc format and the six-step workflow from
[`umbraco-content-requirements-documentation`](../umbraco-content-requirements-documentation/SKILL.md)
([change-workflow.md](../umbraco-content-requirements-documentation/references/change-workflow.md));
both must be installed alongside this one, with the peer skills named below. If a link into any of
them cannot be read, stop and say so.

## Steps

| Step | What happens | Reference |
|---|---|---|
| 1. Inspect | Read the site: home page, master template, compositions, blocks, languages | [workflow.md](references/workflow.md#1-inspect) |
| 2. Decide the kind | Root, Content or Programmatic; Listing is handed off | [page-kinds.md](references/page-kinds.md) |
| 3. Decide the details | The schema through add-document-type; template, SEO, parent and children here; anything else to its peer | [workflow.md](references/workflow.md#2-decide), [combined-changeset.md](references/combined-changeset.md) |
| 4. Write and lint | Every part, the parent update and one changeset | [workflow.md](references/workflow.md#3-write-the-requirements-doc) |
| 5. Approve | **Stop.** The user approves the changeset | [workflow.md](references/workflow.md#4-stop-for-approval) |
| 6. Apply | Through the MCP if connected, otherwise a manual walkthrough | [workflow.md](references/workflow.md#5-apply) |
| 7. Verify | Read everything back, set the status lines, report | [workflow.md](references/workflow.md#6-verify-and-report) |

When the user asks for "a page type" and gives nothing more, propose the default in
[minimum-viable-page.md](references/minimum-viable-page.md) rather than asking a list of questions.

### The core page

Most pages are four requirements pages and five checklist items: the page template, the page, the
parent update and the changeset, with the SEO composition the site already has. Open
[combined-changeset.md](references/combined-changeset.md) only when the page needs something
beyond that: an SEO composition the site lacks, a new data type, a block or a block area.

### How to decide between MCP and manual

MCP when connected, otherwise a manual walkthrough from the same requirements doc; see
[change-workflow.md](../umbraco-content-requirements-documentation/references/change-workflow.md#5-apply).

## Version compatibility

Targets **Umbraco 17+**. MCP tool names were checked as described in
[`umbraco-content-requirements-documentation`](../umbraco-content-requirements-documentation/SKILL.md#version-compatibility);
the connected tool list is the authority.

## Best practices

- **A template is what makes it a page.** Every page has one, programmatic pages included; see
  [templates.md](../umbraco-content-model-conventions/references/templates.md). What the template
  renders is implementation work, not schema. Something visitors never land on is a document type,
  not a page; hand it to [`umbraco-add-document-type`](../umbraco-add-document-type/SKILL.md).
- **Every page takes SEO.** If the site has no SEO composition, it joins this changeset; it is never
  deferred and never flagged `Missing`; see
  [workflow.md](references/workflow.md#seo-is-mandatory).
- **One changeset, one approval.** Blocks, a block area and new data types the page needs are
  written into the same changeset by their peer skills' rules, leaf first; see
  [combined-changeset.md](references/combined-changeset.md).
- **Editing the parent is the riskiest call in the change.** It is an existing type with content
  on it, updated by a full-body replace. Read it, modify it, write it, and compare the property
  count afterwards.
- **No page other than the home page goes at root.** A new page becomes creatable by joining a
  parent's allowed children, which makes the parent part of the change; see
  [allowed-children-and-root.md](../umbraco-content-model-conventions/references/allowed-children-and-root.md#allowed-at-root).
- **A page type is mostly compositions and behaviour.** Give it only the fields no composition
  supplies; a page with no properties of its own is normal, see
  [compositions.md](../umbraco-content-model-conventions/references/compositions.md).
- **"Just create it" is a request for the page, not approval** of a requirements doc the user has
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
