# Type roles

A document type that is not an element is one of five roles. The role decides its folder, its
suffix and which skill adds its behaviour, so settle it before anything else.

## Lookup

| Role | What it is | Folder | Suffix | Behaviour added by |
|---|---|---|---|---|
| Page | Something visitors land on | Document Types root | `...Page` | [umbraco-add-page](../../umbraco-add-page/SKILL.md), or [umbraco-add-listing-page](../../umbraco-add-listing-page/SKILL.md) for a listing pair |
| Composition | A reusable set of fields that does one job | `Compositions/` | `...Composition` | [umbraco-add-composition](../../umbraco-add-composition/SKILL.md) |
| Data folder or item | Shared content that pages pick, such as authors or categories | `Data/` | `...Folder`, `...Item`, or a plain noun | [umbraco-add-data-folder](../../umbraco-add-data-folder/SKILL.md) |
| Site settings | The one settings node for the site | Document Types root | `siteSettings` | [umbraco-add-site-settings](../../umbraco-add-site-settings/SKILL.md) |
| Plain type | Any other schema: content read through the Delivery API, a type shown only inside other pages | `Data/` | Never `...Page` | This skill alone |

The names follow [naming.md](../../umbraco-content-model-conventions/references/naming.md#suffixes)
and the folders
[tree-organisation.md](../../umbraco-content-model-conventions/references/tree-organisation.md#document-types).

## Settling the role

1. **Called by another skill:** the caller names the role. Use it.
2. **The user describes something visitors land on** ("a careers page", "a page for each office"):
   it is a page. Say so and hand off to
   [umbraco-add-page](../../umbraco-add-page/SKILL.md), which uses this skill for the schema and
   adds the template, the SEO composition and the parent.
3. **The user asks for fields shared between types**: it is a composition; hand off to
   [umbraco-add-composition](../../umbraco-add-composition/SKILL.md).
4. **Shared content that pages pick from** (authors, categories, locations): a data folder and its
   item; hand off to [umbraco-add-data-folder](../../umbraco-add-data-folder/SKILL.md), which also
   adds the picker.
5. **Otherwise ask one question and nothing else:** "Will visitors land on this as a page, or is it
   content that other pages or an API read?"

## Why a plain type never sits at the root

The Document Types root is for pages. The requirements linter treats any type there, except
`siteSettings`, as a page and expects a `...Page` alias and a template named after it; a plain
type at the root fails both. It goes in `Data/`, where the linter treats it as data: no template,
no `...Page` suffix and not allowed at root.

## A plain type's behaviour rows

Standalone, this skill fills in the behaviour rows the document type template asks for:

| Row | Value |
|---|---|
| Allowed at root | No |
| Default template, Allowed templates | — |
| Allowed children | — unless the user names children |
| Collection | Row deleted; it belongs to listing pages only |

A plain type with no parent that allows it cannot be created in the content tree. Say so in the
changeset summary, and name what the user wants to allow it under as a follow-up for
[umbraco-change-document-type](../../umbraco-change-document-type/SKILL.md).

**Related:** [workflow.md](workflow.md).
