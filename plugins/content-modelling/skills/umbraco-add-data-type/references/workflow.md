# Workflow

Inspect, decide, write the spec, **stop for approval**, apply, verify. Four of those six steps
are the spec skill's; this file adds only what a data type needs at each one. Read the linked file
at each step rather than working from this summary.

Before step 1, find or agree the spec folder as
[spec-lifecycle.md](../../umbraco-content-requirements-documentation/references/spec-lifecycle.md#where-specs-live)
describes. Ask once; never again once it is recorded.

## 1. Inspect

Follow [inspect-existing-schema.md](../../umbraco-content-requirements-documentation/references/inspect-existing-schema.md)
for the tools and for what to do without the MCP. For a data type, these are the answers you need:

| Question | How | Why it matters here |
|---|---|---|
| Which data types use the editor you need? | `get-all-data-types` or `find-data-type`, then `get-data-type` on each candidate | Reuse needs the same configuration, not only the same editor |
| What uses a candidate? | `get-references-data-type` | Changing a shared data type changes all of these |
| Which editor-kind folders exist? | `get-data-type-root`, `get-data-type-children` | Create the folder only if it is missing |
| Which property will use it, and does it hold content now? | `get-document-type-by-id` on the type | Moving a property with content to a different value format loses it |
| Does the project follow a different convention? | The tree as it is | The project's convention wins; note the departure |

Without the MCP, ask these in one message, and treat every answer as the user's word, not as
something read from the site.

## 2. Decide

| Decision | Reference |
|---|---|
| Reuse, generic variant, or purpose-specific | [reuse-or-create.md](reuse-or-create.md) |
| Property editor, editor UI and value type | [editor-selection.md](editor-selection.md) |
| Name and folder | [naming-and-folders.md](naming-and-folders.md) |
| Configuration: every setting that differs from the editor's defaults, and every constraint | [reuse-or-create.md](reuse-or-create.md#constraints-go-in-the-data-type) |

If the answer is "reuse", there may be nothing to write: say which data type to use and why, and
stop there unless a property is being changed.

## 3. Write the spec

Follow [spec-format.md](../../umbraco-content-requirements-documentation/references/spec-format.md).

| Page | From | Location | Action |
|---|---|---|---|
| The data type | [data-type.md](../../umbraco-content-requirements-documentation/assets/data-type.md) | `Data-Types/<Editor kind>/<Name>.md` | Create, or Update for a changed one |
| The type whose property uses it, when a property is added or moved | [document-type.md](../../umbraco-content-requirements-documentation/assets/document-type.md) or [element-type.md](../../umbraco-content-requirements-documentation/assets/element-type.md) | Where the type lives | Update |
| The changeset | [changeset.md](../../umbraco-content-requirements-documentation/assets/changeset.md) | `_changesets/<yyyy-mm-dd>-<slug>.md` | — |

Points specific to a data type:

- **Configuration** states each setting in words an editor can check in the backoffice (a sentence
  such as "Maximum characters: 160."), plus the setting aliases in the table when they are known
  from a data type read on the site. Do not invent aliases; copy them from `get-data-type`.
- **Database type** is the storage the editor uses (`Nvarchar`, `Ntext`, `Integer`, `Decimal`,
  `Date`). Copy it from a data type of the same editor on the site; write `—` if you cannot read it.
- **Used by** lists every property that will use the data type after this change. For an Update,
  it also lists every existing user, so the approver sees what else changes.
- A type that gains the property follows the same rule as the other add-* skills: an existing spec
  page is edited and set back to `proposed`; with the MCP, a page is written from the read-back;
  without it, the change is a checklist item and the summary says the type is the user's word. If the read-back does not give every row the page needs, treat the type as you would without the MCP; never write a partial page or fill cells with placeholders such as "Unchanged".

The changeset's checklist, in apply order:

```
- [ ] 1. Create data type folder `<Editor kind>`                            (only if new)
- [ ] 2. Create data type `<Name>` in `<Editor kind>`                        (or: Update data type `<Name>`)
- [ ] 3. Set property `<alias>` on `<type alias>` to use `<Name>`            (only if a property changes)
- [ ] 4. Verify every spec against the site and set each status line
```

Then lint the folder with the spec skill's
[`lint-spec.mjs`](../../umbraco-content-requirements-documentation/scripts/lint-spec.mjs) until it reports no
errors. If Node.js is not available, say the spec was not linted.

## 4. Stop for approval

Follow [the approval gate](../../umbraco-content-requirements-documentation/references/spec-lifecycle.md#the-approval-gate).
Show the changeset's summary and spec list, and call out every existing property that the change
touches, any constraint that existing content may now break, and anything that rests on the user's
word. Then stop.

Without the MCP, if the user asked for the backoffice steps, give the walkthrough in the same reply,
headed as steps to follow once the changeset is approved. Writing it is not applying it, and the
status stays `proposed`.

## 5. Apply

With the MCP, follow [apply-via-mcp.md](../../umbraco-content-requirements-documentation/references/apply-via-mcp.md).
Without it, follow [apply-manually.md](../../umbraco-content-requirements-documentation/references/apply-manually.md).
For a data type, also:

- **The folder.** `create-data-type-folder` returns no id. Pass your own new UUID as `id`, then
  confirm it with `get-data-type-folder`.
- **The create.** `create-data-type` takes `name`, `editorAlias`, `editorUiAlias`, `parentId` (the
  folder) and `values`. Build `values` by reading a data type of the same editor with
  `get-data-type` and changing only the settings the spec names; do not guess the aliases.
- **An update.** Read the data type, change only the named settings, write the whole configuration
  back with `update-data-type`, and read it again.
- **A property change.** Read the type, point the property's `dataType` at the new id, send the
  whole body back with `update-document-type`, and confirm the property count is unchanged.

## 6. Verify and report

Follow [verify.md](../../umbraco-content-requirements-documentation/references/verify.md). For a data type, compare
`name`, `editorAlias`, `editorUiAlias` and every configuration value in the spec, and check the
folder with `get-data-type-ancestors`.

Report, separately:

1. What was created or changed, by name, and which properties now use it.
2. What was **read back and matched**, and what was only confirmed by the user or not checked.
3. What is left: anything in the Apply log, existing content that may fail the new constraint, any
   content migration a value-format change would need, and the Razor or C# that reads the value.
