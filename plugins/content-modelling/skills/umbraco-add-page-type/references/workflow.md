# Workflow

Follow
[the six steps](../../umbraco-content-requirements-documentation/references/change-workflow.md).
This file adds only what a page type needs at each step.

## 1. Inspect

For a page type, these are the answers you need before deciding anything:

| Question | Why it matters here |
|---|---|
| Does a type with this name or alias already exist? | Do not create a second one; offer to extend the first |
| Is there a home page, and what is allowed at root? | Decides the parent, and whether the root question below must be asked |
| What does the home page allow now? | The parent update keeps every existing entry |
| Which compositions exist, and which fields does each give? | A page takes existing compositions and does not repeat their fields |
| Is there a master template, and what is its alias? | The new template goes under it; if there is none, one joins the change |
| Which data types exist for the page's own fields? | Reuse `Textstring`, `Richtext editor` or a project type before anything new |

## 2. Decide

Work through this table in order. Each row names the rule to open; apply it rather than
restating it.

| Decision | For a page type | Rule |
|---|---|---|
| Kind | Root, Content or Programmatic. Listing is handed off | [page-kinds.md](page-kinds.md) |
| Name and alias | Title Case name ending `Page`, camelCase alias ending `Page`. The user's noun is kept; the suffix is not optional | [naming.md](../../umbraco-content-model-conventions/references/naming.md) |
| Folder | The Document Types root, never a folder | [tree-organisation.md](../../umbraco-content-model-conventions/references/tree-organisation.md) |
| Icon | A noun that fits, page colour | [icons-and-colours.md](../../umbraco-content-model-conventions/references/icons-and-colours.md) |
| Description | What the page is for | [descriptions.md](../../umbraco-content-model-conventions/references/descriptions.md) |
| Compositions | Existing ones only, chosen per kind | [compositions.md](../../umbraco-content-model-conventions/references/compositions.md) |
| Own properties | Only what no composition gives, on the `Content` tab | [tabs-groups-sorts.md](../../umbraco-content-model-conventions/references/tabs-groups-sorts.md), [property-aliases.md](../../umbraco-content-model-conventions/references/property-aliases.md) |
| Template | Named after the type, under the master | [templates.md](../../umbraco-content-model-conventions/references/templates.md) |
| Allowed at root | `No`, unless the new type is the home page | [allowed-children-and-root.md](../../umbraco-content-model-conventions/references/allowed-children-and-root.md#allowed-at-root) |
| Parent and allowed children | Per kind; the parent is updated in the same change | [allowed-children-and-root.md](../../umbraco-content-model-conventions/references/allowed-children-and-root.md#allowed-children) |
| Vary by culture | `No` on a single-language site; otherwise ask | — |

### Compositions that do not exist yet

Creating a composition is not part of this skill. If the page needs one the site lacks (SEO
fields, sharing fields), leave it out of this requirements doc, say so in the changeset summary,
and name [umbraco-add-composition](../../umbraco-add-composition/SKILL.md) as the follow-up that
adds it and then applies it to this page. Do not list it as a `Missing` dependency: that blocks
approval of a page that is useful without it.

### Own properties

Give the page a field only when no composition already supplies it, and reuse an existing data
type whenever its configuration fits. When a field genuinely needs a configuration no data type on
the site has, the new data type joins the changeset as its own requirements page, named and
foldered by [naming.md](../../umbraco-content-model-conventions/references/naming.md#data-types);
block editors are configured separately, by
[umbraco-configure-block-editor](../../umbraco-configure-block-editor/SKILL.md). A page with no own
properties writes the one line the requirements doc format gives for that case.

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

A page type change is these pages:

| Page | From | Location | Action |
|---|---|---|---|
| Any new data type for an own field | [data-type.md](../../umbraco-content-requirements-documentation/assets/data-type.md) | `Data-Types/<Editor-Kind>/<Name-Slug>.md` | Create |
| The master template, only if the site has none | [template.md](../../umbraco-content-requirements-documentation/assets/template.md) | `Templates/Master.md` | Create |
| Its template | [template.md](../../umbraco-content-requirements-documentation/assets/template.md) | `Templates/<MasterPascalCase>/<AliasPascalCase>.md` | Create |
| The page type | [document-type.md](../../umbraco-content-requirements-documentation/assets/document-type.md) | `Document-Types/<AliasPascalCase>.md` | Create |
| The parent, with the new type added to Allowed children | [document-type.md](../../umbraco-content-requirements-documentation/assets/document-type.md) | `Document-Types/HomePage.md` | Update |
| The changeset | [changeset.md](../../umbraco-content-requirements-documentation/assets/changeset.md) | `_changesets/<yyyy-mm-dd>-<slug>.md` | — |

Points specific to a page type:

- Delete the `Collection` row; it belongs to listing pages only.
- A content page lists itself in its own Allowed children, as a link to its own page. It does not
  list itself in Dependencies.
- **The parent** is an existing type the change edits. Its page is written as
  [requirements-lifecycle.md](../../umbraco-content-requirements-documentation/references/requirements-lifecycle.md#updating-a-type-that-already-exists)
  describes.

The changeset's checklist for a page type, in apply order:

```
- [ ] 1. Create template `master`                                   (only if new)
- [ ] 2. Create template `<alias>` under `master`
- [ ] 3. Create document type `<alias>`, then fix-up (sorts, mandatory, descriptions, template, culture)
- [ ] 4. Set allowed children on `<alias>`: `<alias>`                (content page only)
- [ ] 5. Set allowed children on `homePage`: add `<alias>`, keeping the existing entries
- [ ] 6. Verify every requirements doc against the site and set each status line
```

A new data type, if there is one, comes first, as
[apply-via-mcp.md](../../umbraco-content-requirements-documentation/references/apply-via-mcp.md#order)
orders.

## 4. Stop for approval

For a page type, call out the **existing** parent type that will change: it has content on it.
Then stop; see
[the approval gate](../../umbraco-content-requirements-documentation/references/change-workflow.md#4-stop-for-approval).
If the user asked for the backoffice steps and the MCP is not connected, see
[apply-manually.md](../../umbraco-content-requirements-documentation/references/apply-manually.md#asked-for-the-steps-before-approval).

## 5. Apply

For a page type, also:

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

For a page type, also confirm on the read-back that the default and allowed templates point at the
new template, that the template is a child of the master, and that the parent lists the new type.

In the report, what is left (item 3) names: compositions or data types deferred to other skills,
the template's markup, and for a programmatic page the controller that renders it.
