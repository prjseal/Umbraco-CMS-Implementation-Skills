# One changeset per page

A page is written up once, approved once and applied leaves first. This file says how the parts a
page needs are routed to the peer skill whose rules apply, how their pages merge into one
changeset, and the order they are applied in. Open it when the page needs more than the common
case in [workflow.md](workflow.md#3-write-the-requirements-doc): a block, a block area or a new
data type.

## Parts and who writes them

| Part the page needs | When | Rules from | Pages written |
|---|---|---|---|
| New data type for a page field | Nothing on the site fits | [umbraco-add-data-type](../../umbraco-add-data-type/SKILL.md#used-by-another-skill) | `Data-Types/<Kind>/<Slug>.md`, plus the folder index if the folder is new |
| SEO composition | The site has none | [umbraco-add-composition](../../umbraco-add-composition/SKILL.md#used-by-another-skill) | `Document-Types/Compositions/SeoComposition.md`, plus the `Compositions` index and `Data-Types/Toggle/Toggle-default-on.md` if needed |
| Sharing or page-details composition | Missing | Not added; named as a follow-up | None |
| New block | The user names a block that does not exist | [umbraco-add-block](../../umbraco-add-block/SKILL.md#used-by-another-skill) | `Document-Types/Elements/...`, `Elements/Settings/...`, `Elements/Compositions/...`, `Data-Types/Block-List/...` as the block needs |
| Single new element | One element with no settings and no repeater | [umbraco-add-element-type](../../umbraco-add-element-type/SKILL.md#used-by-another-skill) | `Document-Types/Elements/<Alias>.md` |
| Block area | The page has one | [umbraco-configure-block-editor](../../umbraco-configure-block-editor/SKILL.md#used-by-another-skill), approach A | `Data-Types/Block-Grid/<Placement>-Block-Grid.md`: Create, or Update keeping every existing entry |
| Master template | The site has none | This skill | `Templates/Master.md` |
| Page template | Always | This skill | `Templates/Master/<Alias>.md` |
| Page schema | Always | [umbraco-add-document-type](../../umbraco-add-document-type/SKILL.md#used-by-another-skill), role page | `Document-Types/<Alias>.md` |
| Page behaviour: template link, allowed at root, own children | Always | This skill | The same page |
| Parent update | The parent exists | This skill | `Document-Types/HomePage.md` (Update) |
| Home page | The site has none | Ask, then this skill with the kind Root | `Document-Types/HomePage.md` and its template |

Each peer, when called from here, inspects, decides and writes only; it does not stop for its own
approval and does not apply. It hands back its pages, their dependency flags and its checklist
rows.

## Merge rules

- **One changeset file and one summary.** The summary says what the page is for and lists each
  part the page brought with it, in a sentence each.
- **Pages numbered in apply order**, leaves first (below). The Requirements pages table and the
  checklist agree.
- **Each folder created once.** A `Compositions`, `Elements` or data type folder that several
  parts need is one checklist item, at the first step that needs it, with its index page written
  once.
- **Dependency flags point inside the changeset.** A part written here is `New in this changeset`
  on every page that needs it, with a link; nothing in the changeset is `Missing`.
- **Allowed children set only after every type exists**, so the page's own children and the parent
  update are the last steps before verification.
- **One verify item at the end**, covering every requirements page in the changeset.

## Apply order

Leaves first, matching
[apply-via-mcp.md](../../umbraco-content-requirements-documentation/references/apply-via-mcp.md#order).
Skip any step the page does not need; keep the numbering contiguous in the checklist.

1. Data type folders and plain data types: page fields, the SEO toggle, block field types.
2. Document type folders that are missing: `Compositions`, `Elements`, `Elements/Compositions`,
   `Elements/Settings`.
3. `seoComposition` in `Compositions`, then fix-up (only if new).
4. Settings compositions: create, move into `Elements/Compositions`, fix-up.
5. Settings model: create, move into `Elements/Settings`, fix-up.
6. Repeater item elements: create, move into `Elements`, fix-up.
7. Block List data types.
8. Content elements: create, move into `Elements`, fix-up.
9. Block Grid data type: Create, or Update keeping the existing entries.
10. Template `master` (only if new).
11. The page template under `master`.
12. The page document type (its compositions include `seoComposition`; `mainContent` uses the
    grid), then fix-up.
13. The page's own allowed children.
14. The parent: add the new page, keeping the existing entries.
15. Verify every requirements doc against the site and set each status line.

## At the approval gate

Call out:

- every existing artefact that changes: the parent, an existing block area gaining a block, a
  settings model gaining a composition;
- the new shared artefacts: the SEO composition, a settings model or settings composition, and
  which existing pages lack SEO and are left alone;
- anything resting on the user's word rather than a read of the site.

## In the report

After verification, say what is left: the template markup, a controller for a page rendered by
code, a Razor partial for each new block, and the optional sharing or page-details composition.

## Example: a landing page with a new block

A site with `homePage`, `contentPage`, the three page compositions, a master template, blocks
`hero` and `richText` with the shared settings models, and a `Main Content Block Grid` used by
`contentPage.mainContent`. The user asks for a Landing Page built from blocks, with a new
Testimonial block. The changeset's checklist:

```
- [ ] 1. Create element `testimonial`, move it into `Elements`, then fix-up
- [ ] 2. Update data type `Main Content Block Grid`: add `testimonial` with `blockSettings`, keeping `hero` and `richText`
- [ ] 3. Create template `landingPage` under `master`
- [ ] 4. Create document type `landingPage`, then fix-up (sorts, descriptions, template)
- [ ] 5. Set allowed children on `homePage`: add `landingPage`, keeping the existing entries
- [ ] 6. Verify every requirements doc against the site and set each status line
```

Five requirements pages (the element, the grid as an Update, the template, the page, the home page
as an Update) and one approval. The testimonial's Razor partial and the landing page's markup are
what is left.

**Related:** [workflow.md](workflow.md), [page-kinds.md](page-kinds.md),
[minimum-viable-page.md](minimum-viable-page.md).
