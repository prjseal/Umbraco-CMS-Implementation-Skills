# Workflow

Inspect, decide, write the spec, **stop for approval**, apply, verify. Four of those six steps
are the spec skill's; this file adds only what a listing pair needs at each one. Each of the two
pages is decided the way
[`umbraco-add-page-type`](../../umbraco-add-page-type/SKILL.md) decides one page; read its
[workflow](../../umbraco-add-page-type/references/workflow.md) for the page-level points (template
stub, parent update, compositions that do not exist yet) rather than repeating them here.

Before step 1, find or agree the spec folder as
[spec-lifecycle.md](../../umbraco-content-model-spec/references/spec-lifecycle.md#where-specs-live)
describes. Ask once; never again once it is recorded.

## 1. Inspect

Follow [inspect-existing-schema.md](../../umbraco-content-model-spec/references/inspect-existing-schema.md).
For a listing pair, establish:

| Question | Why it matters here |
|---|---|
| Does a listing or item for this noun already exist? | Extend it instead of making a second |
| Is there a home page, and which content pages exist? | The listing is allowed under the home page (and under content pages) |
| Which page compositions exist? | Both pages take the usual ones; do not repeat their fields |
| Is there a master template? | Both templates go under it |
| Which data types exist for the item's fields, and is there a `Collection View/` folder? | Reuse before creating |
| Where do the items' shared data come from (authors, categories)? | Those are data folders, made separately |
| How many languages? | Vary by culture is asked about only when there is more than one |

Without the MCP, ask these in one message, and treat every answer as the user's word.

## 2. Decide

| Decision | For a listing pair | Rule |
|---|---|---|
| Names and aliases | `<Noun> Listing Page` / `<noun>ListingPage` lists `<Noun> Page` / `<noun>Page`. Singular noun in both | [naming.md](../../umbraco-content-model-conventions/references/naming.md) |
| Folder | Both at the Document Types root | [tree-organisation.md](../../umbraco-content-model-conventions/references/tree-organisation.md) |
| Icons | Listing `icon-list color-light-blue`; item a noun that fits, `color-light-blue` | [icons-and-colours.md](../../umbraco-content-model-conventions/references/icons-and-colours.md) |
| Descriptions | Listing: what it lists ("Lists articles, newest first."). Item: what it is | [descriptions.md](../../umbraco-content-model-conventions/references/descriptions.md) |
| Compositions | The site's page compositions on both | [compositions.md](../../umbraco-content-model-conventions/references/compositions.md) |
| Listing's own fields | Usually none; an intro or featured items at most, on the `Content` tab | [tabs-groups-sorts.md](../../umbraco-content-model-conventions/references/tabs-groups-sorts.md) |
| Item's own fields | What each item holds: a date to order by, the body; `Content` tab, sorts in hundreds | [property-aliases.md](../../umbraco-content-model-conventions/references/property-aliases.md) |
| Templates | One each, named after the alias, under the master | [templates.md](../../umbraco-content-model-conventions/references/templates.md) |
| Allowed children | Listing: the item **and itself**. Item: nothing | [allowed-children-and-root.md](../../umbraco-content-model-conventions/references/allowed-children-and-root.md) |
| Parent | The home page (and content pages, if the site nests sections); never allowed at root | [allowed-children-and-root.md](../../umbraco-content-model-conventions/references/allowed-children-and-root.md) |
| Collection view | Its own data type | [collection-view.md](collection-view.md) |

The listing allows itself so a large section can be divided into sub-listings of the same kind
(news by year, events by region) without a new type.

## 3. Write the spec

Follow [spec-format.md](../../umbraco-content-model-spec/references/spec-format.md). A listing pair
is these pages:

| Page | From | Location | Action |
|---|---|---|---|
| Any new data type for an item field | [data-type.md](../../umbraco-content-model-spec/assets/data-type.md) | `Data-Types/<Editor kind>/` | Create |
| The collection view | [collection-view-data-type.md](../../umbraco-content-model-spec/assets/collection-view-data-type.md) | `Data-Types/Collection-View/<Name>.md` | Create |
| The master template, only if the site has none | [template.md](../../umbraco-content-model-spec/assets/template.md) | `Templates/Master.md` | Create |
| The item's template | [template.md](../../umbraco-content-model-spec/assets/template.md) | `Templates/<Master>/<ItemAlias>.md` | Create |
| The listing's template | [template.md](../../umbraco-content-model-spec/assets/template.md) | `Templates/<Master>/<ListingAlias>.md` | Create |
| The item page type | [document-type.md](../../umbraco-content-model-spec/assets/document-type.md) | `Document-Types/<ItemAlias>.md` | Create |
| The listing page type, with its `List view` row | [document-type.md](../../umbraco-content-model-spec/assets/document-type.md) | `Document-Types/<ListingAlias>.md` | Create |
| The parent, with the listing added to Allowed children | [document-type.md](../../umbraco-content-model-spec/assets/document-type.md) | `Document-Types/HomePage.md` | Update |
| The changeset | [changeset.md](../../umbraco-content-model-spec/assets/changeset.md) | `_changesets/<yyyy-mm-dd>-<slug>.md` | — |

Points specific to a listing pair:

- Keep the `List view` row on the listing page and delete it on the item page. The linter requires
  it on any alias ending `ListingPage`.
- The item's `Used by` names the listing; the listing's `Used by` names the parent and itself.
- The parent follows the page-type rule: an existing spec page is edited, with the MCP a page is
  written from the read-back, without it the change is a checklist item and the summary says the
  parent is the user's word.

The changeset's checklist, in apply order:

```
- [ ] 1. Create any new data type for an item field                              (only if needed)
- [ ] 2. Create data type folder `Collection View` and data type `<Listing name> Collection View`
- [ ] 3. Create template `master`                                               (only if new)
- [ ] 4. Create template `<itemAlias>` under `master`
- [ ] 5. Create template `<listingAlias>` under `master`
- [ ] 6. Create document type `<itemAlias>`, then fix-up (sorts, mandatory, descriptions, template)
- [ ] 7. Create document type `<listingAlias>`, then fix-up (template, collection view)
- [ ] 8. Set allowed children on `<listingAlias>`: `<itemAlias>`, `<listingAlias>`
- [ ] 9. Set allowed children on `homePage`: add `<listingAlias>`, keeping the existing entries
- [ ] 10. Verify every spec against the site and set each status line
```

Then lint the folder with the spec skill's
[`lint-spec.mjs`](../../umbraco-content-model-spec/scripts/lint-spec.mjs) until it reports no
errors. If Node.js is not available, say the spec was not linted.

## 4. Stop for approval

Follow [the approval gate](../../umbraco-content-model-spec/references/spec-lifecycle.md#the-approval-gate).
Show the changeset's summary and spec list, and call out the **existing** parent that will change,
any data (authors, categories) left for a separate change, and anything that rests on the user's
word. Then stop.

Without the MCP, if the user asked for the backoffice steps, give the walkthrough in the same reply,
headed as steps to follow once the changeset is approved. Writing it is not applying it, and the
status stays `proposed`.

## 5. Apply

With the MCP, follow [apply-via-mcp.md](../../umbraco-content-model-spec/references/apply-via-mcp.md),
including the fix-up pass after each create. Without it, follow
[apply-manually.md](../../umbraco-content-model-spec/references/apply-manually.md). For a listing
pair, also:

- **The collection view.** Create it with `create-data-type`, copying the `values` shape from an
  existing collection view read with `get-data-type` (`pageSize`, `orderBy`, `orderDirection`,
  `includeProperties`, `layouts`, `icon`, `tabName`).
- **The item first, then the listing.** The listing's allowed children need the item's id.
- **The listing's fix-up** sets `collection` to `{ "id": <collection view id> }` along with the
  template, and, in the allowed-children step, `allowedDocumentTypes` to the item and itself.
- **The parent** is updated last by read-modify-write, keeping every existing allowed child; see the
  page-type [workflow](../../umbraco-add-page-type/references/workflow.md#5-apply).

## 6. Verify and report

Follow [verify.md](../../umbraco-content-model-spec/references/verify.md). For a listing pair, also
confirm that the listing's `collection` is the new collection view, that each type's allowed
children are exactly as specified, and that the parent lists the listing.

Report, separately:

1. What was created or changed, by name.
2. What was **read back and matched**, and what was only confirmed by the user or not checked.
3. What is left: the listing template's rendering of its children (paging, ordering on the
   website), the item template's markup, any data folders for shared item data (with
   [umbraco-add-data-folder](../../umbraco-add-data-folder/SKILL.md)), and anything in the Apply log.
