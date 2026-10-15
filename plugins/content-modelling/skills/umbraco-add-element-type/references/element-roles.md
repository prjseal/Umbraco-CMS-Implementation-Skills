# Element roles

Every element type is one of four roles. The role decides where it lives, what it is called, which
container its fields sit in and what it may compose, so settle it before anything else. The roles
are a lookup, not alternative approaches: the workflow is the same for all of them.

## Lookup

| | Content element | Repeater item | Settings model | Settings composition |
|---|---|---|---|---|
| **What it is** | The block an editor picks and fills in | One entry in a repeater's Block List | How a block looks, assembled from settings compositions | One style concern |
| **Example alias** | `testimonial`, `videoBlock` | `accordionItem`, `pricingRow` | `blockSettings`, `imageBlockSettings` | `spacingSettingsComposition` |
| **Suffix** | None; `...Block` only on a clash | `...Item` or `...Row` | `...Settings` | `...SettingsComposition` |
| **Folder** | `Elements/` | `Elements/`, beside its parent | `Elements/Settings/` | `Elements/Compositions/` |
| **Container** | One group `Content`, sort 0, no tab | One group `Content`, sort 0, no tab | None: no own properties | Tab `Style` (50) or `Settings` (100), no group |
| **Compositions** | None | None | Settings compositions only | None |
| **Linter role** | element | element | settings model | settings composition |

The names follow [naming.md](../../umbraco-content-model-conventions/references/naming.md), the
folders [tree-organisation.md](../../umbraco-content-model-conventions/references/tree-organisation.md)
and the containers
[tabs-groups-sorts.md](../../umbraco-content-model-conventions/references/tabs-groups-sorts.md#which-container-to-use).

## Content element

The content of a block: it becomes a block when it is registered in a block area. Its fields are
content only, with short generic aliases (`title`, `image`, `link`), never prefixed with the
block's name
([property-aliases.md](../../umbraco-content-model-conventions/references/property-aliases.md)).
It has no settings fields of its own: a settings model, if the block has one, is attached where
the block is registered, by [umbraco-add-block](../../umbraco-add-block/SKILL.md) or
[umbraco-configure-block-editor](../../umbraco-configure-block-editor/SKILL.md).

## Repeater item

One entry in a list an editor adds to, removes from and reorders. It has the same shape as a
content element and short generic aliases (`title`, `content`, not `itemTitle`). It needs a Block
List and a parent element that holds it; that shape is
[umbraco-add-block](../../umbraco-add-block/SKILL.md)'s
[nested-items.md](../../umbraco-add-block/references/nested-items.md).

## Settings model

Optional: a block has one only when it has style options. Writes the "No own properties" line
and lists its settings compositions in the Compositions row. Which settings model a block uses, and whether a new one is needed at all, is decided in
[settings-models.md](../../umbraco-add-block/references/settings-models.md#which-settings-model-a-block-uses).

## Settings composition

One style concern, applied only to settings models. It has tabs and no groups, unlike the element
template's default rows. A field whose values come from a fixed list (colours, sizes) needs its
own purpose data type, which joins the changeset ahead of it; the usual concerns are in
[settings-models.md](../../umbraco-add-block/references/settings-models.md#settings-compositions).

A new settings composition does nothing until a settings model composes it. When the user asks
for one on its own, name composing it into the settings models that need it as the follow-up,
for [umbraco-add-block](../../umbraco-add-block/SKILL.md) or
[umbraco-change-document-type](../../umbraco-change-document-type/SKILL.md).

**Related:** [workflow.md](workflow.md).
