# .github (devaction-labs)

Org-wide GitHub configuration: **reusable workflows**, workflow templates, and
community health files.

## Reusable workflows (`.github/workflows/`)

Call from any private or public repo in the org:

```yaml
jobs:
  gate:
    uses: devaction-labs/.github/.github/workflows/deploy-gate.yml@main
    permissions:
      pull-requests: read
```

| Workflow | Purpose |
| -------- | ------- |
| `deploy-gate.yml` | `proceed=true` only for deploy PRs (`release/*`\|`hotfix/*` + label) or manual/cron |
| `gitflow-branch-name.yml` | Validate PR head branch naming |
| `deploy-label-check.yml` | Require a label (default `deploy`) on PRs |
| `sync-develop.yml` | Merge `main` → `develop` via API after release |
| `tag-release.yml` | Annotated `vX.Y.Z` tag + GitHub Release |

**Pin a ref in production apps** (`@main` while iterating, then tag `@v1` when stable).

### Recommended release efficiency (private runners / minutes)

| Event | What runs |
| ----- | --------- |
| **PR → main** (`release/*` / `hotfix/*` + `deploy`) | Full app CI (test, format, …) |
| **Push → main** (after merge) | **Dialyzer (or similar slow guard) only → deploy**; tag + sync-develop |
| **Do not** re-run the full test suite on merge if required checks already passed on the PR |

Branch protection should require the PR CI checks. Keep PRs up to date with `main`
so the merged tree matches what was tested.

## workflow-templates/

Starter copies for the “New workflow” picker (not auto-updated after copy).
See `workflow-templates/deploy-openship.yml`.
