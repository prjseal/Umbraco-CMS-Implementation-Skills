# Workflow

Inspect, decide, write the spec, **stop for approval**, apply, verify. Four of those six steps
are the spec skill's; this file adds only what a block needs at each one. Read the linked file at
each step rather than working from this summary.

Before step 1, find or agree the spec folder as
[spec-lifecycle.md](../../umbraco-content-model-spec/references/spec-lifecycle.md#where-specs-live)
describes. Ask once; never again once it is recorded.

## 1. Inspect

Follow [inspect-existing-schema.md](../../umbraco-content-model-spec/references/inspect-existing-schema.md)
for the tools and for what to do without the MCP. For a block, these are the answers you need
before deciding anything:

| Question | Why it matters here |
|---|---|
| Is there already an element for this block? | Do not create a second one; offer to extend the first |
| Does the noun clash with a media type, data type or reserved word? | Only then does the alias take the `...Block` suffix |
| Do `Elements/`, `Elements/Settings/` and `Elements/Compositions/` exist? | Missing folders join the change first |
| Which settings models and settings compositions exist, and what do they offer? | Reuse a shared settings model before creating one |
| Which data types exist for the fields (text, rich text, media, link, colour)? | Reuse before creating |
| Where will the block be used? | Not changed here, but named in the summary for umbraco-configure-block-editor |
| Does the project follow a different convention? | The project's convention wins; note the departure |

Aliases come only from `get-document-type-by-id`. Without the MCP, ask these in one message, and
treat every answer as the user's word, not as something read from the site.

## 2. Decide

| Decision | For a block | Rule |
|---|---|---|
| Name and alias | Title Case name; camelCase plain noun, `...Block` only on a clash | [naming.md](../../umbraco-content-model-conventions/references/naming.md) |
| Folder | `Elements/` | [tree-organisation.md](../../umbraco-content-model-conventions/references/tree-organisation.md) |
| Container | No tab; one group `Content`, sort 0 | [tabs-groups-sorts.md](../../umbraco-content-model-conventions/references/tabs-groups-sorts.md) |
| Fields | Content only; short generic aliases (`title`, `image`, `link`); sorts 100, 200, 300 | [property-aliases.md](../../umbraco-content-model-conventions/references/property-aliases.md) |
| Icon | A noun that fits (`icon-list`, `icon-picture`), no colour | [icons-and-colours.md](../../umbraco-content-model-conventions/references/icons-and-colours.md) |
| Description | What the block shows | [descriptions.md](../../umbraco-content-model-conventions/references/descriptions.md) |
| Style options | None, a shared settings model, or a bespoke one | [settings-models.md](settings-models.md) |
| Repeated fields | A child element and its Block List | [nested-items.md](nested-items.md) |
| Compositions on the content element | None. Page compositions never go on elements | [compositions.md](../../umbraco-content-model-conventions/references/compositions.md) |

Element types have no allowed children, are never allowed at root, have no template and do not
vary by culture at the type level.

## 3. Write the spec

Follow [spec-format.md](../../umbraco-content-model-spec/references/spec-format.md). A block is
these pages, as needed, in this order:

| Page | From | Location |
|---|---|---|
| A new data type for a field, only if needed | [data-type.md](../../umbraco-content-model-spec/assets/data-type.md) | `Data-Types/<Editor kind>/<Name>.md` |
| Each new settings composition | [element-type.md](../../umbraco-content-model-spec/assets/element-type.md) | `Document-Types/Elements/Compositions/<Alias>.md` |
| A new settings model | [element-type.md](../../umbraco-content-model-spec/assets/element-type.md) | `Document-Types/Elements/Settings/<Alias>.md` |
| The child element of a repeater | [element-type.md](../../umbraco-content-model-spec/assets/element-type.md) | `Document-Types/Elements/<Alias>.md` |
| The repeater's Block List | [block-data-type.md](../../umbraco-content-model-spec/assets/block-data-type.md) | `Data-Types/Block-List/<Name>.md` |
| The content element | [element-type.md](../../umbraco-content-model-spec/assets/element-type.md) | `Document-Types/Elements/<Alias>.md` |
| The changeset | [changeset.md](../../umbraco-content-model-spec/assets/changeset.md) | `_changesets/<yyyy-mm-dd>-<slug>.md` |

Points specific to a block:

- A **settings composition** page has tabs (`Style` 50 or `Settings` 100) and no groups, unlike the
  element template's default rows. A **settings model** writes the "No own properties" line and
  lists its settings compositions in the Compositions row.
- The content element's `Used by` says where it will be registered; if that is not part of this
  change, write `—` and name the placement in the changeset summary as the follow-up for
  umbraco-configure-block-editor. Say there which settings model belongs with the block.
- Folders are listed in Dependencies as `document-type-container` or `data-type-container`, and
  the `Folder` breadcrumbs link to index pages. Write missing index pages from
  [folder-index.md](../../umbraco-content-model-spec/assets/folder-index.md).
- Existing data types, settings models and settings compositions with no spec page are plain text
  flagged `Exists`.

The changeset's checklist, in apply order:

```
- [ ] 1. Create data type folder `<Editor kind>` and data type `<Name>`                  (only if new)
- [ ] 2. Create document type folders `Elements`, `Elements/Compositions`, `Elements/Settings`   (only those missing)
- [ ] 3. Create settings composition `<alias>`, move it into `Elements/Compositions`, then fix-up   (each new one)
- [ ] 4. Create settings model `<alias>`, move it into `Elements/Settings`, then fix-up      (only if new)
- [ ] 5. Create element `<child alias>`, move it into `Elements`, then fix-up              (repeater only)
- [ ] 6. Create data type folder `Block List` and data type `<Plural of child>`             (repeater only)
- [ ] 7. Create element `<alias>`, move it into `Elements`, then fix-up
- [ ] 8. Verify every spec against the site and set each status line
```

Then lint the folder with the spec skill's
[`lint-spec.mjs`](../../umbraco-content-model-spec/scripts/lint-spec.mjs) until it reports no
errors. If Node.js is not available, say the spec was not linted.

## 4. Stop for approval

Follow [the approval gate](../../umbraco-content-model-spec/references/spec-lifecycle.md#the-approval-gate).
Show the changeset's summary and spec list, and call out which parts are shared (a settings model
or settings composition other blocks will reuse) and anything that rests on the user's word. Then
stop.

Without the MCP, if the user asked for the backoffice steps, give the walkthrough in the same reply,
headed as steps to follow once the changeset is approved. Writing it is not applying it, and the
status stays `proposed`.

## 5. Apply

With the MCP, follow [apply-via-mcp.md](../../umbraco-content-model-spec/references/apply-via-mcp.md),
including the fix-up pass after every create. Without it, follow
[apply-manually.md](../../umbraco-content-model-spec/references/apply-manually.md). For a block,
also:

- **Create, then move.** `create-element-type` has no folder parameter, so each element is created
  at the tree root. Move it with `move-document-type` into its folder straight away, look up the
  folder id at apply time, and confirm the move with `get-document-type-ancestors`.
- **The container.** Pass `group: "Content"` and no tab for content elements; the fix-up sets the
  group's sort to 0. Settings compositions pass their tab (`Style` or `Settings`) and no group.
- **Compositions on a settings model.** Pass the settings compositions' ids in `compositions` on
  the create; the model has no properties of its own.
- **The Block List.** Create it with `create-data-type` after the child element exists, copying the
  shape of its `values` from an existing Block List on the site read with `get-data-type`. The
  block entry references the child element's id; the amount holds the minimum and maximum.
- **The parent.** Its `items` property takes the Block List's id, so it is created last.

## 6. Verify and report

Follow [verify.md](../../umbraco-content-model-spec/references/verify.md). For a block, also
confirm on the read-back that each element sits in the right folder, has no tab and exactly one
`Content` group, that the settings model has no own properties, and that the Block List registers
the child with the specified amount.

Report, separately:

1. What was created, by name: each element, settings model, settings composition and data type.
2. What was **read back and matched**, and what was only confirmed by the user or not checked.
3. What is left: registering the block (and its settings model) in a page's Block Grid or Block
   List with umbraco-configure-block-editor, anything in the Apply log, and the block's Razor
   partial view, which is implementation work.
