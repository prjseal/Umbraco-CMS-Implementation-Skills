# Umbraco content-modelling skills

Implemented phase by phase on one branch of the user's fork, committed locally after each phase. Nothing
goes to the upstream `umbraco` repo until every skill is written; the user opens that PR themselves.

## Context

`plugins/content-modelling/skills/` is empty. The goal is a set of discrete, single-task skills that
model Umbraco content the way an experienced developer would, derived from:

- a real production schema (documented as generated markdown, one page per artefact), whose rules
  were inferred from the schema itself;
- the article "Designing an AI agent skill for adding a page type to Umbraco" and the existing
  monolithic `umbraco-add-page-type` skill (`D:\Code\GitHub\Umbraco-Skills\umbraco-add-page-type\umbraco-add-page-type\`).

The author's working habit is to write markdown describing what will be created before creating it.
The skills make that the workflow: **write a spec, get it approved, then apply it**.

## Decisions (made by the user, do not reopen)

| Topic | Decision |
|---|---|
| Markdown role | Spec first, then apply. Status line per spec: `proposed` → `approved` → `applied <date> via MCP` / `applied <date> manually`. |
| Mechanism | Umbraco Developer MCP when connected; otherwise a manual backoffice walkthrough generated from the spec. No uSync/Deploy files, no `package.xml`. |
| Conflicting conventions | The production schema wins over the article/old skill; article rules stay where the schema is silent or agrees. |
| Scope | All 11 skills in one plan, built in 8 phases. |
| Validation | Amend the `umbraco-skill-author` checklist: markdown-only assets are not code; spec-driven skills are tier **Documented**, validated by evals plus a Node spec linter in CI. |
| Client data | Generic examples only (`articlePage`, `seoComposition`, `accordionItem`). No client name, no client descriptions, no Salesforce/FormAssembly-specific types anywhere in the repo. |
| Spec location | Default `docs/umbraco-schema/` in the user's project; ask once, record the choice in its `README.md`. |
| Name clash | Keep the name `umbraco-add-page-type`; it supersedes the old copy (user retires the old one when this ships). |
| Delivery | All phases are built on `feature/content-modelling-skills` in the user's fork (`prjseal/Umbraco-CMS-Implementation-Skills`) and committed locally at the end of each phase, author `Paul Seal <prjseal@gmail.com>`, no assistant attribution. Phases run back to back without check-ins. No push in auto mode: the user pushes. One version bump at the end, then one upstream PR, opened by the user. (Replaces the earlier "uncommitted, one PR per phase" rule.) |

## Skill set

All under `plugins/content-modelling/skills/`. Every skill has `SKILL.md` (frontmatter `name` +
folded `description` with quoted triggers and a `SKIP:` clause) and `evals/evals.json`.

| # | Skill | Purpose | Extra files |
|---|---|---|---|
| 1 | `umbraco-content-model-conventions` | Index of the rules; creates nothing. Shape copied from `plugins/implementation/skills/umbraco-common-pitfalls/`. | `references/`: `naming.md`, `tree-organisation.md`, `tabs-groups-sorts.md`, `compositions.md`, `property-aliases.md`, `descriptions.md`, `icons-and-colours.md`, `allowed-children-and-root.md`, `templates.md` |
| 2 | `umbraco-content-model-spec` | Spec format, lifecycle, inspect, apply, verify. | `references/`: `spec-format.md`, `spec-lifecycle.md`, `inspect-existing-schema.md`, `apply-via-mcp.md`, `apply-manually.md`, `verify.md`; `assets/`: `document-type.md`, `composition.md`, `element-type.md`, `data-type.md`, `block-data-type.md`, `collection-view-data-type.md`, `template.md`, `folder-index.md`, `changeset.md`; `scripts/lint-spec.mjs`; `evals/files/golden-spec/` + a deliberately broken fixture |
| 3 | `umbraco-add-page-type` | One routable page type plus template (port of the old skill, split thin). | `references/`: `page-kinds.md`, `workflow.md`, `minimum-viable-page.md` |
| 4 | `umbraco-add-composition` | One single-concern composition and where to apply it. | `references/`: `workflow.md`, `standard-compositions.md`, `when-to-apply.md` |
| 5 | `umbraco-add-element-type` | Block element, its settings model, nested `…Item` children. | `references/`: `workflow.md`, `settings-models.md`, `nested-items.md` |
| 6 | `umbraco-add-data-type` | Reuse versus new, naming, folder by editor kind. | `references/`: `workflow.md`, `reuse-or-create.md`, `naming-and-folders.md`, `editor-selection.md` |
| 7 | `umbraco-configure-block-editor` | Block Grid per placement (approach A); Block List per repeater (approach B). The only skill with a real A/B. | `references/`: `approach-a-block-grid-placement.md`, `approach-b-block-list-repeater.md` |
| 8 | `umbraco-add-listing-page` | Listing type + item type + collection view + allowed children. | `references/`: `workflow.md`, `collection-view.md` |
| 9 | `umbraco-add-data-folder` | Non-routable containers/items, including content-driven taxonomy. | `references/`: `workflow.md`, `taxonomy.md` |
| 10 | `umbraco-add-site-settings` | The root `siteSettings` singleton. | `references/`: `workflow.md`, `tab-layout.md` |
| 11 | `umbraco-review-content-model` | Audit a live model or a spec folder; report, never fix. | `references/checks-{naming,structure,sorts,data-types,descriptions,compositions}.md`; `assets/review-report.md` |

SKIP boundaries keep descriptions from colliding: page type skips listing pairs (8), compositions (4),
elements (5), data folders (9), settings (10); element type skips catalogue registration (7); data
type skips block editors (7) and collection views (8); review skips fixing and C# review
(`umbraco-common-pitfalls`).

MCP versus manual is a mechanism order inside one approach (the `umbraco-sitemap` precedent), not an
A/B pair. Page kinds (Root / Listing / Content / Programmatic) are a lookup table, not approaches.

### Sharing without duplication

Skills install side by side, so sibling links resolve (`../umbraco-content-model-conventions/references/naming.md`).

- Rules live only in skill 1. Spec format, inspect, apply, manual fallback and verify live only in skill 2.
- Each add-* `workflow.md` is: inspect → decide (its own content) → write spec → **stop for approval** → apply → verify, with four of those steps being links into skill 2.
- Each dependent `SKILL.md` intro states it needs skills 1 and 2 installed alongside; if a link target is unreadable, stop and say so. No inline copies.
- A forward link to a skill not yet built is written as plain text and converted to a link in the phase that adds the target.

## The conventions (content of skill 1)

- **Names and aliases**: names Title Case; aliases camelCase with acronyms collapsed (`seoComposition`, `xmlSitemapPage`). Suffixes: `…Page`; `…ListingPage` with item `…Page`; `…Composition`; `…SettingsComposition`; `<block>Settings`; `…Folder` / `…Item` for data containers; `…Item` / `…Row` for child elements; `…Block` only to avoid a clash. The rejected `HasSeo` prefix is recorded as an aside.
- **Property aliases**: camelCase; booleans as predicates (`isIndexable`, `hideBanner`); page-level fields area-prefixed (`pageTitle`, `metaTitle`, `shareTitle`); block fields short and generic (`title`, `items`, `link`, `image`); overrides end `Override`; `umbracoNaviHide` kept.
- **Tree**: `Compositions/`, `Data/`, `Elements/` (with `Compositions/` and `Settings/`); routable pages at the tree root; folders above loose types; alphabetical.
- **Tabs, groups, sorts**: document types and compositions use tabs, with groups only when compositions share a tab; elements use no tab and one `Content` group (sort 0). Global tab sorts: type-specific 0, Content 100, Page Details 200, Section Navigation 300, Tags 400, Sidebar 500, SEO & Sharing 600, Visibility 900, AI Assistant 2000, Admin 3000. Property sorts in hundreds.
- **Compositions**: one concern each; flat (never compose a composition); no template, children or root; applied selectively.
- **Elements and blocks**: settings models are separate element types built only from settings compositions; two shared generic settings models, bespoke only when style options are needed; repeaters are a parent element with a Block List of `…Item` children; one block editor data type per placement; Block List data types named as the plural of the child, with min/max as the constraint.
- **Data types**: folders by editor kind; reuse generic types with a bracket qualifier (`Toggle (default on)`); purpose-specific `<Subject> <Editor kind>`; constraints live in the data type, not property validation.
- **Root and children**: only the home page, site settings and data/taxonomy folders at root; content page allows itself plus listings; listing allows its item type plus itself; items allow nothing.
- **Templates**: every routable page (including programmatic ones) gets one default template named after the alias, under the master; extra allowed templates only for a genuine alternate rendering; nothing else gets a template.
- **Listings**: a dedicated `<Type> Collection View` data type each.
- **Site settings**: a separate root singleton, no template, tabs sorted 0 to 9 (this overrides the article's "settings on the home node").
- **Icons/colours, descriptions**: colour by role; compositions described as "Adds …"; property descriptions give example, fallback, default.

## Spec-first workflow (content of skill 2)

Spec pages copy the production wiki layout exactly, minus GUID/UDI/Deploy rows:
`# Name`, status blockquote, `## Definition` (Setting | Value), `## Properties` (12 columns:
Tab | Tab Sort | Group | Group Sort | Name | Alias | Data Type | Editor | Value Type | Mandatory | Sort | Description),
`## Used by`, `## Dependencies` (Flags: `Exists` / `New in this changeset` / `Missing`). Empty values
are an em dash. Folders mirror the backoffice; relative links; optional index page + `.order` per folder.
`_changesets/<yyyy-mm-dd>-<slug>.md` lists the specs, the apply order and a tick-list, which is what
the user approves and what makes apply resumable.

