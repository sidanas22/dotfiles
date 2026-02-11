---
name: smart-commit
description: Automatically generate a concise headline for staged changes and commit them
disable-model-invocation: true
---

# Smart Commit

Automatically analyzes staged changes, generates a conventional commit message (headline + brief description), and commits the changes.

## Process

### 1. Check Staged Changes
- Run `git status` and `git diff --cached` to identify all staged modifications, additions, and deletions.
- If no changes are staged, inform the user and stop.

### 2. Analyze Changes
- Analyze the nature of the changes (e.g., new feature, bug fix, refactoring, documentation).
- Identify key components affected and the core purpose of the changes.

### 3. Generate Commit Message
- **Headline**: Generate a single-line headline.
  - Format: `<type>: <description>` (e.g., `feat: implement user auth`).
  - Keep it under 72 characters.
- **Description**: Generate a brief, bulleted description of the key changes.
  - Focus on *what* was changed and *why*.
  - Keep it concise (2-4 high-level bullet points).

### 4. Confirm with User
- Present the generated commit message (headline and description) to the user.
- Ask: "Would you like to commit with this message?"

### 5. Execute Commit
- Upon confirmation, run `git commit -m "<headline>" -m "<description>"`.
- Report success and show the commit hash.

## Implementation Notes
- **Always** read the diff carefully before generating the headline.
- Adhere to the project's commit message conventions (e.g., using `feat:`, `fix:`, `refactor:`, `docs:` prefixes).
- If untracked files are relevant but not staged, mention them to the user but do NOT stage them automatically.
