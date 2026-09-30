# Nested items: repeaters

Use this when a block repeats a set of fields an editor can add, remove and reorder: accordion
panels, carousel slides, pricing rows, team members, FAQ questions.

## The shape

```
accordion              parent element in Elements/      title, items
  items  ->  Accordion Items     Block List data type in Data Types/Block List/
               accordionItem     child element in Elements/   title, content
```

| Part | Rule | Example |
|---|---|---|
| Parent element | Plain noun, the block an editor picks | `accordion` |
| Its repeating property | `items`, a Block List | `items` |
| Child element | The parent's noun plus `Item`, or `Row` for table-like data | `accordionItem`, `pricingRow` |
| Block List data type | The plural of the child, in `Block List/` | `Accordion Items` |
| How many | Minimum and maximum in the Block List's amount setting | min 1, max unlimited |

The names follow [naming.md](../../umbraco-content-model-conventions/references/naming.md); the
child sits beside its parent in `Elements/`, where alphabetical order keeps them together
([tree-organisation.md](../../umbraco-content-model-conventions/references/tree-organisation.md)).

## Rules

- **Not numbered fields.** `panel1Title`, `panel2Title` limits the editor and cannot be reordered.
- **Not a composition.** A composition shares fields between types; it does not repeat them.
- **A Block List, not a nested Block Grid.** Items are a list, not a layout.
- **The constraint lives in the data type.** "At least one panel" is the Block List's minimum, so
  every property using it behaves the same; do not rely on the parent property being mandatory.
  Setting the parent property mandatory as well is fine.
- **The child has no settings model** unless each item is styled on its own, which is rare. The
  parent's settings model styles the whole block.
- **One Block List per repeater.** Two repeaters that allow different children get two data types,
  even if they look alike.
- **Short generic aliases on both.** The parent has `title` and `items`, the child `title` and
  `content`, not `accordionTitle` or `itemTitle`
  ([property-aliases.md](../../umbraco-content-model-conventions/references/property-aliases.md)).

## The Block List spec page

Write it from
[block-data-type.md](../../umbraco-content-model-spec/assets/block-data-type.md): property editor
`Umbraco.BlockList`, the amount as the configuration sentence, and one row whose content element is
the child and whose settings element is `—`. It is used by the parent element's `items` property.
Configuring a Block List beyond this, such as a block that appears in several lists or has its own
settings, is umbraco-configure-block-editor's job.

## Order

The child element, then the Block List that registers it, then the parent that uses the Block
List. A parent element cannot be created before its Block List exists, because its `items`
property needs the data type's id.

**Related:** [workflow.md](workflow.md), [settings-models.md](settings-models.md).
