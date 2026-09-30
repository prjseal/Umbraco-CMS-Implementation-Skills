# Approach B: a Block List per repeater

## When to choose this approach

Choose this when editors add, remove and reorder **more of one kind of item**: the panels of an
accordion, the slides of a carousel, the rows of a pricing table, a page's list of key facts. If
editors combine different kinds of block into a layout, use
[approach A, a Block Grid per placement](approach-a-block-grid-placement.md) instead.

A repeater usually lives inside a parent block (`accordion.items`), and is created with that block
by [`umbraco-add-element-type`](../../umbraco-add-element-type/SKILL.md) (see its
[nested items](../../umbraco-add-element-type/references/nested-items.md)). Use this file when the
repeater is configured on its own: a page-level list, a second list of the same items, a changed
limit, or a repeater that allows a second kind of item.

## Building blocks

| Part | Rule | Example |
|---|---|---|
| The data type | `Umbraco.BlockList`, editor UI `Umb.PropertyEditorUi.BlockList`, named as the plural of its child, in `Block List/` | `Accordion Items` |
| The property | `items` on a parent block; an area-prefixed alias on a page (`keyFacts`) | `items` |
| Registered blocks | Usually one child element, with no settings element | `accordionItem` |
| Amount | Minimum and maximum in the data type (`validationLimit`) | minimum 1, no maximum |
| Single block mode | Only for a property that holds exactly one block | off |
| Inline editing | On for small items edited in place (a title and a line of text) | off by default |

Names and folders come from
[naming.md](../../umbraco-content-model-conventions/references/naming.md#data-types) and
[tree-organisation.md](../../umbraco-content-model-conventions/references/tree-organisation.md#data-types).

### Rules

- **One Block List per repeater.** Two repeaters that allow different items get two data types.
  Two properties that hold exactly the same items may share one.
- **The limit is the data type's amount**, so every property using it behaves the same. Making the
  property mandatory as well is fine; relying on it alone is not.
- **Items have no settings element** unless each one is styled on its own, which is rare. The
  parent block's settings model styles the whole list.
- **A second kind of item** (a carousel that takes image slides and video slides) is a second block
  in the same Block List; name the data type after what the items have in common (`Carousel
  Slides`).
- **A list of the same item across the whole site** (for example key facts on several page types)
  is one Block List used by a property on a composition, not a property on each type.

## Changing an existing repeater

Read the data type with `get-data-type` and `get-references-data-type`. Raising a limit or adding
an item type is safe. Lowering a maximum, raising a minimum or removing an item type affects
content that already has more, fewer or those items: say which, and ask before planning it.

## Steps

Follow the spec skill for each step, with the points below.

1. **Inspect** ([inspect-existing-schema.md](../../umbraco-content-model-spec/references/inspect-existing-schema.md)):
   the existing Block List if any (`get-data-type`), what uses it, the child element(s) and the
   `Block List/` folder.
2. **Write the spec** ([spec-format.md](../../umbraco-content-model-spec/references/spec-format.md)):
   the data type page from
   [block-data-type.md](../../umbraco-content-model-spec/assets/block-data-type.md), with the
   amount as the configuration sentence ("Amount (min/max): 1 / 6.") and one table row per item
   element, settings element `—`. Add an `Update` for the type or element that gains the property.
   Lint with [`lint-spec.mjs`](../../umbraco-content-model-spec/scripts/lint-spec.mjs).
3. **Stop for approval** ([the approval gate](../../umbraco-content-model-spec/references/spec-lifecycle.md#the-approval-gate)).
   Without the MCP, the walkthrough may be given now, headed as steps to follow after approval.
4. **Apply** ([apply-via-mcp.md](../../umbraco-content-model-spec/references/apply-via-mcp.md) or
   [apply-manually.md](../../umbraco-content-model-spec/references/apply-manually.md)): the child
   element first, then the Block List, then the property that uses it. Copy the `values` shape
   from an existing Block List read with `get-data-type` (`blocks` with `contentElementTypeKey`,
   `validationLimit`, `useSingleBlockMode`, `useInlineEditingAsDefault`).
5. **Verify** ([verify.md](../../umbraco-content-model-spec/references/verify.md)): read the data
   type back and compare the registered items and the amount; read the type that uses it.

## Done

Tell the user what was created or changed, what was read back and matched, and what is left: the
Razor that renders the items, and anything in the Apply log.

**Alternative:** [approach A, a Block Grid per placement](approach-a-block-grid-placement.md).
