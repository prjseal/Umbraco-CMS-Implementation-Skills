# MCP read results: site with no compositions

Recorded from `@umbraco-cms/mcp-dev` 17.6.8 connected to a local Umbraco 17.5.3 site with the
`document-type`, `data-type`, `template` and `language` collections enabled. Treat it as what the
connected server reports. Nothing has been written to the site.

## Connected document-type, template and language tools

```
get-all-document-types
get-document-type-by-id
get-document-types-by-id-array
get-document-type-tree-search
get-document-type-ancestors
get-document-type-allowed-children
create-document-type
update-document-type
move-document-type
create-document-type-folder
get-icons
get-all-data-types
find-data-type
get-data-type
create-data-type
create-data-type-folder
get-template
get-template-root
get-template-children
create-template
get-language
```

## get-all-document-types

```json
[
  { "id": "b2e3c4d5-2222-4d3e-9fa0-000000000010", "name": "Content Page", "isFolder": false, "isElement": false, "parent": null },
  { "id": "b2e3c4d5-2222-4d3e-9fa0-000000000011", "name": "Home Page", "isFolder": false, "isElement": false, "parent": null }
]
```

There are no document type folders.

## get-document-type-by-id (both types)

| Name | Alias | allowedAsRoot | Compositions | Allowed children | Default template | Own properties: alias (tab, sort) |
|---|---|---|---|---|---|---|
| Content Page | `contentPage` | false | — | `contentPage` | `contentPage` | `pageTitle` (Content, 100), `bodyText` (Content, 200) |
| Home Page | `homePage` | true | — | `contentPage` | `homePage` | `heroTitle` (Content, 100), `bodyText` (Content, 200) |

Neither type has a meta title, meta description or sharing field.

## Templates (get-template-root, get-template-children)

| Template | Alias | Children |
|---|---|---|
| Master | `master` | Content Page (`contentPage`), Home Page (`homePage`) |

## Data types (find-data-type; built-in only, no custom data types or folders)

| Name | Editor | Id |
|---|---|---|
| Textstring | `Umbraco.TextBox` | `0cc0eba1-9960-42c9-bf9b-60e150b429ae` |
| Textarea | `Umbraco.TextArea` | `c6bac0dd-4ab9-45b1-8e30-e4b619ee5da3` |
| Richtext editor | `Umbraco.RichText` | `ca90c950-0aff-4e72-b976-a30b1ac57dad` |
| True/false | `Umbraco.TrueFalse` | `92897bc6-a5f3-4ffe-ae27-f2e7e33dda49` |
| Image Media Picker | `Umbraco.MediaPicker3` | `ad9f0cf2-bda2-45d5-9ea1-a63cfc873fd3` |

## get-language

```json
{ "items": [ { "isoCode": "en-GB", "name": "English (United Kingdom)", "isDefault": true } ], "total": 1 }
```
