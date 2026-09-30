# Workflow

Inspect, decide, write the spec, **stop for approval**, apply, verify. Four of those six steps
are the spec skill's; this file adds only what a page type needs at each one. Read the linked
file at each step rather than working from this summary.

Before step 1, find or agree the spec folder as
[spec-lifecycle.md](../../umbraco-content-model-spec/references/spec-lifecycle.md#where-specs-live)
describes. Ask once; never again once it is recorded.

## 1. Inspect

Follow [inspect-existing-schema.md](../../umbraco-content-model-spec/references/inspect-existing-schema.md)
for the tools and for what to do without the MCP. For a page type, these are the answers you need
before deciding anything:

| Question | Why it matters here |
|---|---|
| Does a type with this name or alias already exist? | Do not create a second one; offer to extend the first |
| Is there a home page, and what is allowed at root? | Decides the parent, and whether the root question below must be asked |
| What does the home page allow now? | The parent update keeps every existing entry |
| Which compositions exist, and which fields does each give? | A page takes existing compositions and does not repeat their fields |
| Is there a master template, and what is its alias? | The new template goes under it; if there is none, one joins the change |
| Which data types exist for the page's own fields? | Reuse `Textstring`, `Richtext editor` or a project type before anything new |
| How many languages? | Vary by culture is asked about only when there is more than one |
| Does the project follow a different convention? | The project's convention wins; note the departure |

Aliases come only from `get-document-type-by-id`; `get-all-document-types` does not return them.
Without the MCP, ask these in one message, and treat every answer as the user's word, not as
something read from the site.

## 2. Decide

Work through this table in order. Each row names the rule to open; apply it rather than
restating it.

| Decision | For a page type | Rule |
|---|---|---|
| Kind | Root, Content or Programmatic. Listing is handed off | [page-kinds.md](page-kinds.md) |
| Name and alias | Title Case name ending `Page`; camelCase alias ending `Page`, acronyms collapsed. The user's noun is kept, the suffix is not optional | [naming.md](../../umbraco-content-model-conventions/references/naming.md) |
| Folder | The Document Types root, never a folder | [tree-organisation.md](../../umbraco-content-model-conventions/references/tree-organisation.md) |
| Icon | A noun that fits, with `color-light-blue` | [icons-and-colours.md](../../umbraco-content-model-conventions/references/icons-and-colours.md) |
| Description | One sentence saying what the page is for | [descriptions.md](../../umbraco-content-model-conventions/references/descriptions.md) |
| Compositions | Existing ones only, chosen per kind | [compositions.md](../../umbraco-content-model-conventions/references/compositions.md) |
| Own properties | Only what no composition gives; `Content` tab; sorts in hundreds | [tabs-groups-sorts.md](../../umbraco-content-model-conventions/references/tabs-groups-sorts.md), [property-aliases.md](../../umbraco-content-model-conventions/references/property-aliases.md) |
| Template | Named after the type, file `<alias>.cshtml`, under the master | [templates.md](../../umbraco-content-model-conventions/references/templates.md) |
| Allowed at root | Yes for the home page only | [allowed-children-and-root.md](../../umbraco-content-model-conventions/references/allowed-children-and-root.md) |
| Parent and allowed children | Per kind; the parent is updated in the same change | [allowed-children-and-root.md](../../umbraco-content-model-conventions/references/allowed-children-and-root.md) |
| Vary by culture | `No` on a single-language site. Ask only when there are two or more languages | — |

### Compositions that do not exist yet

Creating a composition is not part of this skill. If the page needs one the site lacks (SEO
fields, sharing fields), leave it out of this spec, say so in the changeset summary, and name
[umbraco-add-composition](../../umbraco-add-composition/SKILL.md) as the follow-up that adds it and then applies it to this page. Do not
list it as a `Missing` dependency: that blocks approval of a page that is useful without it.

### Own properties

Give the page a field only when no composition already supplies it. Put it on the `Content` tab
(sort 100) with property sorts 100, 200, 300. Reuse an existing data type whenever its
configuration fits. When a field genuinely needs a configuration no data type on the site has,
the new data type joins the changeset as its own spec page, named and foldered by
[naming.md](../../umbraco-content-model-conventions/references/naming.md#data-types); block
editors are configured separately, by umbraco-configure-block-editor. A page with no own properties
writes the one line the spec format gives for that case.

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

## 3. Write the spec

Follow [spec-format.md](../../umbraco-content-model-spec/references/spec-format.md). A page type
change is these pages:

| Page | From | Location | Action |
|---|---|---|---|
| The page type | [document-type.md](../../umbraco-content-model-spec/assets/document-type.md) | `Document-Types/<Alias>.md` | Create |
| Its template | [template.md](../../umbraco-content-model-spec/assets/template.md) | `Templates/<Master>/<Alias>.md` | Create |
| The master template, only if the site has none | [template.md](../../umbraco-content-model-spec/assets/template.md) | `Templates/Master.md` | Create |
| The parent, with the new type added to Allowed children | [document-type.md](../../umbraco-content-model-spec/assets/document-type.md) | `Document-Types/HomePage.md` | Update |
| The changeset | [changeset.md](../../umbraco-content-model-spec/assets/changeset.md) | `_changesets/<yyyy-mm-dd>-<slug>.md` | — |

Points specific to a page type:

- Delete the `List view` row; it belongs to listing pages only.
- A content page lists itself in its own Allowed children, as a link to its own page. It does not
  list itself in Dependencies.
- Existing compositions, the master and built-in data types that have no spec page are written as
  plain text and flagged `Exists` in Dependencies.
- The `Folder` breadcrumb links to the `Document-Types.md` index page. Write it from
  [folder-index.md](../../umbraco-content-model-spec/assets/folder-index.md) if the folder does not
  have one yet.
- **The parent.** If it already has a spec page, edit its Allowed children, set it back to
  `proposed` and list it with the action `Update`. If it has none and the MCP is connected, write
  its page from the `get-document-type-by-id` read-back, complete, and list it as `Update`.
  Without the MCP, do not reconstruct a page you cannot read: name the parent as plain text flagged
  `Exists`, make the change a checklist item, and say in the summary that the parent is the user's
  word.

The changeset's checklist for a page type, in apply order:

```
- [ ] 1. Create template `master`                                   (only if new)
- [ ] 2. Create template `<alias>` under `master`
- [ ] 3. Create document type `<alias>`, then fix-up (sorts, mandatory, descriptions, template, culture)
- [ ] 4. Set allowed children on `<alias>`: `<alias>`                (content page only)
- [ ] 5. Set allowed children on `homePage`: add `<alias>`, keeping the existing entries
- [ ] 6. Verify every spec against the site and set each status line
```

A new data type, if there is one, comes first, as
[apply-via-mcp.md](../../umbraco-content-model-spec/references/apply-via-mcp.md#order) orders.

Then lint the folder with the spec skill's
[`lint-spec.mjs`](../../umbraco-content-model-spec/scripts/lint-spec.mjs) until it reports no
errors. If Node.js is not available, say the spec was not linted.

## 4. Stop for approval

Follow [the approval gate](../../umbraco-content-model-spec/references/spec-lifecycle.md#the-approval-gate).
Show the changeset's summary and spec list, and call out two things: the **existing** parent type
that will change, and anything that rests on the user's word rather than a read of the site. Then
stop. A request to "just create it", however firm, is a request for the page type; it is not
approval of a spec the user has not seen.

Without the MCP, if the user asked for the backoffice steps, give the walkthrough in the same reply,
headed as steps to follow once the changeset is approved. Writing it is not applying it, and the
status stays `proposed`.

## 5. Apply

With the MCP, follow [apply-via-mcp.md](../../umbraco-content-model-spec/references/apply-via-mcp.md),
including the fix-up pass after the create. Without it, follow
[apply-manually.md](../../umbraco-content-model-spec/references/apply-manually.md). For a page
type, also:

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
- **The create.** Look up the ids of the existing compositions and data types at apply time, by
  name or alias; never take one from the spec. Leave `parentId` out, because a page sits at the
  tree root.
- **The type's own allowed children.** A content page allows itself, which needs its own id, so
  it is set in the fix-up, not in the create.
- **The parent.** Read the home page with `get-document-type-by-id`, append the new type to
  `allowedDocumentTypes` after the entries it already has, and send the whole body back with
  `update-document-type`. This type has content on it, and a body that leaves out a property
  deletes that property. Read it again and confirm the property count is unchanged and the allowed
  children grew by exactly one.

## 6. Verify and report

Follow [verify.md](../../umbraco-content-model-spec/references/verify.md). For a page type, also
confirm on the read-back that the default and allowed templates point at the new template, that
the template is a child of the master, and that the parent lists the new type.

Report, separately:

1. What was created or changed, by name: the page type, its template, the parent.
2. What was **read back and matched**, and what was only confirmed by the user or not checked.
   The icon colour is always unchecked.
3. What is left: anything in the Apply log, compositions or data types deferred to other skills,
   the template's markup, and for a programmatic page the controller that renders it.
