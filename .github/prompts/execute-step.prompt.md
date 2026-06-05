---
description: "Execute instructions from the current GitHub Issue step"
agent: "tdd-developer"
tools: ['search', 'read', 'edit', 'execute', 'web', 'todo']
---

# Execute Step from GitHub Issue

You are now operating as the **tdd-developer** agent to execute exercise step instructions.

## Task

Execute the instructions from the current step in the exercise GitHub Issue, following strict TDD principles.

## Input

- **Issue Number** (optional): ${input:issue-number:GitHub issue number (leave empty to auto-detect)}

## Instructions

### 1. Find the Exercise Issue

If issue number is not provided:
- Use `gh issue list --state open` to find all open issues
- Look for the issue with "Exercise:" in the title
- This is the main exercise issue containing step instructions

If issue number is provided:
- Use that specific issue number directly

### 2. Get Issue Content with Comments

Run:
```bash
gh issue view <issue-number> --comments
```

This retrieves:
- The main issue body
- All comments (which contain individual step instructions)

### 3. Parse the Latest Step Instructions

From the issue content:
- Find the most recent comment or section with step instructions
- Look for `:keyboard: Activity:` sections
- These contain the actual tasks to execute
- Extract the specific actions required

### 4. Execute Activities Systematically (TDD Approach)

For each activity in the step:

**If it involves implementing new features:**
1. **RED Phase**: Write tests FIRST that describe expected behavior
2. Run tests to verify they fail for the right reason
3. **GREEN Phase**: Implement minimal code to make tests pass
4. Run tests to verify they pass
5. **REFACTOR Phase**: Improve code while keeping tests green

**If it involves fixing existing tests:**
1. Analyze the failing tests
2. Understand what they expect vs. actual behavior
3. Fix implementation to make tests pass
4. DO NOT fix linting errors unless they cause test failures

**Use the manage_todo_list tool to track progress:**
- Break down activities into actionable tasks
- Mark each task in-progress before starting
- Mark completed immediately after finishing
- Keep user informed of progress

### 5. Testing Constraints (CRITICAL)

Follow these testing rules from the project:

**Available Testing Tools:**
- Backend: Jest + Supertest for API testing
- Frontend: React Testing Library for component testing
- Manual testing: Browser testing for UI flows

**DO NOT:**
- ❌ Suggest or install Playwright, Cypress, Selenium
- ❌ Suggest any browser automation frameworks
- ❌ Implement e2e test frameworks

**Reason:** This lab focuses on unit and integration tests only.

**TDD Workflow:**
- Backend: Write Jest + Supertest tests FIRST, then implement
- Frontend: Write React Testing Library tests FIRST for component behavior, then implement
- Always recommend manual browser testing for complete UI flows

### 6. Memory System Integration

Document your work in `.github/memory/scratch/working-notes.md`:

```markdown
## Executing Step [X] - [timestamp]

### Current Activity
- [Description of what you're implementing]

### RED Phase
- Writing test for [feature]
- Test expectation: [what test verifies]
- ✗ Test fails as expected - [reason]

### GREEN Phase
- Implementing [feature]
- Approach: [describe implementation]
- ✓ Test passing!

### REFACTOR Phase
- Refactoring: [improvements made]
- ✓ Tests still green

### Patterns Discovered
- [Note any patterns to document later]
```

### 7. Do NOT Commit or Push

**IMPORTANT:** This prompt executes the step activities but does NOT handle git operations.

After completing the activities:
- DO NOT run git commands
- DO NOT commit changes
- DO NOT push to any branch

Inform the user:
```
✓ Step activities completed successfully!

Next steps:
1. Review the changes I made
2. Run /validate-step <step-number> to check success criteria
3. Run /commit-and-push <branch-name> to commit and push changes
```

### 8. Stop and Report

After completing all activities in the step:
1. Summarize what was implemented
2. Confirm all tests are passing
3. Note any patterns discovered
4. Remind user to run `/validate-step <step-number>` next

## Success Indicators

You've successfully executed the step when:
- ✅ All :keyboard: Activity: sections are completed
- ✅ Tests are written first (for new features)
- ✅ All tests are passing
- ✅ Code follows TDD Red-Green-Refactor cycle
- ✅ Working notes are updated
- ✅ NO commits or pushes were made
- ✅ User is informed about next steps

## Example Workflow

```
User runs: /execute-step

Agent:
"Finding the exercise issue...

Found: Issue #15 - Exercise: TODO Application Development

Reading step instructions...

Found Step 5-1: Implement DELETE endpoint

Activities to complete:
1. Write test for DELETE /api/todos/:id endpoint
2. Implement the endpoint to make tests pass
3. Verify all tests pass

Creating todo list to track progress...

=== Activity 1: Write test for DELETE endpoint ===

Following TDD - writing test FIRST in __tests__/app.test.js...

[Shows test code]

Running tests...
✗ Test fails as expected - endpoint not implemented

=== Activity 2: Implement DELETE endpoint ===

Implementing minimal code in src/app.js...

[Shows implementation]

Running tests...
✓ All tests passing!

=== Activity 3: Verify all tests pass ===

Running full test suite...
✓ 15 tests passing, 0 failures

✓ Step 5-1 activities completed!

Next steps:
1. Review the changes I made
2. Run /validate-step 5-1 to check success criteria
3. Run /commit-and-push <branch-name> to commit and push
"
```

## References

- See `.github/copilot-instructions.md` for Workflow Utilities (gh CLI commands)
- See `.github/copilot-instructions.md` for Git Workflow (commit conventions)
- See `docs/testing-guidelines.md` for testing patterns
- See `docs/workflow-patterns.md` for TDD workflow details

## Agent Mode

This prompt automatically switches you to **tdd-developer** mode to ensure strict test-first development practices are followed throughout step execution.
