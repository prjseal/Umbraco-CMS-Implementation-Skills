# Settings models and settings compositions

A block has two halves in Umbraco: the **content** an editor writes and the **settings** that
control how it looks. Each half is its own element type. This file decides the settings half.
The rules behind it are in
[compositions.md](../../umbraco-content-model-conventions/references/compositions.md#blocks-settings-compositions-and-settings-models).

## The three layers

| Layer | What it is | Alias | Folder | Own properties |
|---|---|---|---|---|
| Settings composition | One style concern: a background colour, an anchor, spacing | `...SettingsComposition` | `Elements/Compositions/` | Yes, on the `Style` (50) or `Settings` (100) tab, no groups |
| Settings model | What a block's settings are, assembled from settings compositions | `<block>Settings` | `Elements/Settings/` | **None** |
| Content element | The block itself | plain noun | `Elements/` | Yes, in one `Content` group |

The settings model is attached to the content element where the block is registered in a Block
Grid or Block List, not on the element itself. That registration belongs to
[umbraco-configure-block-editor](../../umbraco-configure-block-editor/SKILL.md); this skill only makes the settings model exist and says which
block it is for.

## Which settings model a block uses

| The block needs | Use |
|---|---|
| No style options at all | No settings model |
| The usual options, and the block has no title | The site's shared settings model for untitled blocks |
| The usual options, and the block has a title | The site's shared settings model for titled blocks |
| An option the shared models do not offer | A bespoke `<block>Settings`, composed of the shared settings compositions plus a new one for the extra option |

A project normally has exactly two shared settings models, one for blocks without a title and one
for blocks with a title (the titled one adds title options such as heading level or alignment).
Their names are the project's; `blockSettings` and `titledBlockSettings` are reasonable when there
are none yet. If the site has none, create the one this block needs as a shared model, not a
bespoke one, and say that the next block can reuse it.

A bespoke settings model is still built only from settings compositions. If a block needs one
extra option, make that option a new settings composition and compose it in; never give a
settings model a property of its own.

## Settings compositions

Each settings composition is one concern, used by any settings model that needs it.

| Concern | Alias | Tab (sort) | Typical field: alias (data type) |
|---|---|---|---|
| Background colour | `backgroundColourSettingsComposition` | Style (50) | `backgroundColour` (a colour picker data type with the site's palette) |
| Spacing | `spacingSettingsComposition` | Style (50) | `spacing` (a dropdown of the site's spacing sizes) |
| Anchor | `anchorSettingsComposition` | Settings (100) | `anchorId` (Textstring) |
| Visibility | `visibilitySettingsComposition` | Settings (100) | `isHidden` (True/false) |

Property sorts go 100, 200 within each. A field whose values come from a fixed list (colours,
sizes) needs its own purpose data type, named and foldered by
[naming.md](../../umbraco-content-model-conventions/references/naming.md#data-types), for example
`Background Colour Picker` in `Color Picker/`. That data type joins the changeset ahead of the
settings composition. Choosing and configuring it in depth belongs to [umbraco-add-data-type](../../umbraco-add-data-type/SKILL.md).

A settings composition never composes another one, has no template and is never applied to a
content element or a page type.

## Descriptions

A settings model is described by the blocks it serves ("Settings for blocks without a title:
background colour, spacing and anchor."). A settings composition is described as "Adds ..."
like any composition. Settings fields say what each choice does and what the default is ("Leave
empty for the page background.").

**Related:** [workflow.md](workflow.md), [nested-items.md](nested-items.md).
