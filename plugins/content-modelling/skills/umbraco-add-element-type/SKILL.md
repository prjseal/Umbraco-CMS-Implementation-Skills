---
name: umbraco-add-element-type
description: >
  Add one block to an Umbraco 17+ site the way an experienced Umbraco developer would: the
  element type, its settings model built from settings compositions, and, for a repeater, its
  `...Item` child element and the Block List that holds the children. The change is written as a
  markdown requirements doc, approved, then applied through the Umbraco Developer MCP or a manual backoffice
  walkthrough, and read back.
  Use this whenever the user asks to "add a block", "create an element type", "add an accordion
  block", "make a carousel with slides", "add a settings model for this block", "give the block a
  background colour option", or "add a settings composition".
  SKIP: non-Umbraco projects or Umbraco < 17; making the block available in a page's Block Grid or
  Block List (use umbraco-configure-block-editor); compositions for page types (use
  umbraco-add-composition); page types (use umbraco-add-page-type); creating or choosing data types
  in depth (use umbraco-add-data-type); questions about naming only (use
  umbraco-content-model-conventions); writing the block's Razor partial view.
---

# Add Element Type

Adds **one block**: its content element, how it is styled, and, when it repeats something, the
child element and the Block List that holds the children. A block is up to four kinds of
artefact, and this skill decides which of them are needed and how they fit together. It works
requirements doc first: write the change down, stop for the user's approval, apply it, then read it back.

Making the finished block available in a page's Block Grid or Block List is a separate step,
done with [`umbraco-configure-block-editor`](../umbraco-configure-block-editor/SKILL.md).

The rules come from the sibling skill
[`umbraco-content-model-conventions`](../umbraco-content-model-conventions/SKILL.md), and the requirements doc
format, inspect, apply and verify steps come from
[`umbraco-content-requirements-documentation`](../umbraco-content-requirements-documentation/SKILL.md). **Both must be installed
alongside this one.** If a link into either cannot be read, stop and say so; do not work from
memory.

## The parts of a block

| Part | Needed when | Lives in | Reference |
|---|---|---|---|
| Content element (`accordion`) | Always | `Elements/` | [workflow.md](references/workflow.md#2-decide) |
| Settings model (`accordionSettings`, or a shared one) | The block has style options | `Elements/Settings/` | [settings-models.md](references/settings-models.md) |
| Settings composition (`backgroundColourSettingsComposition`) | A style option no existing settings composition gives | `Elements/Compositions/` | [settings-models.md](references/settings-models.md) |
| Child element and its Block List (`accordionItem`, `Accordion Items`) | The block repeats a set of fields | `Elements/`, `Data Types/Block List/` | [nested-items.md](references/nested-items.md) |

The steps are the same for every block: inspect, decide, write the requirements doc, **stop for approval**,
apply, verify. They are in [workflow.md](references/workflow.md).

### How to decide between MCP and manual

There is one approach with two mechanisms, tried in order. Use the Umbraco Developer MCP when its
tools are in the connected tool list and the site answers. Otherwise write the same requirements doc and hand
the user a manual backoffice walkthrough generated from it. Missing tools are never a reason to
skip the requirements doc, to output uSync or `package.xml` files, or to say the block exists.

## Version compatibility

Targets **Umbraco 17+**, the version the conventions were derived from and the MCP tools were
checked against (`@umbraco-cms/mcp-dev` 17.6.8 and 18.1.7; see the requirements documentation skill). Tool names drift
between MCP versions, so the connected tool list is the authority. `create-element-type` has no
folder parameter, so every element is created at the tree root and then moved.

## Best practices

- **The content element carries content only.** Colours, spacing and anchors go on a settings
  model, never on the element an editor fills in.
- **Reuse before you create.** Most blocks are served by one of the site's two shared settings
  models; a bespoke `<block>Settings` is for style options the shared ones lack.
- **No tabs on an element.** One `Content` group, sort 0. An element that seems to need tabs is two
  elements, or its second tab is really its settings model.
- **A repeater is a parent with a Block List of items**, not a long list of numbered fields and not
  a composition. The minimum and maximum live in the Block List data type.
- **Leaf first.** Settings compositions, then the settings model, then child elements, then the
  Block List, then the parent: each needs the ids of the ones before.
- **Approval is a separate turn.** A request to "just build the block" is a request for the block,
  not approval of a requirements doc the user has not seen.

## Validation

Objective assertions live in [`evals/evals.json`](evals/evals.json); run them with
`umbraco-skill-evaluator`. Coverage tier: **Documented**. This skill ships no code and no assets.
The requirements docs it writes are checked by the linter in umbraco-content-requirements-documentation, and an eval graded the guidance.
Nothing ran against a live site.
