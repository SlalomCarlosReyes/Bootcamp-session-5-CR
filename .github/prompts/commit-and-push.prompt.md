---
description: "Analyze changes, generate commit message, and push to feature branch"
tools: ['read', 'execute', 'todo']
---

# Commit and Push Changes

Analyze workspace changes, generate a conventional commit message, and push to a feature branch.

## Input

- **Branch Name** (REQUIRED): ${input:branch-name:Feature branch name (e.g., feature/delete-endpoint)}

## Instructions

### 1. Validate Branch Name

If no branch name is provided:
- Stop and ask the user to provide a branch name
- Explain branch naming conventions:
  - `feature/<descriptive-name>` for new features
  - `fix/<descriptive-name>` for bug fixes
  - Use lowercase with hyphens (e.g., `feature/delete-todo-endpoint`)

### 2. Analyze Changes

Run:
```bash
git status
git diff
```

Review:
- Which files were modified
- What changes were made
- The scope and nature of the changes

### 3. Generate Conventional Commit Message

Based on the changes, create a commit message following the conventional commit format:

**Format:**
```
<type>: <description>

[optional body]

[optional footer]
```

**Types:**
- `feat:` - New features
- `fix:` - Bug fixes
- `test:` - Test additions or modifications
- `refactor:` - Code refactoring without feature changes
- `chore:` - Maintenance tasks (dependencies, configs)
- `docs:` - Documentation changes
- `style:` - Code style changes (formatting, linting)

**Examples:**
- `feat: add delete endpoint for todo items`
- `test: add tests for todo deletion`
- `fix: correct todo validation in update endpoint`
- `refactor: extract todo validation to helper function`

**Guidelines:**
- Use imperative mood ("add" not "added")
- No capitalization after type
- No period at the end
- Keep first line under 72 characters
- Add body if changes need explanation

### 4. Create or Switch to Branch

**If branch does NOT exist:**
```bash
git checkout -b <branch-name>
```

**If branch exists:**
```bash
git checkout <branch-name>
```

**Verify current branch:**
```bash
git branch --show-current
```

**CRITICAL:** Only operate on the user-provided branch name. Never commit to:
- ❌ `main`
- ❌ `master`
- ❌ Any branch other than what the user specified

### 5. Stage All Changes

```bash
git add .
```

**Verify staged changes:**
```bash
git status
```

Show the user what will be committed.

### 6. Commit with Generated Message

```bash
git commit -m "<generated-commit-message>"
```

### 7. Push to Remote Branch

```bash
git push origin <branch-name>
```

If this is the first push to a new branch, git may suggest setting upstream. Use:
```bash
git push -u origin <branch-name>
```

### 8. Confirm Success

After successful push, inform the user:

```
✓ Changes committed and pushed successfully!

Branch: <branch-name>
Commit: <commit-message>

Pushed to: origin/<branch-name>

Next steps:
- Continue with next exercise step
- Or create a pull request when ready
```

## Error Handling

### If uncommitted changes exist in working directory
- Show the user what's uncommitted
- Ask if they want to stash, commit, or abort

### If branch name conflicts with existing branch
- Show current branch content
- Ask user to confirm checkout or choose different name

### If push fails (e.g., remote branch has updates)
- Explain the conflict
- Suggest pulling changes first: `git pull origin <branch-name>`
- Ask user how to proceed

## Git Best Practices

Follow these guidelines from project documentation:

1. **Always stage all changes before committing:**
   ```bash
   git add .
   ```

2. **Write clear commit messages:**
   - Use conventional commit format
   - Be specific and descriptive
   - Each commit should represent one logical change

3. **Push to the correct branch:**
   - Never push directly to main
   - Always use feature branches
   - Verify branch before pushing

4. **Keep commits focused:**
   - Each commit should represent one logical change
   - Don't mix unrelated changes

## Task Tracking

Use the manage_todo_list tool to track the commit workflow:

```
1. Analyze changes with git diff
2. Generate conventional commit message
3. Create/switch to feature branch
4. Stage all changes
5. Commit with generated message
6. Push to remote branch
7. Verify success
```

## Example Workflow

```
User runs: /commit-and-push feature/delete-endpoint

Agent:
"Analyzing changes in workspace...

Files modified:
- packages/backend/src/app.js (added DELETE endpoint)
- packages/backend/__tests__/app.test.js (added delete tests)

Generating commit message...

Type: feat (new feature added)
Message: feat: add delete endpoint for todo items

Creating feature branch...
✓ Switched to new branch 'feature/delete-endpoint'

Staging changes...
✓ All changes staged

Committing...
✓ Committed: feat: add delete endpoint for todo items

Pushing to origin/feature/delete-endpoint...
✓ Successfully pushed

✓ Changes committed and pushed!

Branch: feature/delete-endpoint
Commit: feat: add delete endpoint for todo items
Pushed to: origin/feature/delete-endpoint

Next steps:
- Continue with next exercise step
- Or create a pull request when ready
"
```

## Safety Checks

Before executing git commands:

1. ✅ Verify branch name is provided
2. ✅ Confirm not on main/master branch
3. ✅ Check for uncommitted changes
4. ✅ Verify commit message follows conventions
5. ✅ Show user what will be committed

## References

- See `.github/copilot-instructions.md` for Git Workflow section (conventional commits)
- See `.github/copilot-instructions.md` for Branch Strategy
- See `.github/copilot-instructions.md` for Git Best Practices

## Agent Mode

This prompt works with any currently active agent and does NOT switch agent modes. It inherits the context from whatever agent mode you're currently using.
