---
name: umbraco-add-element-type
description: >
  Add one block to an Umbraco 17+ site: the element type, its settings model built from settings
  compositions, and, for a repeater, its `...Item` child element and the Block List that holds the
  children. The change is written as a requirements doc, approved, applied through the Umbraco
  Developer MCP or a manual backoffice walkthrough, and read back.
  Use this whenever the user asks to "add a block", "create an element type", "add an accordion
  block", "make a carousel with slides", "add a settings model for this block", "give the block a
  background colour option", or "add a settings composition".
  SKIP: non-Umbraco projects or Umbraco < 17; making the block available in a page's Block Grid or
  Block List (use umbraco-configure-block-editor); compositions for page types (use
  umbraco-add-composition); choosing or configuring data types in depth (use
  umbraco-add-data-type); writing the block's Razor partial view.
---

# Add Element Type

Adds **one block**: its content element, how it is styled, and, when it repeats something, the
child element and the Block List that holds the children. A block is up to four kinds of
artefact, and this skill decides which of them are needed and how they fit together. It works
requirements-first: write the change down, stop for the user's approval, apply it, then read it
back.

Making the finished block available in a page's Block Grid or Block List is a separate step,
done with [`umbraco-configure-block-editor`](../umbraco-configure-block-editor/SKILL.md).

The rules come from [`umbraco-content-model-conventions`](../umbraco-content-model-conventions/SKILL.md),
and the requirements doc format and the six-step workflow from
[`umbraco-content-requirements-documentation`](../umbraco-content-requirements-documentation/SKILL.md)
([change-workflow.md](../umbraco-content-requirements-documentation/references/change-workflow.md));
both must be installed alongside this one. If a link into either cannot be read, stop and say so.

## The parts of a block

| Part | Needed when | Lives in | Reference |
|---|---|---|---|
| Content element (`accordion`) | Always | `Elements/` | [workflow.md](references/workflow.md#2-decide) |
| Settings model (`accordionSettings`, or a shared one) | The block has style options | `Elements/Settings/` | [settings-models.md](references/settings-models.md) |
| Settings composition (`backgroundColourSettingsComposition`) | A style option no existing settings composition gives | `Elements/Compositions/` | [settings-models.md](references/settings-models.md) |
| Child element and its Block List (`accordionItem`, `Accordion Items`) | The block repeats a set of fields | `Elements/`, `Data Types/Block List/` | [nested-items.md](references/nested-items.md) |

The steps are the same for every block: inspect, decide, write the requirements doc, **stop for
approval**, apply, verify. What a block adds at each step is in
[workflow.md](references/workflow.md).

### How to decide between MCP and manual

Use the Umbraco Developer MCP when it is connected, otherwise a manual walkthrough from the same
requirements doc; see
[change-workflow.md](../umbraco-content-requirements-documentation/references/change-workflow.md#5-apply).

## Version compatibility

Targets **Umbraco 17+**. MCP tool names were checked as described in
[`umbraco-content-requirements-documentation`](../umbraco-content-requirements-documentation/SKILL.md#version-compatibility);
the connected tool list is the authority.

## Best practices

- **Content on the element, style on its settings model.** Colours, spacing and anchors never go
  on the element an editor fills in; see
  [compositions.md](../umbraco-content-model-conventions/references/compositions.md#blocks-settings-compositions-and-settings-models).
- **Reuse before you create.** Most blocks are served by one of the site's two shared settings
  models; a bespoke `<block>Settings` is for style options the shared ones lack. Which one a block
  takes is decided in [settings-models.md](references/settings-models.md#which-settings-model-a-block-uses).
- **No tabs on an element**; see
  [tabs-groups-sorts.md](../umbraco-content-model-conventions/references/tabs-groups-sorts.md#which-container-to-use).
- **A repeater is a parent with a Block List of `...Item` children**, not numbered fields and not a
  composition; see [nested-items.md](references/nested-items.md).
- **Leaf first.** Settings compositions, then the settings model, then child elements, then the
  Block List, then the parent: each needs the ids of the ones before.
- "Just build the block" is a request for the block, not approval of a requirements doc the user
  has not yet seen.

## Validation

Objective assertions live in [`evals/evals.json`](evals/evals.json); run them with
`umbraco-skill-evaluator`. Coverage tier: **Documented**. This skill ships no code and no assets.
The requirements docs it writes are checked by the linter in
`umbraco-content-requirements-documentation`, and an eval graded the guidance. Nothing ran against
a live site.
