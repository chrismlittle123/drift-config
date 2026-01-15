# drift-config

Central configuration repository for [repo-drift](https://github.com/chrismlittle123/repo-drift) scans across all `chrismlittle123` repositories.

## What This Does

This repo defines coding standards and runs automated scans to detect:

1. **CI/CD Workflow Drift** - Ensures repos have standard CI workflows
2. **Security Issues** - npm audit, hardcoded secrets, committed .env files
3. **Missing Required Files** - README, LICENSE, .gitignore
4. **Tooling Consistency** - TypeScript, ESLint, Prettier configs
5. **New Files to Review** - Discovers workflow and config files that may need attention

## Scan Schedule

- **Weekly**: Every Monday at 9am UTC
- **Manual**: Trigger anytime via [Actions](../../actions/workflows/drift-scan.yml)

## Results

When drift is detected:
- A GitHub Issue is created/updated with details
- JSON results are saved as workflow artifacts
- The workflow shows a failure status

When all is clean:
- Any open drift issue is automatically closed
- The workflow shows success

## Configuration

See [drift.config.yaml](./drift.config.yaml) for the full configuration.

### Excluded Repos

- `drift-config` (this repo)
- `chrismlittle123` (profile README)
- `*-template` (template repos)

### Enforced Standards

| Check | Severity | Description |
|-------|----------|-------------|
| CI Workflow | High | Must match approved workflow template |
| README | - | Required in all repos |
| LICENSE | - | Required in all repos |
| npm audit | - | No high/critical vulnerabilities |
| No secrets | - | No hardcoded credentials in code |
| TypeScript config | - | Required for Node.js projects |
| ESLint config | - | Required for Node.js projects |
| Prettier config | - | Required for Node.js projects |

## Manual Scan

To scan a specific repo:

1. Go to [Actions](../../actions/workflows/drift-scan.yml)
2. Click "Run workflow"
3. Enter the repo name (or leave empty for all)
4. Click "Run workflow"

## Local Testing

```bash
# Install drift CLI
npm install -g repo-drift

# Scan all repos
drift scan --org chrismlittle123

# Scan specific repo
drift scan --org chrismlittle123 --repo repo-drift

# Output as JSON
drift scan --org chrismlittle123 --json
```

## Setup Requirements

This workflow requires a `DRIFT_GITHUB_TOKEN` secret with:
- `repo` scope (to clone and scan private repos)
- `read:org` scope (to list repos)

Create a [Personal Access Token](https://github.com/settings/tokens) and add it as a repository secret.
