# Workflow

Follow [the six steps](../../umbraco-content-requirements-documentation/references/change-workflow.md).
This file adds only what a block needs at each step.

## 1. Inspect

For a block, these are the answers you need before deciding anything:

| Question | Why it matters here |
|---|---|
| Is there already an element for this block? | Do not create a second one; offer to extend the first |
| Does the noun clash with a media type, data type or reserved word? | Only then does the alias take the `...Block` suffix |
| Do `Elements/`, `Elements/Settings/` and `Elements/Compositions/` exist? | Missing folders join the change first |
| Which settings models and settings compositions exist, and what do they offer? | Reuse a shared settings model before creating one |
| Which data types exist for the fields (text, rich text, media, link, colour)? | Reuse before creating |
| On a multi-language site, do the existing elements vary by culture? | A new block matches the blocks beside it; see step 2 |
| Where will the block be used? | Not changed here, but named in the summary for [umbraco-configure-block-editor](../../umbraco-configure-block-editor/SKILL.md) |

## 2. Decide

| Decision | For a block | Rule |
|---|---|---|
| Name and alias | Title Case name; camelCase plain noun, `...Block` only on a clash | [naming.md](../../umbraco-content-model-conventions/references/naming.md) |
| Folder | `Elements/` | [tree-organisation.md](../../umbraco-content-model-conventions/references/tree-organisation.md) |
| Container | No tab; one group `Content`, sort 0 | [tabs-groups-sorts.md](../../umbraco-content-model-conventions/references/tabs-groups-sorts.md#which-container-to-use) |
| Fields | Content only; short generic aliases; sorts in hundreds | [property-aliases.md](../../umbraco-content-model-conventions/references/property-aliases.md) |
| Icon | A noun that fits, no colour | [icons-and-colours.md](../../umbraco-content-model-conventions/references/icons-and-colours.md) |
| Description | What the block shows | [descriptions.md](../../umbraco-content-model-conventions/references/descriptions.md) |
| Style options | None, a shared settings model, or a bespoke one | [settings-models.md](settings-models.md) |
| Repeated fields | A child element and its Block List | [nested-items.md](nested-items.md) |
| Compositions on the content element | None. Page compositions never go on elements | [compositions.md](../../umbraco-content-model-conventions/references/compositions.md) |
| Vary by culture | `No` on a single-language site; `Yes` when blocks must be translated, which needs the type and its properties to vary | [requirements-format.md](../../umbraco-content-requirements-documentation/references/requirements-format.md#properties), Culture paragraph |

Element types have no allowed children, are never allowed at root and have no template. They do
vary by culture when the site's blocks are translated: a block can vary by language only when its
element type (and each property on it) does, so a settings model and its settings compositions
vary with the content element they serve.

## 3. Write the requirements doc

A block is these pages, as needed, in this order:

| Page | From | Location |
|---|---|---|
| A new data type for a field, only if needed | [data-type.md](../../umbraco-content-requirements-documentation/assets/data-type.md) | `Data-Types/<Editor-Kind>/<Name-Slug>.md` |
| Each new settings composition | [element-type.md](../../umbraco-content-requirements-documentation/assets/element-type.md) | `Document-Types/Elements/Compositions/<AliasPascalCase>.md` |
| A new settings model | [element-type.md](../../umbraco-content-requirements-documentation/assets/element-type.md) | `Document-Types/Elements/Settings/<AliasPascalCase>.md` |
| The child element of a repeater | [element-type.md](../../umbraco-content-requirements-documentation/assets/element-type.md) | `Document-Types/Elements/<AliasPascalCase>.md` |
| The repeater's Block List | [block-data-type.md](../../umbraco-content-requirements-documentation/assets/block-data-type.md) | `Data-Types/Block-List/<Name-Slug>.md` |
| The content element | [element-type.md](../../umbraco-content-requirements-documentation/assets/element-type.md) | `Document-Types/Elements/<AliasPascalCase>.md` |
| The changeset | [changeset.md](../../umbraco-content-requirements-documentation/assets/changeset.md) | `_changesets/<yyyy-mm-dd>-<slug>.md` |

Points specific to a block:

- A **settings composition** page has tabs (`Style` 50 or `Settings` 100) and no groups, unlike
  the element template's default rows. A **settings model** writes the "No own properties" line
  and lists its settings compositions in the Compositions row.
- The content element's `Used by` says where it will be registered; if that is not part of this
  change, write `—` and name the placement in the changeset summary as the follow-up for
  [umbraco-configure-block-editor](../../umbraco-configure-block-editor/SKILL.md). Say there
  which settings model belongs with the block.
- An existing settings model that gains a settings composition, or an existing element this
  change edits, is an **Update** page; see
  [requirements-lifecycle.md](../../umbraco-content-requirements-documentation/references/requirements-lifecycle.md#updating-a-type-that-already-exists).

The changeset's checklist, in apply order:

```
- [ ] 1. Create data type folder `<Editor kind>` and data type `<Name>`                  (only if new)
- [ ] 2. Create document type folders `Elements`, `Elements/Compositions`, `Elements/Settings`   (only those missing)
- [ ] 3. Create settings composition `<alias>`, move it into `Elements/Compositions`, then fix-up   (each new one)
- [ ] 4. Create settings model `<alias>`, move it into `Elements/Settings`, then fix-up      (only if new)
- [ ] 5. Create element `<child alias>`, move it into `Elements`, then fix-up              (repeater only)
- [ ] 6. Create data type folder `Block List` and data type `<Plural of child>`             (repeater only)
- [ ] 7. Create element `<alias>`, move it into `Elements`, then fix-up
- [ ] 8. Verify every requirements doc against the site and set each status line
```

## 4. Stop for approval

Call out which parts are shared (a settings model or settings composition other blocks will
reuse), then stop; see
[the approval gate](../../umbraco-content-requirements-documentation/references/change-workflow.md#4-stop-for-approval).
If the user asked for the backoffice steps without the MCP, see
[apply-manually.md](../../umbraco-content-requirements-documentation/references/apply-manually.md#asked-for-the-steps-before-approval).

## 5. Apply

For a block, also:

- **Create, then move, then fix-up.** Every element is created at the tree root
  ([apply-via-mcp.md](../../umbraco-content-requirements-documentation/references/apply-via-mcp.md#tools)).
  Move it with `move-document-type` into `Elements/`, `Elements/Settings/` or
  `Elements/Compositions/` straight away, looking up the folder id at apply time, and confirm the
  move with `get-document-type-ancestors`.
- **The container.** Pass `group: "Content"` and no tab for content elements; the fix-up sets the
  group's sort to 0. Settings compositions pass their tab (`Style` or `Settings`) and no group.
- **Compositions on a settings model.** Pass the settings compositions' ids in `compositions` on
  the create; the model has no properties of its own.
- **The Block List.** Create it with `create-data-type` after the child element exists, copying
  the shape of its `values` from an existing Block List on the site read with `get-data-type`.
  The block entry references the child element's id; the amount holds the minimum and maximum.
- **The parent.** Its `items` property takes the Block List's id, so it is created last.

## 6. Verify and report

For a block, also confirm on the read-back that each element sits in the right folder, has no
tab and exactly one `Content` group, that the settings model has no own properties, and that the
Block List registers the child with the specified amount.

In the report, what is left is: registering the block (and its settings model) in a page's Block
Grid or Block List with [umbraco-configure-block-editor](../../umbraco-configure-block-editor/SKILL.md),
anything in the Apply log, and the block's Razor partial view, which is implementation work.
