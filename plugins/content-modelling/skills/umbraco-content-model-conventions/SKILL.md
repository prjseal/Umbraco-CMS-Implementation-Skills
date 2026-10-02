---
name: umbraco-content-model-conventions
description: >
  Reference index of the conventions for modelling content in Umbraco: names and aliases, the
  Document Types tree, tabs, groups and sort orders, compositions, property aliases, descriptions,
  icons and colours, allowed children and allow-at-root, and templates. It creates nothing; it is
  the rule book the other content-modelling skills link into.
  Use this whenever the user asks about "Umbraco naming conventions", "what should I call this
  document type", "where does this go in the document types tree", "what sort order should this
  tab have", "how should I structure compositions", "should this be allowed at root", or
  "does this type need a template", or when another content-modelling skill needs a rule.
  SKIP: non-Umbraco projects; writing or applying a schema spec (use
  umbraco-content-model-spec); reviewing C# or Razor code (use umbraco-common-pitfalls).
---

# Content Model Conventions

An index of the conventions an experienced Umbraco developer applies when modelling content, so
that any developer can tell what an artefact is, where it lives and how it behaves from its name
and position alone. The rules were derived from a production Umbraco 17 schema; they are house
conventions, not Umbraco requirements.

This skill **creates nothing**. It answers "what is the rule?" and the reason behind it. Writing
the spec and applying it to a site belong to the sibling skill
[`umbraco-content-model-spec`](../umbraco-content-model-spec/SKILL.md).

## How to use this index

- **Designing something new:** find the row for the decision you are making, open only that
  reference file, and apply the rule before you write the spec.
- **Reviewing an existing model:** scan for the signatures in the *Look for* column, then open the
  files that match. Report what breaks a rule and why; do not fix anything unasked.
- **Explaining a rule:** each reference file gives the reason and, where one exists, the rejected
  alternative. Quote the reason rather than inventing one.
- **An existing project disagrees with a rule:** the project's established convention wins for
  that project. Say which rule it departs from, follow the project, and stay consistent.

## Names

| Convention | Look for | Reference |
|---|---|---|
| Document type names, aliases and suffixes | A routable type with no `Page` suffix, `SEO` written as an acronym in an alias | [naming.md](references/naming.md) |
| Property aliases | `seoTitle`, `showBanner`, `heroTitle` on a block, unsuffixed overrides | [property-aliases.md](references/property-aliases.md) |
| Descriptions | Empty descriptions, a composition not described as "Adds ..." | [descriptions.md](references/descriptions.md) |
| Icons and colours | The default icon on everything, colour used at random | [icons-and-colours.md](references/icons-and-colours.md) |

## Structure

| Convention | Look for | Reference |
|---|---|---|
| Document Types tree | Pages inside folders, compositions loose at the root | [tree-organisation.md](references/tree-organisation.md) |
| Tabs, groups and sort orders | Tabs sorted 0, 1, 2; properties sorted 1, 2, 3; tabs on an element type | [tabs-groups-sorts.md](references/tabs-groups-sorts.md) |
| Compositions | A composition that composes another, or carries a template or children | [compositions.md](references/compositions.md) |

## Behaviour

| Convention | Look for | Reference |
|---|---|---|
| Allowed children and allow-at-root | Several page types allowed at root, a listing that allows everything | [allowed-children-and-root.md](references/allowed-children-and-root.md) |
| Templates | A routable type with no template, a template on a composition or element | [templates.md](references/templates.md) |

## Version compatibility

The conventions describe schema shape, which has been stable since tabs and groups were reworked
in Umbraco 9 and block editors arrived. They were derived from, and checked against, an
**Umbraco 17** schema; nothing here depends on an API. Skills that apply these rules target
Umbraco 17+.

## Best practices

- Decide the name, folder, tabs and sorts **before** creating anything. Renaming an alias later
  breaks templates, models and content queries.
- Prefer consistency with the project over purity. One convention applied everywhere beats two
  good conventions mixed.
- These rules are checkable. `umbraco-content-model-spec` ships a linter that enforces the
  mechanical ones on a written spec, so write the spec and run the linter instead of checking by
  eye.
- Stating a rule is not the same as verifying a site follows it. Only claim a live model conforms
  after reading it from the site.

## Validation

Objective assertions live in [`evals/evals.json`](evals/evals.json); run them with
`umbraco-skill-evaluator`. Coverage tier: **Documented**. This skill ships no code and no assets;
an eval graded the guidance and nothing ran.
