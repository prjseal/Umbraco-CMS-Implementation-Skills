# Workflow

Inspect, decide, write the spec, **stop for approval**, apply, verify. Four of those six steps
are the spec skill's; this file adds only what a composition needs at each one. Read the linked
file at each step rather than working from this summary.

Before step 1, find or agree the spec folder as
[spec-lifecycle.md](../../umbraco-content-requirements-documentation/references/spec-lifecycle.md#where-specs-live)
describes. Ask once; never again once it is recorded.

## 1. Inspect

Follow [inspect-existing-schema.md](../../umbraco-content-requirements-documentation/references/inspect-existing-schema.md)
for the tools and for what to do without the MCP. For a composition, these are the answers you
need before deciding anything:

| Question | Why it matters here |
|---|---|
| Is there already a composition for this concern? | Reuse or extend it instead of making a second |
| Does a `Compositions` folder exist? | If not, the folder joins the change first |
| Which compositions already put fields in the tab this one will use? | A shared tab means groups, and an Update of the existing composition |
| Which types would take it, and what do they define now? | Every target is an Update; an alias clash blocks a target |
| Which data types exist for the fields? | Reuse before creating |
| How many languages? | Vary by culture is asked about only when there is more than one |
| Does the project follow a different convention? | The project's convention wins; note the departure |

Read every candidate target with `get-document-type-by-id`, and every composition it already has,
so the alias comparison in [when-to-apply.md](when-to-apply.md#what-blocks-a-type-from-taking-it)
is made against the real schema. `get-document-type-composition-references` tells you what already
uses an existing composition. Without the MCP, ask these in one message, and treat every answer as
the user's word, not as something read from the site.

## 2. Decide

| Decision | For a composition | Rule |
|---|---|---|
| The concern | One job. Two jobs are two compositions | [compositions.md](../../umbraco-content-model-conventions/references/compositions.md) |
| Name and alias | Title Case name ending `Composition`; camelCase alias ending `Composition`, acronyms collapsed (`seoComposition`) | [naming.md](../../umbraco-content-model-conventions/references/naming.md) |
| Folder | `Compositions/` | [tree-organisation.md](../../umbraco-content-model-conventions/references/tree-organisation.md) |
| Icon | `icon-settings` or a noun that fits, no colour | [icons-and-colours.md](../../umbraco-content-model-conventions/references/icons-and-colours.md) |
| Description | "Adds ..." | [descriptions.md](../../umbraco-content-model-conventions/references/descriptions.md) |
| Tab, group and sorts | The global tab sort; a group only when the tab is shared; property sorts in hundreds | [standard-compositions.md](standard-compositions.md), [tabs-groups-sorts.md](../../umbraco-content-model-conventions/references/tabs-groups-sorts.md) |
| Fields and data types | Short list, area-prefixed aliases, existing data types first | [standard-compositions.md](standard-compositions.md) |
| Behaviour | No template, no children, not allowed at root, no compositions of its own | [compositions.md](../../umbraco-content-model-conventions/references/compositions.md) |
| Targets | Selective, by type kind; clashes excluded | [when-to-apply.md](when-to-apply.md) |
| Vary by culture | Match the targets. `No` on a single-language site | — |

## 3. Write the spec

Follow [spec-format.md](../../umbraco-content-requirements-documentation/references/spec-format.md). A composition
change is these pages:

| Page | From | Location | Action |
|---|---|---|---|
| A new data type, only if a field needs one | [data-type.md](../../umbraco-content-requirements-documentation/assets/data-type.md) | `Data-Types/<Editor kind>/<Name>.md` | Create |
| The composition | [composition.md](../../umbraco-content-requirements-documentation/assets/composition.md) | `Document-Types/Compositions/<Alias>.md` | Create |
| An existing composition that now shares the tab and gains a group | [composition.md](../../umbraco-content-requirements-documentation/assets/composition.md) | `Document-Types/Compositions/<Alias>.md` | Update |
| Each target type, with the composition added to its Compositions row | [document-type.md](../../umbraco-content-requirements-documentation/assets/document-type.md) | Where the type lives | Update |
| The changeset | [changeset.md](../../umbraco-content-requirements-documentation/assets/changeset.md) | `_changesets/<yyyy-mm-dd>-<slug>.md` | — |

Points specific to a composition:

- `Used by` lists every target by link (or plain text when it has no spec page).
- The `Compositions` folder is listed in Dependencies as a `document-type-container`, flagged
  `New in this changeset` if it is created now, and the `Folder` breadcrumb links to the index
  pages. Write missing index pages from
  [folder-index.md](../../umbraco-content-requirements-documentation/assets/folder-index.md).
- **The targets.** If a target already has a spec page, add the composition to its Compositions
  row, set it back to `proposed` and list it with the action `Update`. If it has none and the MCP
  is connected, write its page from the `get-document-type-by-id` read-back, complete, and list it
  as `Update`. If the read-back does not give every row the page needs, treat the type as you would without the MCP; never write a partial page or fill cells with placeholders such as "Unchanged". Without the MCP, do not reconstruct pages you cannot read: name each target as plain
  text, make each one a checklist item, and say in the summary that the list of targets and their
  current fields are the user's word.
- Types left out because of a clash are named in the summary with the reason.

The changeset's checklist, in apply order:

```
- [ ] 1. Create data type folder `<Editor kind>` and data type `<Name>`   (only if new)
- [ ] 2. Create document type folder `Compositions`                     (only if new)
- [ ] 3. Create composition `<alias>`, then fix-up (sorts, groups, mandatory, descriptions)
- [ ] 4. Update composition `<existing alias>`: move its fields into group `<Group>`   (only if the tab is now shared)
- [ ] 5. Add `<alias>` to the compositions of `<target alias>`, keeping the existing ones   (one item per target)
- [ ] 6. Verify every spec against the site and set each status line
```

Then lint the folder with the spec skill's
[`lint-spec.mjs`](../../umbraco-content-requirements-documentation/scripts/lint-spec.mjs) until it reports no
errors. If Node.js is not available, say the spec was not linted.

## 4. Stop for approval

Follow [the approval gate](../../umbraco-content-requirements-documentation/references/spec-lifecycle.md#the-approval-gate).
Show the changeset's summary and spec list, and call out: every **existing** type that will change,
any type left out and why, anything that would need a content migration, and anything that rests
on the user's word. Then stop. "Just add it to every page" is a request for the composition, not
approval of a list of types the user has not seen.

Without the MCP, if the user asked for the backoffice steps, give the walkthrough in the same reply,
headed as steps to follow once the changeset is approved. Writing it is not applying it, and the
status stays `proposed`.

## 5. Apply

With the MCP, follow [apply-via-mcp.md](../../umbraco-content-requirements-documentation/references/apply-via-mcp.md),
including the fix-up pass after the create. Without it, follow
[apply-manually.md](../../umbraco-content-requirements-documentation/references/apply-manually.md). For a
composition, also:

- **The create.** Pass `parentId` as the `Compositions` folder's id, looked up at apply time.
  Each property carries its `tab` and, when the tab is shared, its `group`. Sorts, mandatory and
  descriptions are written in the fix-up.
- **The tab name.** `SEO & Sharing` is refused by the MCP; follow
  [Values the MCP rejects](../../umbraco-content-requirements-documentation/references/apply-via-mcp.md#values-the-mcp-rejects)
  and leave the spec as written.
- **An existing composition gaining a group.** Read it, add a group container whose parent is the
  tab's container, point each of its properties at the new group, and send the whole body back.
  Take the container shape from a type on the site that already has a group. Its property count
  must not change.
- **Each target.** Read it with `get-document-type-by-id`, append the composition to its
  `compositions` after the entries it already has, copying the shape of those entries, and send
  the whole body back with `update-document-type`. Read it again: its own property count is
  unchanged and the composition is listed. Do the targets one at a time, ticking each, so a failure
  leaves a clear record of which types have it.

## 6. Verify and report

Follow [verify.md](../../umbraco-content-requirements-documentation/references/verify.md). For a composition, also
confirm that `get-document-type-composition-references` on the new composition lists exactly the
targets in the spec, and that each target's own properties are the ones it had before.

Report, separately:

1. What was created or changed, by name: the composition, any data type, each target.
2. What was **read back and matched**, and what was only confirmed by the user or not checked.
3. What is left: anything in the Apply log (such as the tab name the MCP refused), types left out
   and why, any field move that needs a content migration, and the Razor or C# that reads the new
   fields, which is implementation work.
