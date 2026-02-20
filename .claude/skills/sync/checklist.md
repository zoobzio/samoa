# Sync Checklist

## Syncable Files

These files should be checked for drift and offered for update:

### Agents
- `.claude/agents/zidgel.md`
- `.claude/agents/fidgel.md`
- `.claude/agents/midgel.md`
- `.claude/agents/kevin.md`

### Skills
- `.claude/skills/*` (all skill directories)

### Workflow & Philosophy
- `.claude/PHILOSOPHY.md`
- `.claude/STANDING-ORDERS.md`

### Build & Tooling
- `Makefile`
- `.golangci.yml`
- `.codecov.yml`
- `.goreleaser.yml`

### CI/CD
- `.github/workflows/*`

### Test Infrastructure
- `testing/helpers.go`

## Skip List

These files are package-specific and should NOT be synced:

- `.claude/MISSION.md` — Package purpose
- `README.md` — Package documentation
- `CONTRIBUTING.md` — May be customized
- `go.mod`, `go.sum` — Package identity
- `api.go` — Package source code
- `docs/*` — Package documentation
- `*.go` (except `testing/helpers.go`) — Package implementation
- `*_test.go` (except `testing/helpers_test.go`) — Package tests

## Phase 1: Verify Template Origin

### Check Configuration
- [ ] Look for `.template-origin` file in repo root
- [ ] If missing, prompt user: "What is the template repository? (e.g., `zoobzio/samoa`)"
- [ ] Create `.template-origin` with user's response if needed

### Validate Access
- [ ] Verify template repo exists: `gh repo view [owner/repo]`
- [ ] If access denied, report and stop

## Phase 2: Fetch Template State

### Get Template Files
- [ ] Fetch template default branch: `gh api repos/[owner/repo] --jq '.default_branch'`
- [ ] For each syncable file, fetch content via:
  ```
  gh api repos/[owner/repo]/contents/[path] --jq '.content' | base64 -d
  ```

### Handle Missing Files
- [ ] If file exists in template but not locally: mark as `missing locally`
- [ ] If file exists locally but not in template: mark as `local only`

## Phase 3: Compare Files

### For Each Syncable File
- [ ] Fetch template version
- [ ] Read local version
- [ ] Compare content

### Classify Deviation
- `identical` — No changes needed
- `modified` — Local differs from template
- `missing locally` — Template has file, local doesn't
- `missing in template` — Local has file, template doesn't (new local addition)

### Build Deviation Report
Group by category:
- Agents
- Skills
- Workflow & Philosophy
- Build & Tooling
- CI/CD
- Test Infrastructure

## Phase 4: Present Deviations

### Summary First
Present count per category:
```
Sync Report:
- Agents: 0 identical, 1 modified, 0 missing
- Skills: 18 identical, 2 modified, 1 missing locally
- Build & Tooling: 4 identical, 0 modified
...
```

### Detail Modified Files
For each modified file, show:
- File path
- Brief change summary (lines added/removed)
- Option to view full diff

## Phase 5: User Decisions

### Per-Category or Per-File
Ask user preference:
- [ ] "Review each file individually?"
- [ ] "Or decide per category?"

### For Each Deviation
Present options:
1. **Update** — Replace local with template
2. **Skip** — Keep local version
3. **Diff** — Show full diff, then ask again
4. **Skip Category** — Skip all remaining in this category

### Record Decisions
Track:
- Files to update
- Files skipped (with reason if given)
- Files needing manual merge

## Phase 6: Apply Updates

### Backup First (Optional)
- [ ] Ask: "Create backup branch before applying? (y/n)"
- [ ] If yes: `git checkout -b sync-backup-[timestamp]` then return

### Apply Changes
For each file marked for update:
- [ ] Write template content to local file
- [ ] Report: `Updated: [path]`

### Report Skipped
- [ ] List files skipped and why

### Report Manual
- [ ] List files requiring manual attention

## Phase 7: Summary

### Final Report
```markdown
## Sync Complete

### Updated (N files)
- `.claude/skills/sync/SKILL.md`
- `.github/workflows/ci.yml`

### Skipped (N files)
- `.claude/agents/kevin.md` — User chose to skip
- `Makefile` — Local modifications preserved

### Manual Attention (N files)
- `.golangci.yml` — Conflicts detected, manual merge needed

### No Changes (N files)
- [Count] files already in sync
```

### Next Steps
- [ ] Review changes: `git diff`
- [ ] Commit if satisfied: `/commit` with message "chore: sync with template"
- [ ] Or revert: `git checkout .`
