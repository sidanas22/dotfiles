# List Active Azure DevOps Pull Requests

**Usage:** `list-active-prs`

This command lists all active (open) pull requests in the current Azure DevOps project.

**Command:**
```bash
az repos pr list --status active --top 5
```

**Example output:**
| ID | Title | Author | Source → Target |
|---|-------|--------|-----------------|
| #10480 | frontend updates | John Bob | v2.9/sprint-v2.9 → development-container |
| #1194 | Business Details | Grace Hopper | feature-branch → main |

**Tip:** Combine with `pr-complete` to merge:
```bash
pr-complete 10480
```
