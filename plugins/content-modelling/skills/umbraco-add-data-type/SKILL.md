---
name: umbraco-add-data-type
description: >
  Decide whether a property in Umbraco 17+ needs a new data type or can reuse one, pick the
  property editor, name it and file it in the Data Types tree the way an experienced Umbraco
  developer would. The change is written as a markdown spec, approved, then applied through the
  Umbraco Developer MCP or a manual backoffice walkthrough, and read back.
  Use this whenever the user asks to "add a data type", "create a dropdown for", "limit this field
  to 160 characters", "make a toggle that defaults to on", "which property editor should I use",
  "can I reuse this data type", "tidy up our data types", or "change this data type's settings".
  SKIP: non-Umbraco projects or Umbraco < 17; Block List and Block Grid data types (use
  umbraco-configure-block-editor); a listing's collection view (use umbraco-add-listing-page);
  building a custom property editor (use the Umbraco backoffice extension skills); questions about
  naming only (use umbraco-content-model-conventions); reading values in Razor or C#.
---

# Add Data Type

Decides the **data type behind a property**: reuse an existing one or create a new one, which
property editor, what it is called, which folder it goes in and how it is configured. It works
spec first: write the change down, stop for the user's approval, apply it, then read it back.

Most of the time the answer is "reuse". This skill exists so the site does not collect a dozen
near-identical text areas, and so a constraint such as a character limit lives in one place.

The rules come from the sibling skill
[`umbraco-content-model-conventions`](../umbraco-content-model-conventions/SKILL.md), and the spec
format, inspect, apply and verify steps come from
[`umbraco-content-requirements-documentation`](../umbraco-content-requirements-documentation/SKILL.md). **Both must be installed
alongside this one.** If a link into either cannot be read, stop and say so; do not work from
memory.

## The decisions

| Decision | Reference |
|---|---|
| Reuse, reuse with a qualifier, or create for a purpose | [reuse-or-create.md](references/reuse-or-create.md) |
| Which property editor fits the field | [editor-selection.md](references/editor-selection.md) |
| The name and the folder | [naming-and-folders.md](references/naming-and-folders.md) |
| Inspect, write, approve, apply, verify | [workflow.md](references/workflow.md) |

### How to decide between MCP and manual

There is one approach with two mechanisms, tried in order. Use the Umbraco Developer MCP when its
tools are in the connected tool list and the site answers. Otherwise write the same spec and hand
the user a manual backoffice walkthrough generated from it. Missing tools are never a reason to
skip the spec, to output uSync or `package.xml` files, or to say the data type exists.

## Version compatibility

Targets **Umbraco 17+**. The property editor and editor UI aliases in
[editor-selection.md](references/editor-selection.md) were read from the Umbraco 17.5.3
`Umbraco.Cms.Core` and `Umbraco.Cms.StaticAssets` packages. Umbraco 17 added separate date-only,
time-only, unspecified and time-zone-aware date editors. The MCP tools were checked against
`@umbraco-cms/mcp-dev` 17.6.8 and 18.1.7 (see the spec skill); the connected tool list is the
authority.

## Best practices

- **Reuse first.** A new data type is justified by a configuration no existing one has, not by a
  new property.
- **Constraints belong in the data type.** A character limit, an item count or an allowed media
  type set on the data type applies everywhere it is used; property validation applies to one
  property and is easy to miss.
- **A shared data type is shared.** Changing its configuration changes every property that uses
  it. Read its references before touching it, and create a qualified variant rather than bend a
  shared one for one field.
- **Changing a property's editor can lose content.** Moving a property to a data type with a
  different value format (text to picker, single to multiple) leaves stored values unreadable.
  Stop and say so; never do it to existing content silently.
- **Built-in data types stay as installed.** Do not rename, move or reconfigure them.
- **Approval is a separate turn.** A request to "just make the field required and 160 characters"
  is a request for the data type, not approval of a spec the user has not seen.

## Validation

Objective assertions live in [`evals/evals.json`](evals/evals.json); run them with
`umbraco-skill-evaluator`. Coverage tier: **Documented**. This skill ships no code and no assets.
The specs it writes are checked by the spec skill's linter, and an eval graded the guidance.
Nothing ran against a live site.
