# AGENTS.md

## Project Overview

This repository contains the Payfonte Mintlify documentation site.

- `en/` contains English documentation pages.
- `fr/` contains French documentation pages.
- `api-reference/` contains shared OpenAPI assets.
- `docs.json` defines navigation, language configuration, branding, and Mintlify behavior.
- `images/` and `logo/` contain shared static assets.

## Working Guidelines

1. Keep English and French documentation structurally aligned.
   If a page exists in both languages, preserve the same section order, tabs, cards, accordions, tables, anchors, and overall information architecture unless the user explicitly asks for a language-specific deviation.

2. Preserve Mintlify-compatible MDX.
   Do not introduce unsupported JSX patterns or custom components unless they already exist in the repo and are known to work with Mintlify.

3. Preserve stable anchors and links.
   When changing headings, accordions, or jump links, make sure existing in-page links still resolve or are intentionally updated in both languages.

4. Treat `docs.json` as the navigation source of truth.
   If a new page is added, renamed, or moved, update navigation entries for the relevant language(s).

5. Prefer content edits over broad rewrites.
   Keep terminology, product naming, provider slugs, currencies, and API field names consistent with existing Payfonte docs.

6. Shared technical values must stay identical across languages.
   Do not translate provider slugs, endpoint paths, currencies, code samples, field names, or machine-readable identifiers.

7. Use localized prose, not localized structure.
   Translate explanatory text into French, but keep tables, limits, counts, and provider coverage synchronized with the English source unless the user explicitly requests otherwise.

## Validation Guidelines

1. For page edits, verify internal links, jump links, and heading anchors still match the rendered section names.
2. For layout-heavy changes, use `mintlify dev` from the repo root when practical to visually confirm rendering.
3. When counts or provider coverage change, update all summary sections and detailed sections in the same task.

## Required Translation Rule

After completing any documentation task in one language, immediately update the corresponding page in the other language before considering the task done, unless the user explicitly says not to translate.

This is mandatory for all paired pages under `en/` and `fr/`.

## Completion Checklist

Before finishing a task, confirm:

- the edited page is correct in the primary language
- the matching translated page has been updated
- anchors and jump links still work
- counts, tables, and provider coverage are internally consistent
