# Icons and colours

**Applies to:** the icon of every document type and element type.
**Look for:** the default icon on every type, colour applied to some pages and not others, an
icon name that does not exist on the site.
**Why:** the content tree and the create dialog are scanned by icon first. Colour by role lets an
editor tell a page from a data item before reading the name.

## Icon

Pick an icon that matches the noun: `icon-home` for the home page, `icon-newspaper` for an
article, `icon-list` for a listing, `icon-settings` for settings and compositions. Check that the
name exists on the site before using it (the MCP tool `get-icons`, or the icon picker).

## Colour by role

The icon value is the icon name, optionally followed by a colour class:
`icon-newspaper color-light-blue`.

| Role | Colour | Example |
|---|---|---|
| Routable page | `color-light-blue` | `icon-newspaper color-light-blue` |
| Data folder or data item | `color-green` | `icon-folder color-green` |
| Composition | none | `icon-settings` |
| Element type, settings model | none | `icon-blockquote` |

Colour marks the two kinds of node an editor meets in the content tree: pages and data.
Compositions and elements never appear there, so they stay uncoloured.

## What cannot be checked

`get-icons` returns icon names only. It cannot confirm that a colour class is valid, so a wrong
colour fails silently and the icon renders in the default colour. Use only the colour classes in
the table, and say that the colour was not validated when you report what you applied.

**Related:** [Names, aliases and suffixes](naming.md).
