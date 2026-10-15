# Workflow

Follow
[the six steps](../../umbraco-content-requirements-documentation/references/change-workflow.md).
This file adds only what a page needs at each step. The page's schema is decided by
[umbraco-add-document-type](../../umbraco-add-document-type/SKILL.md) with the role *page*; any
other part the page needs is routed to its peer skill by
[combined-changeset.md](combined-changeset.md).

## 1. Inspect

For a page, these are the answers you need before deciding anything:

| Question | Why it matters here |
|---|---|
| Does a type with this name or alias already exist? | Do not create a second one; offer to extend the first |
| Is there a home page, and what is allowed at root? | Decides the parent, and whether the root question below must be asked |
| What does the home page allow now? | The parent update keeps every existing entry |
| Which compositions exist, and which fields does each give? | A page takes existing compositions and does not repeat their fields |
| Is there an SEO composition? | Every page takes it; if there is none, it joins this change ([SEO is mandatory](#seo-is-mandatory)) |
| Does the page need blocks or a block area, and do they exist? | Missing ones join this change through their peer skills ([combined-changeset.md](combined-changeset.md)) |
| Is there a master template, and what is its alias? | The new template goes under it; if there is none, one joins the change |
| Which data types exist for the page's own fields? | Reuse `Textstring`, `Richtext editor` or a project type before anything new |

## 2. Decide

Work through this table in order. Each row names the rule to open; apply it rather than
restating it.

| Decision | For a page | Rule |
|---|---|---|
| Kind | Root, Content or Programmatic. Listing is handed off | [page-kinds.md](page-kinds.md) |
| Schema | Name and alias (Title Case name ending `Page`, camelCase alias ending `Page`; the user's noun is kept), folder (the Document Types root), icon, description, own properties, vary by culture | [umbraco-add-document-type](../../umbraco-add-document-type/references/workflow.md#2-decide), role *page* |
| Compositions | The SEO composition always; others chosen per kind | [SEO is mandatory](#seo-is-mandatory), [compositions.md](../../umbraco-content-model-conventions/references/compositions.md) |
| Blocks and block area | Only when the page has a block area; each missing part through its peer | [combined-changeset.md](combined-changeset.md) |
| Template | Named after the type, under the master | [templates.md](../../umbraco-content-model-conventions/references/templates.md) |
| Allowed at root | `No`, unless the new type is the home page | [allowed-children-and-root.md](../../umbraco-content-model-conventions/references/allowed-children-and-root.md#allowed-at-root) |
| Parent and allowed children | Per kind; the parent is updated in the same change | [allowed-children-and-root.md](../../umbraco-content-model-conventions/references/allowed-children-and-root.md#allowed-children) |

### SEO is mandatory

Every page takes the site's SEO composition, programmatic pages included: a search results page
still needs a meta title and description.

- **The site has one** (`seoComposition` or the project's equivalent): the page lists it in its
  Compositions row, flagged `Exists`.
- **The site has none:** the SEO composition joins this changeset, under
  [umbraco-add-composition](../../umbraco-add-composition/SKILL.md)'s rules: the SEO row of
  [standard-compositions.md](../../umbraco-add-composition/references/standard-compositions.md#the-table),
  its [data types](../../umbraco-add-composition/references/standard-compositions.md#data-types)
  (`Toggle (default on)` for `isIndexable`, only if the site lacks one), and a `Compositions`
  folder and index page if they are missing. The page lists it flagged `New in this changeset`,
  never `Missing`. Say in the summary that the composition is new and shared.
- **Other pages on the site lack SEO:** list them in the changeset summary and offer to apply the
  composition to them as Update pages. Do not include them unless the user says so; the clash
  check in [when-to-apply.md](../../umbraco-add-composition/references/when-to-apply.md#what-blocks-a-type-from-taking-it)
  applies to each.

Sharing and page-details compositions stay selective, as
[when-to-apply.md](../../umbraco-add-composition/references/when-to-apply.md) says. If the page
would want one the site lacks, leave it out, name it in the summary as a follow-up for
umbraco-add-composition, and do not flag it `Missing`: that would block approval of a page that is
useful without it.

### Own properties

The page's own fields follow
[umbraco-add-document-type](../../umbraco-add-document-type/references/workflow.md#own-properties):
only what no composition gives, on the `Content` tab, reusing data types. A page with a block area
holds it as one property (usually `mainContent`) whose data type is that area's Block Grid.

### The root question

- **No home page, and the new type is not one.** Do not allow the new page at root to make it
  creatable. Say the site has no home page and ask whether to add one in the same change. If the
  user says no, the page is specified with no parent and the summary says editors cannot create it
  until something allows it.
- **Other page types are already allowed at root.** Report which, and why that departs from the
  convention. Change them only if the user confirms, and then as `Update` pages in the changeset.
- **The new type is the home page.** It is allowed at root and allows the site's existing content,
  listing and programmatic pages. Ask about moving anything else that is at root, as
  [page-kinds.md](page-kinds.md#root) says.

## 3. Write the requirements doc

The core page is these pages; anything else the page needs (a new SEO composition, blocks, a
block area) adds its pages as [combined-changeset.md](combined-changeset.md) lists:

| Page | From | Location | Action |
|---|---|---|---|
| The SEO composition, only if the site has none | [composition.md](../../umbraco-content-requirements-documentation/assets/composition.md) | `Document-Types/Compositions/SeoComposition.md` | Create |
| Any new data type for an own field | [data-type.md](../../umbraco-content-requirements-documentation/assets/data-type.md) | `Data-Types/<Editor-Kind>/<Name-Slug>.md` | Create |
| The master template, only if the site has none | [template.md](../../umbraco-content-requirements-documentation/assets/template.md) | `Templates/Master.md` | Create |
| Its template | [template.md](../../umbraco-content-requirements-documentation/assets/template.md) | `Templates/<MasterPascalCase>/<AliasPascalCase>.md` | Create |
| The page type | [document-type.md](../../umbraco-content-requirements-documentation/assets/document-type.md) | `Document-Types/<AliasPascalCase>.md` | Create |
| The parent, with the new type added to Allowed children | [document-type.md](../../umbraco-content-requirements-documentation/assets/document-type.md) | `Document-Types/HomePage.md` | Update |
| The changeset | [changeset.md](../../umbraco-content-requirements-documentation/assets/changeset.md) | `_changesets/<yyyy-mm-dd>-<slug>.md` | — |

Points specific to a page:

- Delete the `Collection` row; it belongs to listing pages only.
- The Compositions row always includes the SEO composition.
- A content page lists itself in its own Allowed children, as a link to its own page. It does not
  list itself in Dependencies.
- **The parent** is an existing type the change edits. Its page is written as
  [requirements-lifecycle.md](../../umbraco-content-requirements-documentation/references/requirements-lifecycle.md#updating-a-type-that-already-exists)
  describes.

The changeset's checklist for the core page, in apply order:

```
- [ ] 1. Create template `master`                                   (only if new)
- [ ] 2. Create template `<alias>` under `master`
- [ ] 3. Create document type `<alias>`, then fix-up (sorts, mandatory, descriptions, template, culture)
- [ ] 4. Set allowed children on `<alias>`: `<alias>`                (content page only)
- [ ] 5. Set allowed children on `homePage`: add `<alias>`, keeping the existing entries
- [ ] 6. Verify every requirements doc against the site and set each status line
```

A new data type, a new SEO composition, blocks and a block area come before row 1, in the order
[combined-changeset.md](combined-changeset.md#apply-order) gives.

## 4. Stop for approval

For a page, call out the **existing** parent type that will change: it has content on it. Call out
too any existing Block Grid that gains a block, and any new shared artefact (an SEO composition, a
settings model) that other types will reuse. Then stop; see
[the approval gate](../../umbraco-content-requirements-documentation/references/change-workflow.md#4-stop-for-approval).
If the user asked for the backoffice steps and the MCP is not connected, see
[apply-manually.md](../../umbraco-content-requirements-documentation/references/apply-manually.md#asked-for-the-steps-before-approval).

## 5. Apply

For a page, also:

- **Template content.** Create the page template with only what makes it a page under the master:

  ```cshtml
  @inherits Umbraco.Cms.Web.Common.Views.UmbracoViewPage
  @{
      Layout = "master.cshtml";
  }
  ```

  The non-generic base class compiles before Models Builder has generated the page's model. The
  markup, and a controller for a programmatic page, are implementation work outside this change.
  A new master uses `Layout = null;` and renders `@RenderBody()` inside a minimal HTML document.
- **The create.** Leave `parentId` out, because a page sits at the tree root.
- **The type's own allowed children.** A content page allows itself, which needs its own id, so
  it is set in the fix-up, not in the create.
- **The parent.** Read the home page with `get-document-type-by-id`, append the new type to
  `allowedDocumentTypes` after the entries it already has, and send the whole body back with
  `update-document-type`. Read it again and confirm the property count is unchanged and the
  allowed children grew by exactly one.

## 6. Verify and report

For a page, also confirm on the read-back that the default and allowed templates point at the
new template, that the template is a child of the master, that the SEO composition is applied,
and that the parent lists the new type.

In the report, what is left (item 3) names: optional compositions deferred to other skills, other
pages that still lack SEO, the template's markup, block partial views, and for a programmatic page
the controller that renders it.
