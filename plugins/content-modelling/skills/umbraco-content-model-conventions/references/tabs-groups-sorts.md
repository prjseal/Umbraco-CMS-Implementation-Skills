# Tabs, groups and sort orders

**Applies to:** the tab, group and sort order of every property.
**Look for:** tabs sorted 0, 1, 2; the same tab with different sorts on two compositions;
properties sorted 1, 2, 3; a tab on an element type; an element with several groups.
**Why:** a page type is assembled from compositions. Tabs only line up in the same order on every
page type if every composition agrees on one global sort per tab.

## Which container to use

| Kind | Tabs | Groups |
|---|---|---|
| Document type, composition | Yes, every property is in a tab | Only when two compositions share a tab and their fields need telling apart |
| Element type | No tab | Exactly one group, `Content`, sort 0 |
| Settings composition (for block settings) | `Style` or `Settings` | None |
| Site settings | Yes | As needed |

An element is edited in a small overlay; tabs there are noise. If an element seems to need tabs it
is two elements, or the second tab is really its settings model.

## Global tab sorts

A tab has the same sort everywhere it appears. The name decides the sort, not the type.

| Tab | Sort |
|---|---|
| A tab specific to one type | 0 |
| Content | 100 |
| Page Details | 200 |
| Section Navigation | 300 |
| Tags | 400 |
| Sidebar | 500 |
| SEO & Sharing | 600 |
| Visibility | 900 |
| Admin | 3000 |

The gaps are deliberate: a project can add a tab between two others without renumbering. Editor
content comes first, configuration in the middle, and the tabs most editors never open come last.

Settings compositions use their own two tabs: `Style` sorts 50 and `Settings` sorts 100.

The site settings singleton is the exception. Its tabs are its own, so they are simply numbered
0 to 9 in the order an editor should read them.

## Groups

When two compositions put fields in the same tab, each gives its fields a group so the editor can
see which belong together, for example `SEO` and `Sharing` inside `SEO & Sharing`. Group sorts
start at 0 and order the groups within that tab. A composition that owns its tab alone uses no
group.

## Property sorts

Property sorts go up in hundreds: 100, 200, 300. A page type that needs a field between two
composition fields can then use 150 without touching the composition. Sorts restart in each
container.

## Applying sorts

Sorts are part of the spec. The MCP create tools take sort from array order and cannot set it, so
sorts are written in the fix-up pass that `umbraco-content-requirements-documentation` describes. Do not assume a
created type has the right sorts until you have read it back.

**Related:** [Compositions](compositions.md), [Allowed children and root](allowed-children-and-root.md).
