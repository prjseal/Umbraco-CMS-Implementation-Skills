---
name: umbraco-add-composition
description: >
  Add one single-concern composition to an Umbraco 17+ site and apply it to the document types
  that need it: SEO fields, sharing fields, page details, visibility and the like. The change is
  written as a markdown spec, approved, then applied through the Umbraco Developer MCP or a manual
  backoffice walkthrough, and read back.
  Use this whenever the user asks to "add a composition", "create an SEO composition", "add open
  graph fields to every page", "share these fields between page types", "add meta title and
  description to our pages", "move these fields into a composition", or "which pages should get
  this composition".
  SKIP: non-Umbraco projects or Umbraco < 17; settings compositions for block settings models (use
  umbraco-add-element-type); adding a page type (use umbraco-add-page-type); creating or choosing
  data types in depth (use umbraco-add-data-type); questions about naming only (use
  umbraco-content-model-conventions); reading composition fields in Razor or C#.
---

# Add Composition

Adds **one composition for document types**: a reusable set of fields that does one job, placed
in `Compositions/`, and applied to the page types that need that job. It works spec first: write
the change down, stop for the user's approval, apply it, then read it back.

This skill decides only what is specific to a composition. The rules come from the sibling skill
[`umbraco-content-model-conventions`](../umbraco-content-model-conventions/SKILL.md), and the spec
format, inspect, apply and verify steps come from
[`umbraco-content-requirements-documentation`](../umbraco-content-requirements-documentation/SKILL.md). **Both must be installed
alongside this one.** If a link into either cannot be read, stop and say so; do not work from
memory.

## Steps

| Step | What happens | Reference |
|---|---|---|
| 1. Inspect | Read the existing compositions and every type that would take the new one | [workflow.md](references/workflow.md#1-inspect) |
| 2. Decide the concern | One job, its name, tab, group and fields | [standard-compositions.md](references/standard-compositions.md) |
| 3. Decide where it goes | Which types take it, and which must not | [when-to-apply.md](references/when-to-apply.md) |
| 4. Write and lint | The composition, an Update for each target type, a changeset | [workflow.md](references/workflow.md#3-write-the-spec) |
| 5. Approve | **Stop.** The user approves the changeset | [workflow.md](references/workflow.md#4-stop-for-approval) |
| 6. Apply | Through the MCP if connected, otherwise a manual walkthrough | [workflow.md](references/workflow.md#5-apply) |
| 7. Verify | Read everything back, set the status lines, report | [workflow.md](references/workflow.md#6-verify-and-report) |

### How to decide between MCP and manual

There is one approach with two mechanisms, tried in order. Use the Umbraco Developer MCP when its
tools are in the connected tool list and the site answers. Otherwise write the same spec and hand
the user a manual backoffice walkthrough generated from it. Missing tools are never a reason to
skip the spec, to output uSync or `package.xml` files, or to say the composition exists.

## Version compatibility

Targets **Umbraco 17+**, the version the conventions were derived from and the MCP tools were
checked against (`@umbraco-cms/mcp-dev` 17.6.8 and 18.1.7; see the spec skill). Tool names drift
between MCP versions, so the connected tool list is the authority.

## Best practices

- **One concern per composition.** SEO and sharing are two compositions even when they share a
  tab, so a page can take one without the other.
- **Flat, and fields only.** A composition never composes another, and has no template, children
  or allow-at-root.
- **Applying is editing existing types.** Every target is an existing type, often with content,
  changed by a full-body replace. Read it, modify it, write it, and compare the property count.
- **A clashing alias blocks the composition.** A type that already has a property with one of the
  composition's aliases cannot take it. Moving such a field into a composition deletes the stored
  values unless they are migrated first, so stop and say so; never remove a property to make room.
- **Approval is a separate turn.** A request to "just add it to every page" is a request for the
  composition, not approval of a spec the user has not seen.

## Validation

Objective assertions live in [`evals/evals.json`](evals/evals.json); run them with
`umbraco-skill-evaluator`. Coverage tier: **Documented**. This skill ships no code and no assets.
The specs it writes are checked by the spec skill's linter, and an eval graded the guidance.
Nothing ran against a live site.
