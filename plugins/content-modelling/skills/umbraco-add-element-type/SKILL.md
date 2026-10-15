---
name: umbraco-add-element-type
description: >
  Specify one element type on an Umbraco 17+ site: a block's content element, a repeater's item,
  a settings model or a settings composition, with its name, alias, folder under Elements/,
  container, fields, icon and description. umbraco-add-block uses it for each element of a block.
  Written as a requirements doc, approved, applied through the Umbraco Developer MCP or a manual
  backoffice walkthrough, and read back.
  Use this whenever the user asks to "create an element type", "add an element type for a
  testimonial", "add a settings composition", "add a spacing option as a settings composition",
  "add a child item element", or "what fields should this element have".
  SKIP: Umbraco < 17; a whole block with its settings and repeater (use
  umbraco-add-block); registering an element in a Block Grid or Block List (use
  umbraco-configure-block-editor); page compositions (use umbraco-add-composition); changing an
  element that exists (use umbraco-change-document-type); the element's Razor partial view.
---

# Add Element Type

Specifies **one element type**: content that lives inside something else, never on its own in
the content tree. An element is one of four roles, and the role decides its folder, its suffix,
its container and what it may compose. It works requirements-first: write the change down, stop
for the user's approval, apply it, then read it back.

**An element type is not a block.** A block is a content element registered in a block area,
with an optional settings element and, for a repeater, its items. A repeater item, a settings model
and a settings composition are elements but never blocks, and a content element becomes a block
only when it is registered. Deciding which elements a block needs is
[`umbraco-add-block`](../umbraco-add-block/SKILL.md)'s job; this skill specifies each one.

The rules come from [`umbraco-content-model-conventions`](../umbraco-content-model-conventions/SKILL.md),
and the requirements doc format and the six-step workflow from
[`umbraco-content-requirements-documentation`](../umbraco-content-requirements-documentation/SKILL.md)
([change-workflow.md](../umbraco-content-requirements-documentation/references/change-workflow.md));
both must be installed alongside this one. If a link into either cannot be read, stop and say so.

## Element roles

| Role | Example | Folder | Reference |
|---|---|---|---|
| Content element | `testimonial`, `accordion` | `Elements/` | [element-roles.md](references/element-roles.md#content-element) |
| Repeater item | `accordionItem`, `pricingRow` | `Elements/` | [element-roles.md](references/element-roles.md#repeater-item) |
| Settings model | `blockSettings`, `imageBlockSettings` | `Elements/Settings/` | [element-roles.md](references/element-roles.md#settings-model) |
| Settings composition | `spacingSettingsComposition` | `Elements/Compositions/` | [element-roles.md](references/element-roles.md#settings-composition) |

The steps are the same for every role: inspect, decide, write the requirements doc, **stop for
approval**, apply, verify. What an element adds at each step is in
[workflow.md](references/workflow.md).

When the user asks for an element that only makes sense as part of a block ("an accordion with
panels", "a hero with a background colour option"), say so and use
[`umbraco-add-block`](../umbraco-add-block/SKILL.md), which calls this skill for each element.

### How to decide between MCP and manual

Use the Umbraco Developer MCP when it is connected, otherwise a manual walkthrough from the same
requirements doc; see
[change-workflow.md](../umbraco-content-requirements-documentation/references/change-workflow.md#5-apply).

## Used by another skill

When another skill (such as `umbraco-add-block` or `umbraco-add-page`) calls this one, run
inspect, decide and write only, and hand back the element's requirements page and its checklist
rows. The calling skill owns the changeset, the single approval and the apply order, so do not
stop for approval separately.

## Version compatibility

Targets **Umbraco 17+**. MCP tool names were checked as described in
[`umbraco-content-requirements-documentation`](../umbraco-content-requirements-documentation/SKILL.md#version-compatibility);
the connected tool list is the authority.

## Best practices

- **No tabs on a content element or an item.** One `Content` group, sort 0; see
  [tabs-groups-sorts.md](../umbraco-content-model-conventions/references/tabs-groups-sorts.md#which-container-to-use).
- **A plain noun.** `testimonial`, not `testimonialBlock`; the `...Block` suffix is only for a
  clash with a media type, data type or reserved word; see
  [naming.md](../umbraco-content-model-conventions/references/naming.md#suffixes).
- **Content on the element, style on its settings model.** Colours, spacing and anchors never go
  on an element an editor fills in; see
  [compositions.md](../umbraco-content-model-conventions/references/compositions.md#blocks-settings-compositions-and-settings-models).
- **Page compositions never go on elements**, and a settings composition is composed only into
  settings models.
- **Create at the root, then move.** `create-element-type` has no folder parameter, so every
  element is moved into its folder straight after it is created.
- "Just create the element" is a request for the element, not approval of a requirements doc the
  user has not yet seen.

## Validation

Objective assertions live in [`evals/evals.json`](evals/evals.json); run them with
`umbraco-skill-evaluator`. Coverage tier: **Documented**. This skill ships no code and no assets.
The requirements docs it writes are checked by the linter in
`umbraco-content-requirements-documentation`, and an eval graded the guidance. Nothing ran against
a live site.