Apply order: data type folders and plain data types → document type folders → compositions → element
types (leaf first) → Block List / Block Grid data types → collection views → templates (master first)
→ page and data types → allowed children last.

MCP facts verified against `@umbraco-cms/mcp-dev` 17.6.8 and 18.1.7 (package source, not live calls):

- `get-document-type-allowed-at-root` and `search-document-type` (used by the old skill) **do not exist**. Use `get-all-document-types` + `get-document-types-by-id-array` (read `allowedAsRoot`) and `get-document-type-tree-search`.
- `create-document-type` / `create-element-type` accept only `name`, `alias`, `dataTypeId`, `tab`, `group` per property; sorts are array index, mandatory is false, no description, no template. `create-element-type` has no `parentId`.
- So every create needs a **fix-up pass**: `get-document-type-by-id` → `update-document-type` with the full body (sorts, mandatory, descriptions, templates, culture), plus `move-document-type` for elements. Always read, modify, write, then verify.
- `apply-via-mcp.md` tells the agent to trust the connected tool list over the reference and names the versions checked.
- `get-icons` returns names only; colour suffixes cannot be validated.

## Shared repo changes (Phase 1)

- `.mcp.json`: tool collections become `document,media,document-type,data-type,template,language`.
- `.claude/skills/umbraco-skill-author/references/conformance-checklist.md` and `authoring-steps.md`: wording for Documented-only, markdown-asset skills validated by the spec linter.
- `scripts/check-skill-links.mjs` (Node, new): resolves every relative markdown link under `plugins/`.
- `.github/workflows/validate-skills.yml`: two steps, the link checker and `lint-spec.mjs` against the golden spec plus the broken fixture (which must fail).
- `CLAUDE.md` one paragraph; `README.md` structure line. (README's dangling `AGENTS.md` link is out of scope; note only.)
- **No new Python.** `scripts/generate-examples.py` is untouched; it only walks `examples/*/.generate.json`, which these skills do not have.

`lint-spec.mjs` checks: required headings, Definition rows, 12-column header, valid status line,
alias casing and suffix by folder, tab sorts against the global table, elements with no tab and one
`Content` group, compositions with no template/children/root/nested compositions, routable pages
with a template named after the alias, links resolve, no leftover `<Placeholder>` tokens; sort
multiples of 100 as a warning.

## Phases

Step 0, once, before Phase 1: `git checkout main`, `git pull`, `git checkout -b feature/content-modelling-skills`.
(The repo has no `develop`.) All phases are worked on that one branch, with **one local commit per phase**.

| Phase | Contents |
|---|---|
| 1 | Skills 1 and 2, linter, golden spec, link checker, CI steps, `.mcp.json`, checklist wording, CLAUDE.md/README |
| 2 | Skill 3 |
| 3 | Skill 4 |
| 4 | Skill 5 |
| 5 | Skill 6 |
| 6 | Skills 7 and 8 |
| 7 | Skills 9 and 10 |
| 8 | Skill 11 |

After Phase 8:

1. Bump the content-modelling plugin once, 1.0.0 to 1.1.0, in
   `plugins/content-modelling/.claude-plugin/plugin.json` and the matching entry in
   `.claude-plugin/marketplace.json` together, and commit.
2. The user pushes the branch to the fork and merges it into the fork's `main` (a PR on the fork
   runs the CI checks). Target the fork explicitly: `gh repo set-default prjseal/Umbraco-CMS-Implementation-Skills`
   or `--repo prjseal/Umbraco-CMS-Implementation-Skills`, because `gh` on a fork can default to the parent.
