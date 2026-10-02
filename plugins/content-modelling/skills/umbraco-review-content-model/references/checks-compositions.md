# Checks: compositions, elements and settings models

Rules: [compositions.md](../../umbraco-content-model-conventions/references/compositions.md).
On a requirements folder the linter reports `composition-shape`, `element-shape` and
`settings-model-shape`; carry those into the report.

## Compositions

| Check | How to detect | Severity | Fix with |
|---|---|---|---|
| A composition that composes another | A type used as a composition that has `compositions` of its own. Umbraco refuses this in the backoffice, so seeing it means imported or migrated schema | Low: rare; report it as data to clean up rather than a modelling defect | [umbraco-add-composition](../../umbraco-add-composition/SKILL.md) |
| Composition with behaviour | A composition with a template, allowed children or allowed at root | Medium | [umbraco-add-composition](../../umbraco-add-composition/SKILL.md) |
| More than one concern | A composition whose fields belong to different jobs (SEO fields and a hero image) | Medium | [umbraco-add-composition](../../umbraco-add-composition/SKILL.md): split it, which means migrating the values of the fields that move |
| Used by one type only | `get-document-type-composition-references` returns one type, and no second is planned | Low | leave or fold back; folding back is a migration |
| Applied to types that do not need it | SEO or sharing compositions on data items, site settings or elements | Low | [umbraco-add-composition](../../umbraco-add-composition/SKILL.md) |
| The same fields on several types instead of a composition | Two or more page types defining the same aliases with the same data types | Medium: extracting them is a migration | [umbraco-add-composition](../../umbraco-add-composition/SKILL.md) |

## Elements and settings

| Check | How to detect | Severity | Fix with |
|---|---|---|---|
| Styling on the content element | Colour, spacing, alignment or anchor properties on an element that editors fill with content | Medium | [umbraco-add-element-type](../../umbraco-add-element-type/SKILL.md) |
| Settings model with own properties | A type in `Elements/Settings/` with properties of its own | Medium | [umbraco-add-element-type](../../umbraco-add-element-type/SKILL.md) |
| A bespoke settings model per block where the shared ones would do | Many `<block>Settings` types with the same compositions | Low | [umbraco-add-element-type](../../umbraco-add-element-type/SKILL.md) |
| Page composition on an element | An element type composing `seoComposition` or another page composition | Medium | [umbraco-add-element-type](../../umbraco-add-element-type/SKILL.md) |
| Repeater as a composition or numbered fields | Repeated item fields in a composition, or `item1...item6` | Medium | [umbraco-add-element-type](../../umbraco-add-element-type/SKILL.md) |

**Related:** [checks-structure.md](checks-structure.md), [checks-sorts.md](checks-sorts.md).
