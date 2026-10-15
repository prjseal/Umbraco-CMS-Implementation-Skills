# Workflow

Follow [the six steps](../../umbraco-content-requirements-documentation/references/change-workflow.md).
This file adds only what the schema of one document type needs at each step. A skill that calls
this one adds its own behaviour on top: a page's template and parent, a listing's collection, a
data folder's picker.

## 1. Inspect

For a document type, these are the answers you need before deciding anything:

| Question | Why it matters here |
|---|---|
| Does a type with this name or alias already exist? | Do not create a second one; offer to extend the first |
| Which compositions exist, and which fields does each give? | The type takes existing compositions and does not repeat their fields |
| Does a property alias on the type clash with one a chosen composition gives? | A clash blocks the composition; see [when-to-apply.md](../../umbraco-add-composition/references/when-to-apply.md#what-blocks-a-type-from-taking-it) |
| Does the folder for its role (`Data/`, `Compositions/`) exist? | A missing folder joins the change first |
| Which data types exist for its fields? | Reuse `Textstring`, `Richtext editor` or a project type before anything new |

## 2. Decide

Work through this table in order. Each row names the rule to open; apply it rather than restating
it.

| Decision | For a document type | Rule |
|---|---|---|
| Role | Page, composition, data, site settings or plain; a caller names it | [type-roles.md](type-roles.md) |
| Name and alias | Title Case name; camelCase alias with the role's suffix. The user's noun is kept | [naming.md](../../umbraco-content-model-conventions/references/naming.md) |
| Folder | By role | [tree-organisation.md](../../umbraco-content-model-conventions/references/tree-organisation.md) |
| Icon | A noun that fits, coloured by role | [icons-and-colours.md](../../umbraco-content-model-conventions/references/icons-and-colours.md) |
| Description | What the type is for | [descriptions.md](../../umbraco-content-model-conventions/references/descriptions.md) |
| Compositions | Existing ones only; flat; no alias clash | [compositions.md](../../umbraco-content-model-conventions/references/compositions.md) |
| Own properties | Only what no composition gives, on the `Content` tab | [tabs-groups-sorts.md](../../umbraco-content-model-conventions/references/tabs-groups-sorts.md), [property-aliases.md](../../umbraco-content-model-conventions/references/property-aliases.md) |
| Vary by culture | `No` on a single-language site; otherwise ask | — |

### Own properties

Give the type a field only when no composition already supplies it, and reuse an existing data
type whenever its configuration fits. When a field genuinely needs a configuration no data type on
the site has, the new data type joins the changeset as its own requirements page, named and
foldered by [naming.md](../../umbraco-content-model-conventions/references/naming.md#data-types);
reuse or create is decided by [umbraco-add-data-type](../../umbraco-add-data-type/SKILL.md).
Block editors are configured separately, by
[umbraco-configure-block-editor](../../umbraco-configure-block-editor/SKILL.md). A type with no own
properties writes the one line the requirements doc format gives for that case; a page made only of
compositions is normal.

## 3. Write the requirements doc

| Page | From | Location | Action |
|---|---|---|---|
| Any new data type for an own field | [data-type.md](../../umbraco-content-requirements-documentation/assets/data-type.md) | `Data-Types/<Editor-Kind>/<Name-Slug>.md` | Create |
| The document type | [document-type.md](../../umbraco-content-requirements-documentation/assets/document-type.md) | `Document-Types/<AliasPascalCase>.md` for a page, or under its role's folder | Create |
| The changeset (standalone only) | [changeset.md](../../umbraco-content-requirements-documentation/assets/changeset.md) | `_changesets/<yyyy-mm-dd>-<slug>.md` | — |

Points specific to the schema:

- Delete the `Collection` row unless the caller is a listing.
- The behaviour rows (Allowed at root, templates, Allowed children) are the caller's. Standalone,
  fill them as [type-roles.md](type-roles.md#a-plain-types-behaviour-rows) says.
- A `Folder` breadcrumb links to its folder index page; write a missing index from
  [folder-index.md](../../umbraco-content-requirements-documentation/assets/folder-index.md).

The checklist rows for the schema, in apply order:

```
- [ ] 1. Create data type folder `<Editor kind>` and data type `<Name>`             (only if new)
- [ ] 2. Create document type folder `<Data or Compositions>`                      (only if missing)
- [ ] 3. Create document type `<alias>` in `<folder>`, then fix-up (sorts, mandatory, descriptions, culture)
- [ ] 4. Verify every requirements doc against the site and set each status line   (standalone only)
```

When another skill called this one, hand back rows 1-3; that skill adds its own rows (a template
before row 3, allowed children after it) and orders them in its changeset.

## 4. Stop for approval

Standalone, call out that nothing can create the type in the content tree until a parent allows
it, if that is so; then stop. See
[the approval gate](../../umbraco-content-requirements-documentation/references/change-workflow.md#4-stop-for-approval).
If the user asked for the backoffice steps and the MCP is not connected, see
[apply-manually.md](../../umbraco-content-requirements-documentation/references/apply-manually.md#asked-for-the-steps-before-approval).

## 5. Apply

- **The create.** Use `create-document-type`. A page leaves `parentId` out, because it sits at the
  tree root; a type in `Data/` or `Compositions/` passes the folder's id, looked up at apply time.
- **The fix-up.** Run the fix-up pass from
  [apply-via-mcp.md](../../umbraco-content-requirements-documentation/references/apply-via-mcp.md#the-fix-up-pass)
  for sorts, mandatory flags, descriptions and culture.
- **Compositions.** Pass the compositions' ids in `compositions` on the create.

## 6. Verify and report

Confirm on the read-back that the type sits in the folder its role says, that it has no template,
that every composition is applied, and that the property count matches the requirements page.

In the report, what is left names: what still has to allow the type as a child, data types or
compositions deferred to other skills, and anything in the Apply log.
