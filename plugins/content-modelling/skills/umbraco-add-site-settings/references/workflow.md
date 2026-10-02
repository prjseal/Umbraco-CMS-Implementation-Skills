# Workflow

Inspect, decide, write the spec, **stop for approval**, apply, verify. Four of those six steps
are the spec skill's; this file adds only what the settings singleton needs at each one. Read the
linked file at each step rather than working from this summary.

Before step 1, find or agree the spec folder as
[spec-lifecycle.md](../../umbraco-content-requirements-documentation/references/spec-lifecycle.md#where-specs-live)
describes. Ask once; never again once it is recorded.

## 1. Inspect

Follow [inspect-existing-schema.md](../../umbraco-content-requirements-documentation/references/inspect-existing-schema.md).
For site settings, establish:

| Question | Why it matters here |
|---|---|
| Is there already a `siteSettings` type, or settings on another type? | Extend the existing one; never create a second |
| Does the home page carry settings fields (a settings tab, a settings composition)? | Moving them loses values unless migrated |
| What is allowed at root? | Site settings joins the home page and data folders there |
| Is it a multi-site install? | One settings node per site; how code finds the right one is agreed with the developer |
| How many languages? | Settings such as footer text may need to vary by culture |
| Which data types and elements exist for the fields (pickers, repeaters)? | Reuse before creating |

Without the MCP, ask these in one message, and treat every answer as the user's word.

## 2. Decide

| Decision | For site settings | Rule |
|---|---|---|
| Name and alias | Site Settings, `siteSettings`: the one root type without a `Page` suffix | [naming.md](../../umbraco-content-model-conventions/references/naming.md) |
| Folder | The Document Types root, beside the pages | [tree-organisation.md](../../umbraco-content-model-conventions/references/tree-organisation.md) |
| Icon | `icon-settings`, coloured like data (`color-green`) because editors meet it in the content tree beside data folders; follow the project if it colours it differently | [icons-and-colours.md](../../umbraco-content-model-conventions/references/icons-and-colours.md) |
| Description | What it holds ("Settings for the whole site: logo, navigation, footer and defaults.") | [descriptions.md](../../umbraco-content-model-conventions/references/descriptions.md) |
| Allowed at root | Yes | [allowed-children-and-root.md](../../umbraco-content-model-conventions/references/allowed-children-and-root.md#site-settings) |
| Allowed children, templates, compositions | None | [allowed-children-and-root.md](../../umbraco-content-model-conventions/references/allowed-children-and-root.md#site-settings), [templates.md](../../umbraco-content-model-conventions/references/templates.md) |
| Tabs and sorts | Its own tabs, 0 to 9 in reading order | [tab-layout.md](tab-layout.md) |
| Fields | Area-prefixed aliases, pickers for links, repeaters for structured lists | [tab-layout.md](tab-layout.md#field-rules) |
| Vary by culture | Ask on a multi-language site: footer text and navigation labels usually vary; a tracking id does not | — |

### Settings already on the home page

If the home page already carries settings fields with content, do not plan to delete them. Say
that moving them to the new node loses their values unless they are migrated (a content task),
specify the new node alongside, and record the removal from the home page as a later change once
the values have moved. The same rule applies to any existing property moved between types.

## 3. Write the spec

Follow [spec-format.md](../../umbraco-content-requirements-documentation/references/spec-format.md).

| Page | From | Location | Action |
|---|---|---|---|
| Any new data type (pickers, a script text area) | [data-type.md](../../umbraco-content-requirements-documentation/assets/data-type.md) | `Data-Types/<Editor kind>/` | Create |
| Any repeater element and its Block List | via [umbraco-add-element-type](../../umbraco-add-element-type/SKILL.md) | `Document-Types/Elements/`, `Data-Types/Block-List/` | Create |
| The settings type | [document-type.md](../../umbraco-content-requirements-documentation/assets/document-type.md) | `Document-Types/SiteSettings.md` | Create |
| The changeset | [changeset.md](../../umbraco-content-requirements-documentation/assets/changeset.md) | `_changesets/<yyyy-mm-dd>-<slug>.md` | — |

Points specific to site settings:

- Default template, Allowed templates, Allowed children and Compositions are all `—`, and the
  `List view` row is deleted. The linter rejects a template on `siteSettings`.
- Every property row has a Tab Sort between 0 and 9, and property sorts in hundreds within each tab.
- `Used by` is `—`: nothing allows it as a child.

The changeset's checklist, in apply order:

```
- [ ] 1. Create any new data types and repeater elements                          (only if needed)
- [ ] 2. Create document type `siteSettings`, then fix-up (tab sorts 0-9, sorts, descriptions, allowed at root)
- [ ] 3. Create the Site Settings content node at the content root                (content, by the user)
- [ ] 4. Verify every spec against the site and set each status line
```

Then lint the folder with the spec skill's
[`lint-spec.mjs`](../../umbraco-content-requirements-documentation/scripts/lint-spec.mjs) until it reports no
errors. If Node.js is not available, say the spec was not linted.

## 4. Stop for approval

Follow [the approval gate](../../umbraco-content-requirements-documentation/references/spec-lifecycle.md#the-approval-gate).
Show the changeset's summary and spec list, and call out any settings that currently live elsewhere
and would need migrating, any script fields (editors can inject markup), and anything that rests on
the user's word. Then stop.

Without the MCP, if the user asked for the backoffice steps, give the walkthrough in the same reply,
headed as steps to follow once the changeset is approved. Writing it is not applying it, and the
status stays `proposed`.

## 5. Apply

With the MCP, follow [apply-via-mcp.md](../../umbraco-content-requirements-documentation/references/apply-via-mcp.md),
including the fix-up pass after the create. Without it, follow
[apply-manually.md](../../umbraco-content-requirements-documentation/references/apply-manually.md). For site
settings, also:

- **The create** passes `allowedAsRoot: true` and no `parentId`.
- **The fix-up** sets each tab's `sortOrder` to its 0 to 9 value; the create numbers them in order
  of first appearance, which is right only if the properties were passed in tab order.
- **The content node** is created by the user in the Content section, at the root. Only one.

## 6. Verify and report

Follow [verify.md](../../umbraco-content-requirements-documentation/references/verify.md). Also confirm that the
type is allowed at root, has no template and no allowed children, and that every tab sort matches
the spec.

Report, separately:

1. What was created, by name.
2. What was **read back and matched**, and what was only confirmed by the user or not checked.
3. What is left: creating the settings content node and filling it in, migrating any settings still
   on the home page, the Razor or C# that reads the settings (found by type, not by assuming the
   home page), permissions on the node, and anything in the Apply log.
