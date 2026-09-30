# Page kinds

Every routable page is one of four kinds. The kind decides the parent, the allowed children, the
compositions and the icon, so settle it before anything else. The kinds are a lookup, not
alternative approaches: the workflow is the same for all of them.

## Lookup

| | Root | Content | Programmatic | Listing |
|---|---|---|---|---|
| **What it is** | The site itself | An everyday page an editor writes | A page whose output is produced by code | A page that lists one item type |
| **Name signals** | Home | About, Contact, Content, Landing, Campaign | Search, Sitemap, Error, Not Found, Login | Blog, News, Events, Articles, "listing", anything plural |
| **Example alias** | `homePage` | `contentPage`, `contactPage` | `searchPage`, `xmlSitemapPage` | `articleListingPage` |
| **Allowed at root** | Yes | No | No | No |
| **Allowed as a child of** | — | The home page and itself | The home page | The home page and content pages |
| **Allows** | Content, listing and programmatic pages | Itself, plus listing pages | Nothing | Its item type, plus itself |
| **Default template** | Yes, named after the alias | Yes | Yes, even though code drives it | Yes |
| **Compositions** | Those every page takes | Those every page takes | Only what applies: SEO fields, usually no sharing image | Those every page takes |
| **Icon** | `icon-home color-light-blue` | A noun that fits, `color-light-blue` | A noun that fits, `color-light-blue` | `icon-list color-light-blue` |
| **This skill** | Adds it | Adds it | Adds it | **Hands off** to umbraco-add-listing-page |

The rules behind each row are in
[allowed-children-and-root.md](../../umbraco-content-model-conventions/references/allowed-children-and-root.md),
[templates.md](../../umbraco-content-model-conventions/references/templates.md),
[compositions.md](../../umbraco-content-model-conventions/references/compositions.md) and
[icons-and-colours.md](../../umbraco-content-model-conventions/references/icons-and-colours.md).
Check any icon name against the site before using it.

## Settling the kind

1. If the name makes the kind obvious (`Home Page`, `Search Page`, `Contact Page`), use it
   without asking.
2. If it is plural or says "listing", "feed" or "archive", it is a listing and a new item type
   together. That is a listing pair: say so and hand off to umbraco-add-listing-page instead of
   building half of it here.
3. If the user says the page is rendered by a controller, a search index or an API, it is
   programmatic, whatever it is called.
4. Otherwise ask one question and nothing else: "Is this a content page editors write, a
   programmatic page rendered by code (like search), or the site's home page?"

## Kind-specific points

### Root

There is one home page per site. If the site already has one, a second root is almost always a
mistake; ask whether this is a second site (a multi-site install) before adding it. When a home
page is added to a site where other page types are already allowed at root, list them and ask
whether each should become a child of the home page instead. Change nothing about an existing type
without the user's confirmation, because content may already sit at the root.

### Content

The default kind, and the one [minimum-viable-page.md](minimum-viable-page.md) produces. It allows
itself so editors can build their own hierarchy. Most content pages need few or no fields of their
own; the page's body and title usually come from compositions or a block grid.

### Programmatic

Still routable, so still a template: without one the page has a URL that returns nothing. The
controller that renders it (route hijacking or a custom `RenderController`) is code and belongs to
an implementation task, not to this schema change. Say that the controller is still to be written
when you report. Route hijacking finds its controller by the document type alias
(`searchPage` is rendered by `SearchPageController`), so agree the alias with whoever writes the
controller before the spec is approved. A programmatic page allows no children and takes compositions selectively; a
search results page wants SEO fields but rarely a sharing image, and it may want indexing off.

### Listing

Out of scope here. A listing is only useful with its item type, its own collection view data type
and the allowed-children pairing between them, which umbraco-add-listing-page creates together.
If the user insists on only the listing type, say that it will not be usable until the item type
exists.

**Related:** [workflow.md](workflow.md), [minimum-viable-page.md](minimum-viable-page.md).
