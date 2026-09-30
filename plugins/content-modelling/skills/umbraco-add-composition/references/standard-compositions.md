# Standard compositions

Most sites need the same few page compositions. Start from this table when the user names a
concern ("SEO fields", "sharing image", "hide from navigation"), and change the fields to suit the
project. If the site already has a composition for the concern, extend or reuse it; never create a
second one for the same job.

Each row is **one** composition. Two concerns that happen to be requested together (meta tags and
a sharing image) are still two compositions.

## The table

| Concern | Name and alias | Tab (sort) | Group when the tab is shared | Typical fields: alias (data type) |
|---|---|---|---|---|
| Page title and summary | Page Details Composition, `pageDetailsComposition` | Page Details (200) | — | `pageTitle` (Textstring), `pageSummary` (Textarea) |
| Search engines | SEO Composition, `seoComposition` | SEO & Sharing (600) | SEO (0) | `metaTitle` (Textstring), `metaDescription` (a purpose-built text area, or Textarea), `isIndexable` (a toggle that defaults on), `canonicalUrlOverride` (Textstring) |
| Social sharing | Open Graph Composition, `openGraphComposition` | SEO & Sharing (600) | Sharing (100) | `shareTitle` (Textstring), `shareDescription` (Textarea), `shareImage` (Image Media Picker) |
| Tags | Tags Composition, `tagsComposition` | Tags (400) | — | `tags` (Tags) |
| Navigation and sitemap visibility | Visibility Composition, `visibilityComposition` | Visibility (900) | — | `umbracoNaviHide` (True/false), `hideFromSitemap` (True/false) |

The tab sorts come from the global table in
[tabs-groups-sorts.md](../../umbraco-content-model-conventions/references/tabs-groups-sorts.md);
the aliases follow
[property-aliases.md](../../umbraco-content-model-conventions/references/property-aliases.md)
(area prefixes on page fields, booleans as predicates, `...Override` for overrides, and
`umbracoNaviHide` kept as Umbraco names it). Property sorts go 100, 200, 300 within the
composition.

## Groups

A composition that owns its tab alone has **no group**. Groups appear only when a second
composition puts fields in the same tab, so the editor can tell the two sets apart:

- `seoComposition` alone in `SEO & Sharing`: no group.
- `openGraphComposition` joins it: `seoComposition`'s fields move into a group `SEO` (sort 0) and
  `openGraphComposition`'s go in a group `Sharing` (sort 100).

So adding the second composition to a shared tab is also an **Update** of the first one. Moving a
property into a group within the same tab changes where it is shown, not the values stored in it.

`SEO & Sharing` contains an ampersand, which the MCP refuses; see
[Values the MCP rejects](../../umbraco-content-model-spec/references/apply-via-mcp.md#values-the-mcp-rejects)
for what to do. The spec keeps the real name.

## Data types

Reuse a built-in data type (Textstring, Textarea, True/false, Image Media Picker, Tags) whenever
its configuration fits. A field with a constraint the built-in cannot express, such as a
160-character limit on a meta description or a toggle that defaults on, gets its own data type,
named and foldered by [naming.md](../../umbraco-content-model-conventions/references/naming.md#data-types)
(`Meta Description Text Area` in `Text Area/`, `Toggle (default on)` in `Toggle/`). That data type
joins the changeset as its own spec page, ahead of the composition. The reuse-or-create decision
in depth belongs to umbraco-add-data-type.

## Descriptions

The composition's description starts "Adds ...", and each property description gives the example,
fallback and default that apply
([descriptions.md](../../umbraco-content-model-conventions/references/descriptions.md)). SEO and
sharing fields almost always have a fallback: say what it is ("Falls back to the page title if this
is not set").

**Related:** [when-to-apply.md](when-to-apply.md), [workflow.md](workflow.md).
