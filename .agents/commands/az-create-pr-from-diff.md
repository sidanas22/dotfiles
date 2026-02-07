---
name: az-create-pr-from-diff
description: Create an Azure DevOps PR from branch diff with auto-generated title and description
disable-model-invocation: true
---

# Create PR from Branch Diff

Automatically creates an Azure DevOps pull request with a title and description generated from the diff between source and target branches.

## Usage

```
/create-pr-from-diff <source-branch> <target-branch>
```

## Arguments

- `source-branch`: The branch containing the changes (e.g., `v2.9/feature/static-creative-rnd`)
- `target-branch`: The branch to merge into (e.g., `v2.9/sprint-v2.9`)

## Process

### 1. Confirm branches with user
- Ask user to confirm the source branch (where changes are)
- Ask user to confirm the target branch (where to merge into)
- Only proceed after explicit confirmation

### 2. Validate branches
- Check that both branches exist on remote
- Ensure the source branch has commits ahead of target

### 3. Generate PR content
- Get diff statistics: `git diff <target-branch>..<source-branch> --stat`
- Analyze which files changed (focus on key areas: .claude/, protos, schema, etc.)
- Generate a descriptive PR title based on the changes
- Generate a detailed PR description with:
  - Summary of changes
  - Sections for new features/documentation
  - Organized by change type

### 4. Create and format PR
- Use `az repos pr create` to create the PR
- Extract PR ID from response
- Use `az repos pr update` to set properly formatted description with actual newlines
- Return PR URL

### 5. Finding Reviewers (Optional)
If the user wants to add reviewers, use these commands to find available users:

**Get Contributors:**
```bash
az devops security group list --project <project-name> --output table
# Find the Contributors group descriptor, then:
az devops security group membership list --id <contributors-descriptor> --output table
```

**Get Project Administrators:**
```bash
# Use the Project Administrators descriptor from the group list
az devops security group membership list --id <project-admins-descriptor> --output table
```

**Get Team Members:**
```bash
az devops team list-member --team "<team-name>" --project <project-name> --output table
```

**Example workflow:**
```bash
# 1. First, list all security groups to get descriptors
az devops security group list --project <project-name> --output table

# 2. Then get members from specific groups
# Contributors
az devops security group membership list --id "CONTRIBUTORS_DESCRIPTOR_ID_HERE" --output table

# Project Administrators
az devops security group membership list --id "PROJECT_ADMINS_DESCRIPTOR_ID_HERE" --output table
```

## Examples

### PR Creation Flow
```bash
# User runs
/create-pr-from-diff v2.9/feature/static-creative-rnd v2.9/sprint-v2.9

# System:
# 1. Checks diff statistics
# 2. Identifies changed files (.claude/skills/, CLAUDE.md, db/ddl/)
# 3. Generates title: "Add Claude Skills - Database & gRPC Development Documentation"
# 4. Creates PR with initial description
# 5. Updates with formatted description containing proper line breaks
# 6. Returns: PR #10429 URL with formatted description
```

## Implementation Notes

- **ALWAYS confirm source and target branches with user before proceeding**
- Always use `git diff --stat` to understand what changed
- Categorize changes (docs, features, fixes, schema, etc.)
- Make PR titles descriptive and action-oriented
- Use proper markdown formatting with actual newlines in descriptions
- Handle special characters in descriptions properly
- Display the branches to confirm before any git operations
- If user asks about reviewers, use the commands in section 5 to find available Contributors or Project Administrators
- Note: Reviewers may have different roles (Contributors, Project Admins, Build Admins) - check appropriate groups
