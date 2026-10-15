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
| Where will the block be used? | A block area the user names is updated in this change; otherwise it is named in the summary as the follow-up |
| If a block area is named, what does its Block Grid register now? | The Update keeps every existing entry ([approach A](../../umbraco-configure-block-editor/references/approach-a-block-grid-placement.md#adding-a-block-to-an-existing-placement)) |

## 2. Decide

Each element of the block is decided by
[umbraco-add-element-type](../../umbraco-add-element-type/references/workflow.md#2-decide) for its
role: name and alias, folder, container, fields, icon, description and vary by culture. This table
adds only what makes them a block:

| Decision | For a block | Rule |
|---|---|---|
| Content element | The block's name: a plain noun, `...Block` only on a clash; content fields only | [element-roles.md](../../umbraco-add-element-type/references/element-roles.md#content-element) |
| Style options | None (no settings element; settings are optional), a shared settings model, or a bespoke one | [settings-models.md](settings-models.md) |
| Repeated fields | A child element and its Block List | [nested-items.md](nested-items.md) |
| Compositions on the content element | None. Page compositions never go on elements | [compositions.md](../../umbraco-content-model-conventions/references/compositions.md) |
| Registration | In the block area the user names, with its settings model; otherwise a follow-up | [Registering in a named block area](#registering-in-a-named-block-area) |
| Vary by culture | The settings model and its settings compositions vary with the content element they serve | [element-roles.md](../../umbraco-add-element-type/references/element-roles.md) |

### Registering in a named block area

When the user names an existing block area ("add it to the main content grid"), the block is
registered there in the same changeset. Its Block Grid becomes an **Update** page that adds one
row, the content element with its settings model (or `—` when it has none), and keeps every
existing row, as
[approach A](../../umbraco-configure-block-editor/references/approach-a-block-grid-placement.md#adding-a-block-to-an-existing-placement)
describes; its spans and group follow the blocks already there. A block area that does not exist
yet is not created here: name it as the follow-up for
[umbraco-configure-block-editor](../../umbraco-configure-block-editor/SKILL.md).

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
| The named block area's Block Grid, as an Update | [block-data-type.md](../../umbraco-content-requirements-documentation/assets/block-data-type.md) | `Data-Types/Block-Grid/<Name-Slug>.md` |
| The changeset | [changeset.md](../../umbraco-content-requirements-documentation/assets/changeset.md) | `_changesets/<yyyy-mm-dd>-<slug>.md` |

Points specific to a block:

- A **settings composition** page has tabs (`Style` 50 or `Settings` 100) and no groups, unlike
  the element template's default rows. A **settings model** writes the "No own properties" line
  and lists its settings compositions in the Compositions row.
- The content element's `Used by` names the Block Grid it is registered in. If no block area was
  named, write `—` and name the placement in the changeset summary as the follow-up for
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
- [ ] 8. Update data type `<Block Grid>`: add `<alias>` with `<settings model or —>`, keeping the existing entries   (named block area only)
- [ ] 9. Verify every requirements doc against the site and set each status line
```

When another skill called this one, hand back rows 1-8; that skill orders them in its own
changeset.

## 4. Stop for approval

Call out which parts are shared (a settings model or settings composition other blocks will
reuse) and, when a block area is named, that its Block Grid is an existing data type that will
change; then stop; see
[the approval gate](../../umbraco-content-requirements-documentation/references/change-workflow.md#4-stop-for-approval).
If the user asked for the backoffice steps without the MCP, see
[apply-manually.md](../../umbraco-content-requirements-documentation/references/apply-manually.md#asked-for-the-steps-before-approval).

## 5. Apply

For a block, also:

- **Each element** is created, moved into its folder and fixed up as
  [umbraco-add-element-type](../../umbraco-add-element-type/references/workflow.md#5-apply)
  describes.
- **The Block List.** Create it with `create-data-type` after the child element exists, copying
  the shape of its `values` from an existing Block List on the site read with `get-data-type`.
  The block entry references the child element's id; the amount holds the minimum and maximum.
- **The parent.** Its `items` property takes the Block List's id, so it is created after the
  Block List.
- **The registration.** Read the Block Grid with `get-data-type`, append one block entry with the
  content element's and settings model's ids, and send the whole configuration back with
  `update-data-type`, every existing entry included. It goes last, because it needs both ids.

## 6. Verify and report

For a block, also confirm on the read-back that each element sits in the right folder, has no
tab and exactly one `Content` group, that the settings model has no own properties, and that the
Block List registers the child with the specified amount. When a block area was named, read its
Block Grid back and confirm it has exactly one more entry, with the right settings model, and
every earlier entry unchanged.

In the report, what is left is: registering the block (and its settings model) in a block area
that was not named, with [umbraco-configure-block-editor](../../umbraco-configure-block-editor/SKILL.md),
anything in the Apply log, and the block's Razor partial view, which is implementation work.
