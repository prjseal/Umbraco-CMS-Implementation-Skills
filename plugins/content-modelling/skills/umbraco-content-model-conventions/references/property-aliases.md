# Property aliases

**Applies to:** every property on a document type, composition or element type.
**Look for:** booleans named as nouns (`banner`, `indexing`), page fields with no area prefix
(`title` on a page), block fields with a prefix (`accordionTitle`), overrides with no suffix.
**Why:** the alias is what templates and models read. A predictable alias means nobody opens the
backoffice to find out what a field is called.

## Rules

| Rule | Do | Avoid |
|---|---|---|
| camelCase, acronyms collapsed | `metaTitle`, `ogImage`, `canonicalUrlOverride` | `MetaTitle`, `oGImage`, `canonicalURLOverride` |
| Booleans are predicates | `isIndexable`, `hideBanner`, `openByDefault` | `indexable`, `banner`, `open` |
| Page-level fields carry an area prefix | `pageTitle`, `metaTitle`, `shareTitle` | three fields all called `title` across compositions |
| Block fields are short and generic | `title`, `items`, `link`, `image` | `accordionTitle`, `accordionItems` |
| An override ends in `Override` | `canonicalUrlOverride` | `canonicalUrl` for an optional replacement |
| Umbraco's reserved aliases are kept as they are | `umbracoNaviHide` | renaming it to `hideFromNavigation` |

## Why the two halves differ

Compositions are mixed together on one page type, so two compositions that both define `title`
collide. The area prefix (`page`, `meta`, `share`) makes the alias unique and says where the
value is used.

A block is its own small model, read as `block.Content.Title`. Repeating the block name inside
the alias adds nothing, and generic aliases let one partial render several blocks.

## Overrides

An override replaces a value the site would otherwise compute. Naming it `...Override` tells the
developer there is a fallback to implement and tells the editor to leave it empty by default. Put
the fallback in the property description; see [descriptions.md](descriptions.md).

**Related:** [Names, aliases and suffixes](naming.md), [Descriptions](descriptions.md).
