---
name: smart-jira
description: Automatically analyze work to create Jira tasks/subtasks or log time based on git context and sprint detection.
---

# Smart Jira

Synthesizes local git activity and branch context to manage Jira administrative overhead (Tasks, Sub-tasks, or Worklogs).

## Logic & Context Detection

### 1. Context & Sprint Detection
- **Sprint Identification**: Extract the version/sprint from the current branch name (e.g., `v2.9` -> `XP v2.9`).
- **Activity Analysis**: Prompt the user to specify the git history window to analyze (e.g., "1 day", "3 commits", or "none") to identify the focus of work.
- **Issue Association**: If the branch name contains an ID (e.g., `XP-2612`), default to updating that issue instead of creating a new one.

### 2. Modes of Operation
This command supports three modes based on the user's need or branch state:
- **`--log-only`**: Only add a worklog to the current issue detected in the branch name or a provided ID.
- **`--sub-task`**: Create a sub-task under a parent issue (must provide `--parent` or have a parent issue detected).
- **`--task`** (Default): Create a new Task in the detected sprint.

### 3. Workflow
1. **Analyze**: Parse git commits to draft "Business Logic" bullets.
2. **Draft**: Prepare the Summary, Description, and Time Estimate.
3. **Present**: Display the draft to the user, including the target Sprint and Issue Type.
4. **Confirm**: **ALWAYS** ask: "Should I proceed with these Jira changes?" before executing any write operations.

## Usage Patterns
- `smart-jira`: Full analysis, create Task, and log time.
- `smart-jira --log-only 4h`: Log 4 hours to the issue in the current branch.
- `smart-jira --sub-task --parent XP-2573`: Create sub-task under XP-2573.

## Safety & Accuracy
- **Confirmation**: No Jira issue creation or worklog addition without explicit user approval.
- **Duplicate Prevention**: Check for existing issues with similar summaries in the current sprint before creating.
- **No Jargon**: Focus descriptions on business value (e.g., "Enhanced data recovery" vs "Fixed Redis outbox").
