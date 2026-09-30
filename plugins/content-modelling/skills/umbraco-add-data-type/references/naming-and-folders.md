# Naming and folders

The patterns are defined in
[naming.md](../../umbraco-content-model-conventions/references/naming.md#data-types) and the
folder rule in
[tree-organisation.md](../../umbraco-content-model-conventions/references/tree-organisation.md#data-types).
This file covers applying them.

## Choosing the name

| Kind | Pattern | Examples |
|---|---|---|
| Generic variant of a built-in | `<Built-in name> (<qualifier>)`, the qualifier saying what differs | `Toggle (default on)`, `Textarea (3 rows)`, `Multi URL Picker (single)` |
| Purpose-specific | `<Subject> <Editor kind>`, the subject saying what it is for | `Meta Description Text Area`, `Alignment Radio Button List`, `Background Colour Picker`, `Event Date Picker` |

- The **subject** names the purpose, not the property or the page: `Meta Description Text Area`,
  not `SEO Composition Meta Description`. Another type with the same need can then reuse it.
- The **qualifier** names the difference in plain words an editor would recognise: `(default on)`,
  not `(value 1)`.
- Title Case, acronyms upper case in the name (`SEO Keywords Tags`), like every display name.
- Never a name that only says "custom" or "new" (`Custom Textarea`, `Textarea 2`).

## Choosing the folder

One folder per property editor, named after the editor as the backoffice shows it, created when
the first custom data type of that kind appears.

| Property editor | Folder |
|---|---|
| `Umbraco.TextBox` | `Text Box` |
| `Umbraco.TextArea` | `Text Area` |
| `Umbraco.RichText` | `Rich Text` |
| `Umbraco.TrueFalse` | `Toggle` |
| `Umbraco.DropDown` | `Dropdown` |
| `Umbraco.RadioButtonList` | `Radio Button List` |
| `Umbraco.CheckBoxList` | `Checkbox List` |
| `Umbraco.Integer`, `Umbraco.Decimal` | `Numeric` |
| `Umbraco.DateOnly`, `Umbraco.DateTimeUnspecified`, `Umbraco.DateTimeWithTimeZone`, `Umbraco.TimeOnly` | `Date Picker` |
| `Umbraco.ColorPicker` | `Color Picker` |
| `Umbraco.MediaPicker3` | `Media Picker` |
| `Umbraco.MultiUrlPicker` | `Multi URL Picker` |
| `Umbraco.ContentPicker`, `Umbraco.MultiNodeTreePicker` | `Content Picker` |
| `Umbraco.Tags` | `Tags` |
| `Umbraco.BlockList`, `Umbraco.BlockGrid`, `Umbraco.ListView` | `Block List`, `Block Grid`, `Collection View` (made by the block editor and listing skills) |

If the project already names these folders differently, keep the project's names.

Built-in data types stay where the installer put them, at the root of Data Types. A variant of a
built-in is a project artefact and goes in its editor's folder.

## The spec page

The spec file name is the data type name with each run of other characters replaced by one hyphen:
`Toggle (default on)` becomes `Toggle-default-on.md`, in `Data-Types/Toggle/`. The linter checks
this. The folder is listed in Dependencies as a `data-type-container`, and its index page is
written from [folder-index.md](../../umbraco-content-model-spec/assets/folder-index.md) if it does
not exist yet.

**Related:** [reuse-or-create.md](reuse-or-create.md), [workflow.md](workflow.md).
