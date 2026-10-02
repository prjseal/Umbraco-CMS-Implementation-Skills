---
name: umbraco-add-site-settings
description: >
  Add the site-wide settings node to an Umbraco 17+ site the way an experienced Umbraco developer
  would: a separate `siteSettings` singleton allowed at the content root, with no template and no
  children, its own tabs numbered 0 to 9 (general, header, footer, social, SEO defaults, scripts),
  and the settings fields in them. The change is written as a markdown requirements doc, approved, then applied
  through the Umbraco Developer MCP or a manual backoffice walkthrough, and read back.
  Use this whenever the user asks to "add site settings", "where should the logo and footer links
  go", "add global settings", "add a settings node", "store the social links for the whole site",
  "add a tracking code setting", or "move settings off the home page".
  SKIP: non-Umbraco projects or Umbraco < 17; settings for a single page (a composition, use
  umbraco-add-composition); block styling settings (use umbraco-add-element-type); appsettings.json
  or code configuration; reading settings in Razor or C#; questions about naming only (use
  umbraco-content-model-conventions).
---

# Add Site Settings

Adds **the site settings singleton**: one `siteSettings` node at the root of the content tree that
holds what belongs to the whole site (the logo, navigation, footer, social links, default sharing
image, tracking codes). It is not the home page. It works requirements doc first: write the change down, stop
for the user's approval, apply it, then read it back.

The rules come from the sibling skill
[`umbraco-content-model-conventions`](../umbraco-content-model-conventions/SKILL.md), and the requirements doc
format, inspect, apply and verify steps come from
[`umbraco-content-requirements-documentation`](../umbraco-content-requirements-documentation/SKILL.md). **Both must be installed
alongside this one.** If a link into either cannot be read, stop and say so; do not work from
memory.

## Steps

| Step | What happens | Reference |
|---|---|---|
| 1. Inspect | Existing settings (on the home page, in another node), root types, languages | [workflow.md](references/workflow.md#1-inspect) |
| 2. Decide the tabs | Which tabs, in which order, 0 to 9 | [tab-layout.md](references/tab-layout.md) |
| 3. Decide the fields | Aliases, data types, repeaters | [workflow.md](references/workflow.md#2-decide) |
| 4. Write and lint | The settings type, any data types and repeaters, a changeset | [workflow.md](references/workflow.md#3-write-the-requirements-doc) |
| 5. Approve | **Stop.** The user approves the changeset | [workflow.md](references/workflow.md#4-stop-for-approval) |
| 6. Apply | Through the MCP if connected, otherwise a manual walkthrough | [workflow.md](references/workflow.md#5-apply) |
| 7. Verify | Read everything back, set the status lines, report | [workflow.md](references/workflow.md#6-verify-and-report) |

### How to decide between MCP and manual

There is one approach with two mechanisms, tried in order. Use the Umbraco Developer MCP when its
tools are in the connected tool list and the site answers. Otherwise write the same requirements doc and hand
the user a manual backoffice walkthrough generated from it. Missing tools are never a reason to
skip the requirements doc, to output uSync or `package.xml` files, or to say the settings exist.

## Version compatibility

Targets **Umbraco 17+**, the version the conventions were derived from and the MCP tools were
checked against (`@umbraco-cms/mcp-dev` 17.6.8 and 18.1.7; see the requirements documentation skill). The connected tool
list is the authority.

## Best practices

- **Settings are not on the home page.** A separate node keeps the home page a page, lets settings
  have their own permissions, and is found by type. This deliberately overrides the older advice to
  put settings on the root page.
- **One per site.** A second `siteSettings` node is a second site; say so if the user asks for one.
- **Its tabs are its own.** Numbered 0 to 9 in the order an editor reads them, not the global tab
  sorts, because no other type shares them.
- **No template, no children, no page compositions.** It is not routable and has nothing below it.
- **Moving settings off the home page loses their values** unless they are migrated. Stop and say
  so; never delete the home page's fields to make room.
- **Approval is a separate turn.** "Just add a settings node" is a request for the change, not
  approval of a requirements doc the user has not seen.

## Validation

Objective assertions live in [`evals/evals.json`](evals/evals.json); run them with
`umbraco-skill-evaluator`. Coverage tier: **Documented**. This skill ships no code and no assets.
The requirements docs it writes are checked by the linter in umbraco-content-requirements-documentation, and an eval graded the guidance.
Nothing ran against a live site.
