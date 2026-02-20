# Sync

Synchronize a derived package with its template repository.

## Philosophy

Template repositories provide shared infrastructure—agents, skills, CI/CD, tooling configs. When the template evolves, derived packages should have a clear path to adopt improvements without losing their customizations.

This skill:
- Identifies drift between local files and the template
- Distinguishes infrastructure (syncable) from customizations (skip)
- Presents deviations for user decision
- Applies updates selectively

## Execution

1. Read `checklist.md` in this skill directory
2. Verify template origin configuration
3. Fetch current template state from GitHub
4. Compare syncable files against local versions
5. Present deviations grouped by category
6. Prompt user for decisions per-file or per-category
7. Apply selected updates

## Specifications

### Template Origin

The template repository is identified by a `.template-origin` file in the repo root:

```
zoobzio/samoa
```

If this file doesn't exist, prompt the user to create it with the template repo identifier.

### Comparison Method

For each syncable file:
1. Fetch template version via `gh api` or raw content
2. Compare against local version
3. Classify: `identical`, `modified`, `missing locally`, `missing in template`

### User Decisions

For each deviation, offer:
- **Update** — Replace local with template version
- **Skip** — Keep local version unchanged
- **Diff** — Show full diff before deciding
- **Skip All in Category** — Skip remaining files in this category

### Conflict Handling

If local file has meaningful changes the user wants to preserve:
- Show the diff
- Offer manual merge (user handles it)
- Or skip and document for later

## Prohibitions

DO NOT:
- Auto-update without user confirmation
- Modify files in the skip list
- Overwrite local changes without presenting them first
- Assume template location without checking `.template-origin`

## Output

A sync report containing:
- Files checked
- Files updated
- Files skipped (with reason)
- Files requiring manual attention
