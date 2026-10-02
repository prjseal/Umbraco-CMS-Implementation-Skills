# Article Page

> **Status:** approved

## Definition

| Setting | Value |
|---|---|
| Name | Article Page |
| Alias | `articlePage` |
| Kind | Document Type |
| Icon | `icon-newspaper color-light-blue` |
| Description | A single article, news item or story. |
| Folder | [Document Types](../Document-Types.md) |
| Allowed at root | No |
| Vary by culture | No |
| Default template | [Article Page](../Templates/Master/ArticlePage.md) |
| Allowed templates | [Article Page](../Templates/Master/ArticlePage.md) |
| Allowed children | — |
| Compositions | [Page Details Composition](Compositions/PageDetailsComposition.md), [SEO Composition](Compositions/SeoComposition.md) |

## Properties

| Tab | Tab Sort | Group | Group Sort | Name | Alias | Data Type | Editor | Value Type | Mandatory | Sort | Description |
|---|---|---|---|---|---|---|---|---|---|---|---|
| Content | 100 | — | — | Main Content | `mainContent` | [Main Content Block Grid](../Data-Types/Block-Grid/Main-Content-Block-Grid.md) | `Umbraco.BlockGrid` | `Umbraco.Cms.Core.Models.Blocks.BlockGridModel` | No | 100 | The body of the article, built from blocks. |

## Used by

- Allowed as a child of: [Article Listing Page](ArticleListingPage.md)

## Dependencies

| Artifact | Type | Flags |
|---|---|---|
| [Main Content Block Grid](../Data-Types/Block-Grid/Main-Content-Block-Grid.md) | `data-type` | New in this changeset |
| [Page Details Composition](Compositions/PageDetailsComposition.md) | `document-type` | New in this changeset |
| [SEO Composition](Compositions/SeoComposition.md) | `document-type` | New in this changeset |
| [Article Page](../Templates/Master/ArticlePage.md) | `template` | New in this changeset |
