# Workflow

Follow [the six steps](../../umbraco-content-requirements-documentation/references/change-workflow.md).
This file adds only what one element type needs at each step.

## 1. Inspect

For an element, these are the answers you need before deciding anything:

| Question | Why it matters here |
|---|---|
| Is there already an element for this? | Do not create a second one; offer to extend the first |
| Does the noun clash with a media type, data type or reserved word? | Only then does a content element take the `...Block` suffix |
| Do `Elements/`, `Elements/Settings/` and `Elements/Compositions/` exist? | Missing folders join the change first |
| Which data types exist for the fields (text, rich text, media, link, colour)? | Reuse before creating |
| On a multi-language site, do the existing elements vary by culture? | A new element matches the elements beside it; see step 2 |
| Where will it be used? | Not changed here, but named in the summary as the follow-up |

## 2. Decide

| Decision | For an element | Rule |
|---|---|---|
| Role | Content element, repeater item, settings model or settings composition | [element-roles.md](element-roles.md) |
| Name and alias | Title Case name; camelCase alias with the role's suffix | [naming.md](../../umbraco-content-model-conventions/references/naming.md) |
| Folder | By role, under `Elements/` | [tree-organisation.md](../../umbraco-content-model-conventions/references/tree-organisation.md) |
| Container | By role: one `Content` group, sort 0, or a `Style`/`Settings` tab | [tabs-groups-sorts.md](../../umbraco-content-model-conventions/references/tabs-groups-sorts.md#which-container-to-use) |
| Fields | Content only on content elements and items; short generic aliases; sorts in hundreds | [property-aliases.md](../../umbraco-content-model-conventions/references/property-aliases.md) |
| Icon | A noun that fits, no colour | [icons-and-colours.md](../../umbraco-content-model-conventions/references/icons-and-colours.md) |
| Description | What the element shows, or "Adds ..." for a settings composition | [descriptions.md](../../umbraco-content-model-conventions/references/descriptions.md) |
| Compositions | Settings compositions on a settings model only; nothing else composes | [compositions.md](../../umbraco-content-model-conventions/references/compositions.md#blocks-settings-compositions-and-settings-models) |
| Vary by culture | `No` on a single-language site; `Yes` when blocks must be translated, which needs the type and its properties to vary | [requirements-format.md](../../umbraco-content-requirements-documentation/references/requirements-format.md#properties), Culture paragraph |

Element types have no allowed children, are never allowed at root and have no template. A block
can vary by language only when its element type (and each property on it) does, so a settings
model and its settings compositions vary with the content element they serve.

When a field genuinely needs a configuration no data type on the site has, the new data type joins
the changeset as its own requirements page; reuse or create is decided by
[umbraco-add-data-type](../../umbraco-add-data-type/SKILL.md).

## 3. Write the requirements doc

| Page | From | Location |
|---|---|---|
| A new data type for a field, only if needed | [data-type.md](../../umbraco-content-requirements-documentation/assets/data-type.md) | `Data-Types/<Editor-Kind>/<Name-Slug>.md` |
| The element | [element-type.md](../../umbraco-content-requirements-documentation/assets/element-type.md) | `Document-Types/Elements/<AliasPascalCase>.md`, or `Elements/Settings/` or `Elements/Compositions/` by role |
| The changeset (standalone only) | [changeset.md](../../umbraco-content-requirements-documentation/assets/changeset.md) | `_changesets/<yyyy-mm-dd>-<slug>.md` |

The element's `Used by` says where it will be used; if that is not part of this change, write `—`
and name the follow-up in the changeset summary.

The checklist rows for one element, in apply order:

```
- [ ] 1. Create data type folder `<Editor kind>` and data type `<Name>`             (only if new)
- [ ] 2. Create document type folder `<Elements, Elements/Settings or Elements/Compositions>`   (only if missing)
- [ ] 3. Create element `<alias>`, move it into `<folder>`, then fix-up
- [ ] 4. Verify every requirements doc against the site and set each status line   (standalone only)
```

When another skill called this one, hand back rows 1-3 and that skill orders them in its own
changeset.

## 4. Stop for approval

Standalone, call out anything shared (a settings composition other settings models will compose),
then stop; see
[the approval gate](../../umbraco-content-requirements-documentation/references/change-workflow.md#4-stop-for-approval).
If the user asked for the backoffice steps without the MCP, see
[apply-manually.md](../../umbraco-content-requirements-documentation/references/apply-manually.md#asked-for-the-steps-before-approval).

## 5. Apply

- **Create, then move, then fix-up.** Every element is created at the tree root
  ([apply-via-mcp.md](../../umbraco-content-requirements-documentation/references/apply-via-mcp.md#tools)).
  Move it with `move-document-type` into its folder straight away, looking up the folder id at
  apply time, and confirm the move with `get-document-type-ancestors`.
- **The container.** Pass `group: "Content"` and no tab for content elements and items; the fix-up
  sets the group's sort to 0. Settings compositions pass their tab (`Style` or `Settings`) and no
  group.
- **Compositions on a settings model.** Pass the settings compositions' ids in `compositions` on
  the create; the model has no properties of its own.
- **A Block List field** on a parent element takes the Block List's id, so the parent is created
  after its Block List; see [nested-items.md](../../umbraco-add-block/references/nested-items.md#order).

## 6. Verify and report

Confirm on the read-back that the element sits in its folder; that a content element or item has
no tab and exactly one `Content` group; that a settings model has no own properties; and that a
settings composition has its tab and no group.

In the report, what is left is: where the element is used (registering it with
[umbraco-configure-block-editor](../../umbraco-configure-block-editor/SKILL.md), or composing a
settings composition into settings models), anything in the Apply log, and the element's Razor
partial view, which is implementation work.
