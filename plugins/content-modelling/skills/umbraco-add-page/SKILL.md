---
name: umbraco-add-page
description: >
  Add one page to an Umbraco 17+ site: the document type, its template, the SEO composition it
  takes and the parent that allows it, plus any block, block area or data type the page needs,
  in one changeset with one approval. The change is written as a requirements doc, approved,
  applied leaves first through the Umbraco Developer MCP or a manual backoffice walkthrough, and
  read back.
  Use this whenever the user asks to "add a page", "add a page type", "create a document type for
  a page", "add a contact page", "create a home page type", "add a search page", "add a landing
  page with a testimonial block", or "add a new kind of page to Umbraco".
  SKIP: non-Umbraco projects or Umbraco < 17; a type with no template that visitors never land on
  (use umbraco-add-document-type); changing a page that exists (use umbraco-change-document-type);
  a listing page with its item type (use umbraco-add-listing-page); data folders and items (use
  umbraco-add-data-folder); writing the Razor markup or a controller for the page.
---

# Add Page

Adds **one page**: a document type visitors land on, with its template, the SEO composition, its
place under a parent, and whatever else the page needs that the site does not yet have: a block,
a block area, a data type for one of its fields. Everything the page needs is written up together,
approved once, then applied leaves first so each part exists before something depends on it.

A template is what makes a page; see [the terms](../umbraco-content-model-conventions/SKILL.md#terms).
The parts a page is made of are each decided by the peer skill that owns that concept. This skill
calls those peers, merges what they write into one changeset and owns the single approval. The
peers are not its children: every one of them works on its own as well.

The rules come from [`umbraco-content-model-conventions`](../umbraco-content-model-conventions/SKILL.md),
and the requirements doc format and the six-step workflow from
[`umbraco-content-requirements-documentation`](../umbraco-content-requirements-documentation/SKILL.md)
([change-workflow.md](../umbraco-content-requirements-documentation/references/change-workflow.md));
both must be installed alongside this one, as must the peers below. If a link into any of them
cannot be read, stop and say so.

## What a page is made of

| Part | Decided by | When |
|---|---|---|
| Kind, template, parent and children, the SEO rule | This skill | Always |
| The document type's schema: name, alias, folder, icon, tabs, fields, compositions, culture | [umbraco-add-document-type](../umbraco-add-document-type/SKILL.md#used-by-another-skill), role page | Always |
| The SEO composition | [umbraco-add-composition](../umbraco-add-composition/SKILL.md#used-by-another-skill) | Created only when the site has none; taken by the page always |
| A data type for a field nothing on the site fits | [umbraco-add-data-type](../umbraco-add-data-type/SKILL.md#used-by-another-skill) | When the page needs one |
| A block the user names that does not exist | [umbraco-add-block](../umbraco-add-block/SKILL.md#used-by-another-skill) | When the page needs one |
| A single element with no settings and no repeater | [umbraco-add-element-type](../umbraco-add-element-type/SKILL.md#used-by-another-skill) | When the page needs one |
| The block area where editors combine blocks | [umbraco-configure-block-editor](../umbraco-configure-block-editor/SKILL.md#used-by-another-skill), approach A | When the page has one |

A peer called from here inspects, decides and writes its own requirements pages, then hands them
back with their checklist rows. It does not stop for its own approval. How the pages merge into
one changeset, and the order they are applied in, is in
[combined-changeset.md](references/combined-changeset.md).

## Steps

| Step | What happens | Reference |
|---|---|---|
| 1. Inspect | Read the site: home page, master template, compositions (SEO first), block areas, blocks, data types, languages | [workflow.md](references/workflow.md#1-inspect) |
| 2. Decide the kind | Root, Content or Programmatic; Listing is handed off | [page-kinds.md](references/page-kinds.md) |
| 3. Decide the details | Reuse first; the schema through `add-document-type`; template, parent, children, SEO, blocks | [workflow.md](references/workflow.md#2-decide) |
| 4. Write and lint | One changeset holding every part the page needs, pages numbered in apply order | [workflow.md](references/workflow.md#3-write-the-requirements-doc), [combined-changeset.md](references/combined-changeset.md) |
| 5. Approve | **Stop.** One approval for the whole changeset | [workflow.md](references/workflow.md#4-stop-for-approval) |
| 6. Apply | Leaves first; through the MCP if connected, otherwise a manual walkthrough | [workflow.md](references/workflow.md#5-apply) |
| 7. Verify | Read everything back, set the status lines, say what is left | [workflow.md](references/workflow.md#6-verify-and-report) |

When the user asks for "a page" and gives nothing more, propose the default in
[minimum-viable-page.md](references/minimum-viable-page.md) rather than asking a list of questions.

### Page or document type?

A page is a document type visitors are meant to land on, and the template is what makes it one.
Any node can have a URL; nothing renders it as a page without a template. When it is not clear
whether visitors will land on the thing asked for (an office, an author, a careers entry), ask
that one question and nothing else. A yes stays here. A no goes to
[`umbraco-add-document-type`](../umbraco-add-document-type/SKILL.md), which asks the same question
the other way round and hands a page back here.

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
- **A template is what makes it a page**, programmatic pages included; see
  [templates.md](../umbraco-content-model-conventions/references/templates.md). What the template
  renders is implementation work, not schema.
- **Every page takes SEO.** If the site has no SEO composition, the first page brings it with it;
  see [SEO is mandatory](references/workflow.md#seo-is-mandatory). Sharing and page-details
  compositions stay optional.
- **A page is mostly compositions and behaviour.** Give it only the fields no composition
  supplies; a page with no properties of its own is normal, see
  [compositions.md](../umbraco-content-model-conventions/references/compositions.md).
- **One page, one approval.** A block, a block area or a data type the page needs joins the same
  changeset and is approved with it, leaves first; see
  [combined-changeset.md](references/combined-changeset.md). Do not split a page into several
  approvals, and do not let a peer stop for its own.
- **Type the alias by hand**, never Umbraco's generated one for a name with an acronym; see
  [naming.md](../umbraco-content-model-conventions/references/naming.md#casing).
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
a live site. The eval fixtures under `evals/files/` are this skill's own copies; sibling skills
keep theirs, because eval file paths resolve relative to the skill, so the copies can drift.
