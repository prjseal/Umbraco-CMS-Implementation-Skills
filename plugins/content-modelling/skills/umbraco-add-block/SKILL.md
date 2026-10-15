---
name: umbraco-add-block
description: >
  Add one block to an Umbraco 17+ site: the content element editors fill in, an optional settings
  element that styles it (a shared or bespoke settings model built from settings compositions)
  and, for a repeater, its `...Item` child and the Block List that holds them. Each element follows
  umbraco-add-element-type; this skill decides which parts a block needs, and registers it in a
  block area the user names. Written as a requirements doc, approved, applied through the Umbraco
  Developer MCP or a manual backoffice walkthrough, and read back.
  Use this whenever the user asks to "add a block", "add an accordion block", "make a carousel
  with slides", "add a team block with members", or "give the block a background colour option".
  SKIP: Umbraco < 17; one element type on its own (use umbraco-add-element-type); a new Block Grid
  or Block List (use umbraco-configure-block-editor); a page that uses the block (use
  umbraco-add-page); page compositions (use umbraco-add-composition); Razor partial views.
---

# Add Block

Adds **one block**: a content element that editors can place in a block area. The content element
holds what editors write. A settings element, which holds how it looks, is **optional**: only a
block with style options has one. When the block repeats something, it also has a child element
and the Block List that holds the children. This skill decides which of those parts a block needs
and how they fit together; each element is specified by
[`umbraco-add-element-type`](../umbraco-add-element-type/SKILL.md). It works requirements-first:
write the change down, stop for the user's approval, apply it, then read it back.

When the user names an existing block area for the block ("add it to the main content grid"), the
registration joins the same changeset as an Update of that Block Grid. Setting up a new block area
is [`umbraco-configure-block-editor`](../umbraco-configure-block-editor/SKILL.md)'s job.

The rules come from [`umbraco-content-model-conventions`](../umbraco-content-model-conventions/SKILL.md),
and the requirements doc format and the six-step workflow from
[`umbraco-content-requirements-documentation`](../umbraco-content-requirements-documentation/SKILL.md)
([change-workflow.md](../umbraco-content-requirements-documentation/references/change-workflow.md));
both must be installed alongside this one. If a link into either cannot be read, stop and say so.

## The parts of a block

| Part | Needed when | Element role | Reference |
|---|---|---|---|
| Content element (`accordion`) | Always | [Content element](../umbraco-add-element-type/references/element-roles.md#content-element) | [workflow.md](references/workflow.md#2-decide) |
| Settings element: a settings model (`accordionSettings`, or a shared one) | Optional: only when the block has style options | [Settings model](../umbraco-add-element-type/references/element-roles.md#settings-model) | [settings-models.md](references/settings-models.md) |
| Settings composition (`backgroundColourSettingsComposition`) | A style option no existing settings composition gives | [Settings composition](../umbraco-add-element-type/references/element-roles.md#settings-composition) | [settings-models.md](references/settings-models.md) |
| Child element and its Block List (`accordionItem`, `Accordion Items`) | The block repeats a set of fields | [Repeater item](../umbraco-add-element-type/references/element-roles.md#repeater-item) | [nested-items.md](references/nested-items.md) |
| Registration in a Block Grid (`Main Content Block Grid`) | The user names an existing block area | — | [workflow.md](references/workflow.md#registering-in-a-named-block-area) |

The steps are the same for every block: inspect, decide, write the requirements doc, **stop for
approval**, apply, verify. What a block adds at each step is in
[workflow.md](references/workflow.md).

### How to decide between MCP and manual

Use the Umbraco Developer MCP when it is connected, otherwise a manual walkthrough from the same
requirements doc; see
[change-workflow.md](../umbraco-content-requirements-documentation/references/change-workflow.md#5-apply).

## Used by another skill

When another skill (such as `umbraco-add-page`) calls this one, run inspect, decide and write
only, and hand back the block's requirements pages and checklist rows. The calling skill owns the
changeset, the single approval and the apply order, so do not stop for approval separately.

## Version compatibility

Targets **Umbraco 17+**. MCP tool names were checked as described in
[`umbraco-content-requirements-documentation`](../umbraco-content-requirements-documentation/SKILL.md#version-compatibility);
the connected tool list is the authority.

## Best practices

- **An element is not a block.** A block is a content element registered in a block area. Its
  settings model, settings compositions and repeater items are elements too, but they are parts of
  the block, never blocks of their own.
- **Settings are optional.** A block with no style options gets no settings element and is
  registered with settings `—`. Do not add a settings model "in case"; one can be added later.
- **Content on the element, style on its settings model.** Colours, spacing and anchors never go
  on the element an editor fills in; see
  [compositions.md](../umbraco-content-model-conventions/references/compositions.md#blocks-settings-compositions-and-settings-models).
- **Reuse before you create.** Most blocks are served by one of the site's two shared settings
  models; a bespoke `<block>Settings` is for style options the shared ones lack. Which one a block
  takes is decided in [settings-models.md](references/settings-models.md#which-settings-model-a-block-uses).
- **A repeater is a parent with a Block List of `...Item` children**, not numbered fields and not a
  composition; see [nested-items.md](references/nested-items.md).
- **Each element follows its role**: the name, folder, container and fields are decided by
  [`umbraco-add-element-type`](../umbraco-add-element-type/references/element-roles.md).
- **Register with the settings model, when there is one.** A block's styling is chosen where it
  is registered, so the content element and its settings element are registered together.
- **Leaf first.** Settings compositions, then the settings model, then child elements, then the
  Block List, then the parent, then the registration: each needs the ids of the ones before.
- "Just build the block" is a request for the block, not approval of a requirements doc the user
  has not yet seen.

## Validation

Objective assertions live in [`evals/evals.json`](evals/evals.json); run them with
`umbraco-skill-evaluator`. Coverage tier: **Documented**. This skill ships no code and no assets.
The requirements docs it writes are checked by the linter in
`umbraco-content-requirements-documentation`, and an eval graded the guidance. Nothing ran against
a live site.
