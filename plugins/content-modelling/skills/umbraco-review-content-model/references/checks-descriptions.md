# Checks: descriptions and icons

Rules: [descriptions.md](../../umbraco-content-model-conventions/references/descriptions.md),
[icons-and-colours.md](../../umbraco-content-model-conventions/references/icons-and-colours.md).

## Descriptions

| Check | How to detect | Severity | Fix with |
|---|---|---|---|
| Type with no description | Empty `description` on a document type or element type | Low | the owning skill |
| Composition not described as "Adds ..." | A composition description that does not start "Adds" | Low | [umbraco-add-composition](../../umbraco-add-composition/SKILL.md) |
| Description restates the name | "The title" on Title, "Article page" on Article Page | Low | the owning skill |
| Property with a fallback or default not explained | An `...Override` property, a SEO field or a default-on toggle whose description does not say what happens when it is empty | Low | the owning skill |
| Limit not mentioned | A property whose data type enforces a limit the description does not state | Low | the owning skill |

Count descriptions rather than listing each empty one: "31 of 40 properties have no description,
mostly on Article Page and News Page" is more useful than 31 findings.

## Icons

| Check | How to detect | Severity | Fix with |
|---|---|---|---|
| Default icon everywhere | Many types on `icon-document` or the backoffice default | Low | the owning skill |
| Colour not by role | Pages not `color-light-blue`, data not `color-green`, or colour on compositions and elements | Low | the owning skill |
| Icon name that does not exist | An icon not returned by `get-icons` | Low | the owning skill |

`get-icons` returns names only, so a wrong colour class cannot be proven; report colour findings as
seen, not as validated.

**Related:** [checks-naming.md](checks-naming.md).
