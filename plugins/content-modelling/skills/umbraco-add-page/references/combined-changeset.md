# Combined changeset

A page often needs more than its own type and template: an SEO composition the site lacks, a block
that does not exist yet, a block area to put it in. Each of those has a peer skill with its own
rules. This file routes each part to the peer whose rules apply and merges everything into **one
changeset with one approval**.

Each peer is called as its `Used by another skill` section describes: it inspects, decides and
writes its requirements pages and checklist rows, and hands them back. It does not stop for
approval; this skill does, once, for the whole change.

## Which peer decides each part

| Part the page needs | When | Rules come from | Pages written |
|---|---|---|---|
| A new data type for a page field | No data type on the site fits | [umbraco-add-data-type](../../umbraco-add-data-type/SKILL.md#used-by-another-skill) | `Data-Types/<Editor-Kind>/<Name-Slug>.md` |
| The SEO composition | The site has none | [umbraco-add-composition](../../umbraco-add-composition/SKILL.md#used-by-another-skill), as [SEO is mandatory](workflow.md#seo-is-mandatory) says | `Document-Types/Compositions/SeoComposition.md`, plus a toggle data type and folder index if missing |
| A sharing or page-details composition | The site has none | Not added; named as a follow-up | — |
| A new block | The user names a block the site does not have | [umbraco-add-block](../../umbraco-add-block/SKILL.md#used-by-another-skill) | Its elements, settings model, settings compositions and Block List |
| A single new element | One element with no settings and no repeater | [umbraco-add-element-type](../../umbraco-add-element-type/SKILL.md#used-by-another-skill) | `Document-Types/Elements/<AliasPascalCase>.md` |
| A block area | The page has a block area | [umbraco-configure-block-editor](../../umbraco-configure-block-editor/SKILL.md#used-by-another-skill), approach A | `Data-Types/Block-Grid/<Name-Slug>.md`: Create for a new area, Update when an existing grid gains a block |
| The master template | The site has none | This skill ([templates.md](../../umbraco-content-model-conventions/references/templates.md)) | `Templates/Master.md` |
| The page template | Always | This skill | `Templates/<MasterPascalCase>/<AliasPascalCase>.md` |
| The page's schema | Always | [umbraco-add-document-type](../../umbraco-add-document-type/SKILL.md#used-by-another-skill), role *page* | `Document-Types/<AliasPascalCase>.md` |
| The page's behaviour (template, allowed at root, own children) | Always | This skill ([page-kinds.md](page-kinds.md)) | The same page |
| The parent update | The parent exists | This skill ([requirements-lifecycle.md](../../umbraco-content-requirements-documentation/references/requirements-lifecycle.md#updating-a-type-that-already-exists)) | `Document-Types/HomePage.md` (Update) |
| A home page | The site has none | Ask first ([the root question](workflow.md#the-root-question)); if yes, this skill again with kind Root | `Document-Types/HomePage.md` and its template |

Reuse first. An existing block, block area, data type or composition is used as it is and flagged
`Exists`; only what the site lacks goes to a peer.

## Merging the parts

- **One changeset file, one Summary.** The summary says what the page is and lists every new
  shared artefact (an SEO composition, a settings model, a block other pages can use).
- **Requirements pages are numbered in apply order** in the changeset's Requirements pages table.
- **Each folder is created once,** however many parts need it.
- **Allowed children are set last,** once every type exists.
- **One verify item,** at the end.

## Apply order

Leaf first, matching
[apply-via-mcp.md](../../umbraco-content-requirements-documentation/references/apply-via-mcp.md#order).
Keep only the rows the page needs:

```
- [ ] 1. Create data type folders and data types (page fields, the SEO toggle, block fields)   (only if new)
- [ ] 2. Create document type folders `Compositions`, `Elements`, `Elements/Compositions`, `Elements/Settings`   (only those missing)
- [ ] 3. Create composition `seoComposition` in `Compositions`, then fix-up                    (only if new)
- [ ] 4. Create each settings composition, move it into `Elements/Compositions`, then fix-up    (only if new)
- [ ] 5. Create settings model `<alias>`, move it into `Elements/Settings`, then fix-up         (only if new)
- [ ] 6. Create each repeater item element, move it into `Elements`, then fix-up                (repeater only)
- [ ] 7. Create each Block List data type                                                     (repeater only)
- [ ] 8. Create each content element, move it into `Elements`, then fix-up                     (new blocks only)
- [ ] 9. Create or update data type `<Block Grid>`, keeping the existing entries               (block area only)
- [ ] 10. Create template `master`                                                           (only if new)
- [ ] 11. Create template `<alias>` under `master`
- [ ] 12. Create document type `<alias>` with `seoComposition`, then fix-up (sorts, mandatory, descriptions, template, culture)
- [ ] 13. Set allowed children on `<alias>`: `<alias>`                                       (content page only)
- [ ] 14. Set allowed children on `homePage`: add `<alias>`, keeping the existing entries
- [ ] 15. Verify every requirements doc against the site and set each status line
```

## At the approval gate

Call out, beside the parent update the [workflow](workflow.md#4-stop-for-approval) already names:

- every **existing** artefact that changes: a Block Grid that gains a block, a settings model that
  gains a settings composition;
- every **new shared** artefact: the SEO composition, a settings model, a block other pages can
  use, and which existing pages lack SEO and are left alone;
- anything that rests on the user's word rather than a read of the site;
- the follow-ups outside the change: the template's markup, block partial views, a controller for
  a programmatic page, and optional compositions the site lacks.

**Related:** [workflow.md](workflow.md), [minimum-viable-page.md](minimum-viable-page.md).
