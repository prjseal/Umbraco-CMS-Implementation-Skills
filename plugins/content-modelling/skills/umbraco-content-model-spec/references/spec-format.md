# Spec format

One markdown page per artefact, in a folder that mirrors the backoffice trees. Start every page
from the matching template in `assets/` and replace every `<Placeholder>`; the linter rejects any
that are left.

## Folder layout

```
docs/umbraco-schema/
    README.md                         records the spec location; links to the indexes
    .order                            optional: page order for wikis that read it
    _changesets/
        2026-09-30-article-section.md
    Data-Types.md                     folder index
    Data-Types/
        Toggle.md                     folder index
        Toggle/Toggle-default-on.md
    Document-Types.md
    Document-Types/
        ArticlePage.md                routable pages at the root, as in the backoffice
        Compositions/SeoComposition.md
        Data/AuthorFolder.md
        Elements/Accordion.md
        Elements/Compositions/...     settings compositions
        Elements/Settings/...         settings models
    Templates.md
    Templates/Master.md
    Templates/Master/ArticlePage.md   a page template sits under its master
```

The folder a page is in decides which rules apply to it, so put it where the artefact lives in
the backoffice. The tree itself is defined in
[tree-organisation.md](../../umbraco-content-model-conventions/references/tree-organisation.md).

| Artefact | File name | Template |
|---|---|---|
| Document type (page, data folder or item, site settings) | the alias in PascalCase: `ArticlePage.md` | [document-type.md](../assets/document-type.md) |
| Composition | `SeoComposition.md` | [composition.md](../assets/composition.md) |
| Element type, settings model, settings composition | `AccordionItem.md` | [element-type.md](../assets/element-type.md) |
| Data type | the name with each run of other characters as one hyphen: `Toggle-default-on.md` | [data-type.md](../assets/data-type.md) |
| Block List or Block Grid data type | as above | [block-data-type.md](../assets/block-data-type.md) |
| Collection view data type | as above | [collection-view-data-type.md](../assets/collection-view-data-type.md) |
| Template | the alias in PascalCase | [template.md](../assets/template.md) |
| Folder index | the folder name, beside the folder | [folder-index.md](../assets/folder-index.md) |
| Changeset | `<yyyy-mm-dd>-<slug>.md` in `_changesets/` | [changeset.md](../assets/changeset.md) |

## Page layout

Every spec page has, in this order:

1. `# Name`, the artefact's display name.
2. The status line, `> **Status:** proposed`. Values are in
   [spec-lifecycle.md](spec-lifecycle.md). An element type adds a second blockquote saying it is
   an element type.
3. `## Definition`, a `| Setting | Value |` table. The rows are fixed per kind; keep every row
   in the template and use an em dash for a value that does not apply. The one optional row is
   `List view`: keep it on a listing page and delete it everywhere else. Element types have no
   culture or template rows.
4. `## Properties` (content types) or `## Configuration` (data types). Templates have neither.
5. `## Used by`, what refers to this artefact.
6. `## Dependencies`, what this artefact needs.

There are no GUID, UDI or Deploy rows. Identity is the alias (or the name, for a data type); ids
are looked up at apply time and never written into a spec.

### Writing values

- An empty value is an em dash, `—`. Never leave a cell blank.
- Aliases, editor aliases, value types, icons and file names are in backticks.
- A reference to another artefact with a spec page is a **relative markdown link** to that page.
  A reference to something that already exists on the site and has no spec page is plain text,
  and it must then appear in Dependencies with the flag `Exists`.
- `Folder` is a breadcrumb of links to the folder index pages.
- Yes and No are written `Yes` and `No`.

### Properties

Exactly twelve columns:

```
| Tab | Tab Sort | Group | Group Sort | Name | Alias | Data Type | Editor | Value Type | Mandatory | Sort | Description |
```

One row per property the type defines itself. Inherited properties are not repeated. A type with
none writes one line instead of the table: `No own properties: everything is inherited from the
compositions listed above.`

Element types write `—` for Tab and Tab Sort, `Content` for Group and `0` for Group Sort. The
sorts to use are in
[tabs-groups-sorts.md](../../umbraco-content-model-conventions/references/tabs-groups-sorts.md).

### Dependencies

```
| Artifact | Type | Flags |
```

`Type` is one of `data-type`, `document-type`, `template`, `data-type-container`,
`document-type-container`. `Flags` is exactly one of:

| Flag | Meaning |
|---|---|
| `Exists` | Already on the site; confirmed when the schema was inspected |
| `New in this changeset` | Created by this changeset; must link to its spec page |
| `Missing` | Needed, not on the site and not in the changeset. The spec cannot be approved until this is resolved |

Every data type named in Properties must be listed. A folder is listed as a container and links
to its index page.

## Index pages and `.order`

An index page is optional. It is named after its folder, starts with
`> Folder index: lists the specs in this folder.` and contains one link per page. A `.order` file
lists the page names in a folder, one per line and without the extension, for wikis that use it.

## Changeset

A changeset lists the specs a change consists of, the order they are applied in and a tick-list.
It is the document the user approves. Sections: `## Summary`, `## Specs` (an
`| Order | Spec | Kind | Action |` table; Action is `Create` or `Update`), `## Apply checklist`
(one `- [ ]` item per step, in apply order, ending with allowed children and verification) and
`## Apply log`. The order is defined in [apply-via-mcp.md](apply-via-mcp.md#order).

## Linting

```bash
node scripts/lint-spec.mjs docs/umbraco-schema
```

Run it from this skill's folder, or give the full path to the script. It needs Node.js and
nothing else. It exits 0 when there are no errors, 1 when there are, and prints one line per
finding with the file, line and rule. It checks: required headings and Definition rows, the
twelve-column header, the status line, alias casing and suffix by folder, tab sorts against the
global table, element and composition shape, a template named after the alias for every routable
page, that links resolve, that flags are valid, and that no `<Placeholder>` is left. A property
sort that is not a multiple of 100 is a warning.

If Node.js is not available, say the spec was not linted. Do not report it as lint-clean.
