# Workflow

Inspect, decide, write the spec, **stop for approval**, apply, verify. Four of those six steps
are the spec skill's; this file adds only what shared data needs at each one. Read the linked file
at each step rather than working from this summary.

Before step 1, find or agree the spec folder as
[spec-lifecycle.md](../../umbraco-content-model-spec/references/spec-lifecycle.md#where-specs-live)
describes. Ask once; never again once it is recorded.

## 1. Inspect

Follow [inspect-existing-schema.md](../../umbraco-content-model-spec/references/inspect-existing-schema.md).
For shared data, establish:

| Question | Why it matters here |
|---|---|
| Is there already a folder or item type for this data? | Extend it instead of making a second |
| Does a `Data/` folder exist in Document Types? | If not, it joins the change first |
| What is allowed at root? | The data folder joins the home page and site settings there |
| Are the values stored somewhere already (a text field on pages, tags)? | Moving existing values into items is a content migration |
| Which page types will pick the items, and is there a composition they share? | Where the picker property goes |
| Is there a `Content Picker/` data type folder, or existing pickers? | Reuse an identical picker; otherwise a new one |

Without the MCP, ask these in one message, and treat every answer as the user's word.

## 2. Decide

| Decision | For shared data | Rule |
|---|---|---|
| Names and aliases | Folder `<noun>Folder`; item the plain singular noun (`author`), or `<noun>Item` when the noun is not a real-world thing (`reusableContentItem`) | [naming.md](../../umbraco-content-model-conventions/references/naming.md) |
| Folder in Document Types | `Data/`, both types | [tree-organisation.md](../../umbraco-content-model-conventions/references/tree-organisation.md) |
| Icons | Both `color-green`: `icon-folder color-green` for the folder, a noun for the item | [icons-and-colours.md](../../umbraco-content-model-conventions/references/icons-and-colours.md) |
| Descriptions | What it holds ("Holds the authors that articles can pick from.") | [descriptions.md](../../umbraco-content-model-conventions/references/descriptions.md) |
| Allowed at root | Folder Yes, item No | [allowed-children-and-root.md](../../umbraco-content-model-conventions/references/allowed-children-and-root.md) |
| Allowed children | Folder: the item, and itself only if editors need sub-folders. Item: nothing | [allowed-children-and-root.md](../../umbraco-content-model-conventions/references/allowed-children-and-root.md) |
| Templates and compositions | None on either. No SEO, sharing or page details | [templates.md](../../umbraco-content-model-conventions/references/templates.md), [compositions.md](../../umbraco-content-model-conventions/references/compositions.md) |
| Item fields | What each item holds beyond its name, on the `Content` tab (100), sorts in hundreds. Links (a website, a map, a booking page) are a Multi URL Picker with a maximum of 1, never a Textstring holding a URL; pages are picked, not typed | [tabs-groups-sorts.md](../../umbraco-content-model-conventions/references/tabs-groups-sorts.md), [property-aliases.md](../../umbraco-content-model-conventions/references/property-aliases.md), [editor-selection.md](../../umbraco-add-data-type/references/editor-selection.md#choices-that-come-up-often) |
| Folder fields | Usually none | — |
| The picker | A multinode tree picker restricted to the folder and the item type, with the maximum the use needs | [taxonomy.md](taxonomy.md#the-shape-of-a-content-driven-taxonomy) |
| Where the picker property goes | On the composition the consuming types share, or the one type that needs it | [taxonomy.md](taxonomy.md#where-the-picker-goes) |

## 3. Write the spec

Follow [spec-format.md](../../umbraco-content-model-spec/references/spec-format.md). Shared data is
these pages:

| Page | From | Location | Action |
|---|---|---|---|
| The item type | [document-type.md](../../umbraco-content-model-spec/assets/document-type.md) | `Document-Types/Data/<ItemAlias>.md` | Create |
| The folder type | [document-type.md](../../umbraco-content-model-spec/assets/document-type.md) | `Document-Types/Data/<FolderAlias>.md` | Create |
| The picker data type | [data-type.md](../../umbraco-content-model-spec/assets/data-type.md) | `Data-Types/Content-Picker/<Name>.md` | Create |
| Each type or composition that gains the picker | [document-type.md](../../umbraco-content-model-spec/assets/document-type.md) or [composition.md](../../umbraco-content-model-spec/assets/composition.md) | Where it lives | Update |
| The changeset | [changeset.md](../../umbraco-content-model-spec/assets/changeset.md) | `_changesets/<yyyy-mm-dd>-<slug>.md` | — |

Points specific to shared data:

- Both pages write `—` for Default template, Allowed templates and Compositions; the linter
  rejects a template on a type in `Data/`. Delete the `List view` row.
- The picker's Configuration names the start node as the data folder **content node** the user
  creates (by name), the allowed item type and the maximum. The start node is content, not
  schema: note in the summary that the folder node must exist before the picker is configured.
- Types that gain the picker follow the usual rule: an existing spec page is edited and set back to
  `proposed`; with the MCP a page is written from the read-back; without it the change is a
  checklist item and the summary says the type is the user's word. If the read-back does not give every row the page needs, treat the type as you would without the MCP; never write a partial page or fill cells with placeholders such as "Unchanged".

The changeset's checklist, in apply order:

```
- [ ] 1. Create document type folder `Data`                                     (only if new)
- [ ] 2. Create document type `<itemAlias>` in `Data`, then fix-up
- [ ] 3. Create document type `<folderAlias>` in `Data`, then fix-up (allowed at root)
- [ ] 4. Set allowed children on `<folderAlias>`: `<itemAlias>`
- [ ] 5. Create the `<Folder name>` content node at the content root             (content, by the user)
- [ ] 6. Create data type folder `Content Picker` and data type `<Picker name>`
- [ ] 7. Add property `<alias>` to `<type or composition alias>`, keeping everything else   (one per consumer)
- [ ] 8. Verify every spec against the site and set each status line
```

Step 5 is content, not schema, and is carried out by the user in the Content section. It is listed
so the checklist order is right, and the verification does not depend on it.

Then lint the folder with the spec skill's
[`lint-spec.mjs`](../../umbraco-content-model-spec/scripts/lint-spec.mjs) until it reports no
errors. If Node.js is not available, say the spec was not linted.

## 4. Stop for approval

Follow [the approval gate](../../umbraco-content-model-spec/references/spec-lifecycle.md#the-approval-gate).
Show the changeset's summary and spec list, and call out every existing type that gains a picker,
any values already stored elsewhere that would need migrating into items, and anything that rests
on the user's word. Then stop.

Without the MCP, if the user asked for the backoffice steps, give the walkthrough in the same reply,
headed as steps to follow once the changeset is approved. Writing it is not applying it, and the
status stays `proposed`.

## 5. Apply

With the MCP, follow [apply-via-mcp.md](../../umbraco-content-model-spec/references/apply-via-mcp.md),
including the fix-up pass after each create. Without it, follow
[apply-manually.md](../../umbraco-content-model-spec/references/apply-manually.md). For shared data,
also:

- **Both types in `Data/`.** `create-document-type` takes the `Data` folder's id as `parentId`;
  look it up at apply time.
- **The item before the folder**, so the folder's allowed children can name it.
- **The picker after the folder node exists.** The start node is a content node id; ask the user to
  create the folder node, then look up its id. Copy the picker's `values` shape from an existing
  multinode tree picker read with `get-data-type`.
- **Each consumer** by read-modify-write, confirming the property count grew by exactly one.

## 6. Verify and report

Follow [verify.md](../../umbraco-content-model-spec/references/verify.md). For shared data, also
confirm that the folder is allowed at root and allows the item, that neither type has a template,
and that the picker's start node and allowed type are the ones in the spec.

Report, separately:

1. What was created or changed, by name.
2. What was **read back and matched**, and what was only confirmed by the user or not checked.
3. What is left: creating the folder content node and its items, migrating any values stored
   elsewhere, the Razor or C# that reads the picked items, and anything in the Apply log.
