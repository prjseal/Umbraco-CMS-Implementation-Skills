# Checks: structure

Rules: [tree-organisation.md](../../umbraco-content-model-conventions/references/tree-organisation.md),
[allowed-children-and-root.md](../../umbraco-content-model-conventions/references/allowed-children-and-root.md),
[templates.md](../../umbraco-content-model-conventions/references/templates.md).
On a spec folder the linter reports `allowed-at-root`, `template-named-after-alias` and
`listing-collection-view`; carry those into the report.

## Tree

| Check | How to detect | Severity | Fix with |
|---|---|---|---|
| Routable page inside a folder | `get-document-type-ancestors` of a page type returns a folder | Low | a move, no data impact |
| Composition, element or data type loose at the root | A composition not in `Compositions/`, an element not in `Elements/`, a data item not in `Data/` | Low | a move |
| Missing or extra top-level folders | Folders other than `Compositions`, `Data`, `Elements` at the root (a multi-site folder per site is fine) | Low | a move |
| Custom data types not in editor-kind folders | A custom data type at the Data Types root | Low | [umbraco-add-data-type](../../umbraco-add-data-type/SKILL.md) |

## Root and children

| Check | How to detect | Severity | Fix with |
|---|---|---|---|
| Something other than the home page, site settings or a data folder allowed at root | `allowedAsRoot: true` on any other type | Medium | the owning skill; moving existing root content is a content task |
| No home page | Nothing routable allowed at root | Medium | [umbraco-add-page-type](../../umbraco-add-page-type/SKILL.md) |
| Listing allows unrelated types, or not itself | A listing's `allowedDocumentTypes` is not its item plus itself | Medium | [umbraco-add-listing-page](../../umbraco-add-listing-page/SKILL.md) |
| Item, data item, programmatic page or site settings allows children | Non-empty `allowedDocumentTypes` on a leaf | Medium | the owning skill |
| A type nothing allows | Not allowed at root and in no type's allowed children, so it cannot be created | Medium | the owning skill |
| Settings on the home page | A settings tab or settings fields (logo, footer, social links) on the home page type | Medium; moving them is a content migration | [umbraco-add-site-settings](../../umbraco-add-site-settings/SKILL.md) |
| Two site settings types | More than one singleton-style settings type for one install | Medium | [umbraco-add-site-settings](../../umbraco-add-site-settings/SKILL.md) |

## Templates

| Check | How to detect | Severity | Fix with |
|---|---|---|---|
| Routable type with no default template | A page type with no `defaultTemplate` | High: the page has a URL that renders nothing | [umbraco-add-page-type](../../umbraco-add-page-type/SKILL.md) |
| Template alias differs from the type alias | `defaultTemplate` resolves to another alias | Low | the owning skill |
| Template on a non-routable type | A composition, element, data item or site settings with a template | Medium | the owning skill |
| Several allowed templates | More than one allowed template with no stated alternate rendering | Low | the owning skill |
| Page template not under the master | `get-template-children` on the master does not list it | Low | — |

## Listings

| Check | How to detect | Severity | Fix with |
|---|---|---|---|
| Listing with no collection view | A listing type with no `collection` | Low | [umbraco-add-listing-page](../../umbraco-add-listing-page/SKILL.md) |
| Collection view shared by different listings | One collection view data type used by listings of different item types | Low | [umbraco-add-listing-page](../../umbraco-add-listing-page/SKILL.md) |

**Related:** [checks-naming.md](checks-naming.md), [checks-compositions.md](checks-compositions.md).
