# Complete an Azure DevOps Pull Request

**Usage:** `pr-complete <pr-id>`

This command completes (merges) an active Azure DevOps PR with:
- Normal merge (no fast-forward) - creates a merge commit
- Source branch is kept (not deleted)

**Steps:**
1. Show active PRs to help identify the PR ID
2. Update the PR status to `completed` using `az repos pr update --id <pr-id> --status completed`

**Example:**
```bash
pr-complete 10480
```

**Note:** This uses the Azure CLI command `az repos pr update --status completed` which triggers an auto-complete merge with the repository's default merge strategy (no fast-forward).