3. Later, the user opens one upstream PR: add an `upstream` remote for
   `umbraco/Umbraco-CMS-Implementation-Skills`, cut a branch from freshly pulled `upstream/main`, bring
   the content-modelling paths across, `gh pr create --repo umbraco/Umbraco-CMS-Implementation-Skills --head prjseal:<branch>`.
   Each phase owns its own skill folders, so it can still be split by path if upstream prefers smaller PRs.

## Verification (per phase, all local)

1. `node scripts/check-skill-links.mjs` passes.
2. `node plugins/content-modelling/skills/umbraco-content-model-spec/scripts/lint-spec.mjs <golden-spec>` passes and the broken fixture fails (proven able to fail).
3. Self-audit each new skill against `.claude/skills/umbraco-skill-author/references/conformance-checklist.md`.
4. `evals/evals.json` per skill: three prompts, four to seven objective expectations, one MCP-connected and one MCP-absent, each with a trap and a build-honesty expectation. Run `umbraco-skill-evaluator`; clean up its workspace.
5. Phase 1 only, manual runtime proof of the fix-up pass on the blank host: `dotnet run --project Umbraco-CMS.Skills.Blank --urls https://localhost:44372`, create the API user with `.claude/skills/umbraco-reference-instance/scripts/create-api-user.mjs`, apply the golden spec through the MCP, read back with `get-document-type-by-id`. Record what was and was not verified. (The `umbraco` MCP failed to connect in the planning session, so this is unproven.)
6. The phase is committed locally on `feature/content-modelling-skills`; `git status` is clean afterwards and nothing is pushed.

## Risks

- The full-body `update-document-type` can drop properties if the body is wrong; the read-modify-write-verify rule and the Phase 1 runtime proof exist to catch this.
- MCP tool names drift between versions; references must defer to the connected tool list.
- Eight phases of uncommitted work on one branch is a large unsaved working tree; splitting it into PRs later is by path, which works because each phase owns its own skill folders (Phase 1 owns all shared files).
