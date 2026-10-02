---
name: umbraco-configure-block-editor
description: >
  Configure a block editor data type in Umbraco 17+ and the property that uses it: a Block Grid
  for each placement where editors lay out content (approach A), or a Block List for each
  repeater of one kind of item (approach B). Covers which blocks each allows, their settings
  models, groups, column spans and limits, and adding a block to an existing placement. The change
  is written as a markdown requirements doc, approved, then applied through the Umbraco Developer MCP or a
  manual backoffice walkthrough, and read back.
  Use this whenever the user asks to "add a block grid", "set up the main content area", "add this
  block to the page", "make the new block available in the grid", "configure the block list",
  "which blocks can editors use here", "add a sidebar with blocks", or "set column spans".
  SKIP: non-Umbraco projects or Umbraco < 17; creating the element types themselves (use
  umbraco-add-element-type); other data types (use umbraco-add-data-type); the rich text editor's
  own blocks; a listing's collection view (use umbraco-add-listing-page); Block Grid layout
  stylesheets and block Razor partials.
---

# Configure Block Editor

Configures **one block editor data type**: which blocks it offers, with which settings, where and
how many, and the property that uses it. It works requirements doc first: write the change down, stop for the
user's approval, apply it, then read it back.

The blocks themselves (content elements, settings models, child items) are made by
[`umbraco-add-element-type`](../umbraco-add-element-type/SKILL.md). This skill registers them.

The rules come from the sibling skill
[`umbraco-content-model-conventions`](../umbraco-content-model-conventions/SKILL.md), and the requirements doc
format, inspect, apply and verify steps come from
[`umbraco-content-requirements-documentation`](../umbraco-content-requirements-documentation/SKILL.md). **Both must be installed
alongside this one.** If a link into either cannot be read, stop and say so; do not work from
memory.

## Approaches

| | A: Block Grid per placement | B: Block List per repeater |
|---|---|---|
| Reference | [approach-a-block-grid-placement.md](references/approach-a-block-grid-placement.md) | [approach-b-block-list-repeater.md](references/approach-b-block-list-repeater.md) |
| What it is | An area of a page where editors combine different blocks | An ordered list of one kind of item inside a block or a page |
| Examples | `mainContent` on content pages, a sidebar | Accordion panels, carousel slides, key facts |
| Editor | `Umbraco.BlockGrid` | `Umbraco.BlockList` |
| Name | `<Placement> Block Grid` | The plural of the child: `Accordion Items` |
| Blocks allowed | Many, grouped, each with its settings model | Usually one |
| Layout | Columns, spans, optional layout blocks with areas | A single column |
| Limits | Optional minimum and maximum | Minimum and maximum, usually set |
| Coverage tier | **Documented** | **Documented** |

### How to decide

- **Editors choose what goes where, from several kinds of block:** A. This is the default for any
  page area, even one whose blocks always stack full width today. A Block List placement cannot
  later become a Block Grid without migrating the content, while a grid can start with full-width
  blocks only.
- **Editors add more of the same thing:** B.
- **Both:** a placement (A) offers a parent block whose items are a repeater (B), as the accordion
  does. Configure each data type with its own approach.
- **The project already uses Block Lists for page areas:** follow the project and say which
  convention it departs from.

Umbraco Developer MCP versus manual backoffice steps is not a third approach: both approaches use
the MCP when it is connected and a manual walkthrough when it is not.

## Version compatibility

Targets **Umbraco 17+**. The editor and editor UI aliases (`Umbraco.BlockGrid`,
`Umb.PropertyEditorUi.BlockGrid`, `Umbraco.BlockList`, `Umb.PropertyEditorUi.BlockList`) were read
from the Umbraco 17.5.3 packages. The MCP tools were checked against `@umbraco-cms/mcp-dev` 17.6.8
and 18.1.7 (see the requirements documentation skill); the connected tool list is the authority.

## Best practices

- **One data type per placement or repeater.** Two places that allow different blocks get two
  data types, even when they overlap, so changing one never changes the other.
- **Register blocks with their settings models.** A block's styling is chosen where it is
  registered; register the content element and its settings element together.
- **Changing a block editor changes existing content's options.** Adding a block is safe.
  Removing one, or tightening a limit, affects pages that already use it: read what uses the data
  type first, and never remove a block that content still uses.
- **Limits live in the data type.** "At least one item" and "no more than six" are the data
  type's amount, not the property's validation.
- **Approval is a separate turn.** "Just add it to the grid" is a request for the change, not
  approval of a requirements doc the user has not seen.

## Validation

Objective assertions live in [`evals/evals.json`](evals/evals.json); run them with
`umbraco-skill-evaluator`. Coverage tier: **Documented** for both approaches. This skill ships no
code and no assets. The requirements docs it writes are checked by the linter in umbraco-content-requirements-documentation, and an eval graded
the guidance. Nothing ran against a live site.
