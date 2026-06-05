---
description: "Validate that all success criteria for the current step are met"
agent: "code-reviewer"
tools: ['search', 'read', 'execute', 'web', 'todo']
---

# Validate Step Success Criteria

You are now operating as the **code-reviewer** agent to systematically validate step completion.

## Task

Check that all success criteria for a specific exercise step are met in the current workspace.

## Input

- **Step Number** (REQUIRED): ${input:step-number:Step number to validate (e.g., 5-0, 5-1, 5-2)}

## Instructions

### 1. Validate Step Number Format

The step number must be provided in format: `X-Y` (e.g., `5-0`, `5-1`, `5-2`)

If not provided or invalid format:
- Stop and ask the user to provide the step number
- Explain the format: "Step numbers are in format X-Y, like 5-0 or 5-1"

### 2. Find the Exercise Issue

Use gh CLI to find the main exercise issue:

```bash
gh issue list --state open
```

Look for the issue with "Exercise:" in the title. This contains all step instructions.

### 3. Get Issue Content with Comments

Run:
```bash
gh issue view <issue-number> --comments
```

This retrieves:
- The main issue body
- All comments containing step instructions

### 4. Locate the Specific Step

Search through the issue content for:
```
# Step ${step-number}:
```

For example, if validating step `5-1`, search for `# Step 5-1:`

Extract the entire step section, including:
- Step title and description
- Activities (what should have been done)
- **Success Criteria** section

### 5. Extract Success Criteria

Find the section that starts with one of:
- `### Success Criteria`
- `## Success Criteria`
- `**Success Criteria:**`

This section lists the requirements that must be met for the step to be considered complete.

Common success criteria patterns:
- ✅ Tests passing (specific count or "all tests pass")
- ✅ Files created or modified
- ✅ Specific functionality working
- ✅ Code following patterns/conventions
- ✅ No lint errors
- ✅ Specific API endpoints responding correctly

### 6. Validate Each Criterion Systematically

For each criterion in the success criteria:

**Use the manage_todo_list tool to track validation:**
```
1. Check criterion 1: [description]
2. Check criterion 2: [description]
3. Check criterion 3: [description]
...
```

**Common validation checks:**

**For "tests passing":**
```bash
npm test
# or for specific package
npm test --workspace=backend
npm test --workspace=frontend
```
Check output for pass/fail counts

**For "files exist":**
```bash
ls -la path/to/file.js
```
or use file search tools

**For "no lint errors":**
```bash
npm run lint
# or
npm run lint --workspace=backend
npm run lint --workspace=frontend
```

**For "API endpoints work":**
- Check test files for endpoint tests
- Verify tests are passing
- Or manually test with curl/Postman if specified

**For "code follows patterns":**
- Read relevant files
- Check against project patterns in `.github/memory/patterns-discovered.md`
- Verify conventions from docs

### 7. Report Validation Results

For each criterion, report:

**If criterion is met:**
```
✅ [Criterion description]
   Evidence: [What you found that proves it's met]
```

**If criterion is NOT met:**
```
❌ [Criterion description]
   Issue: [What's wrong or missing]
   Fix: [Specific guidance on how to fix]
```

**If criterion is partially met:**
```
⚠️ [Criterion description]
   Status: [What's complete and what's not]
   Remaining: [What still needs to be done]
```

### 8. Provide Overall Status

After checking all criteria:

**If all criteria met:**
```
✅ Step ${step-number} - ALL SUCCESS CRITERIA MET!

All requirements satisfied:
- [List of met criteria]

The step is complete. You can proceed to:
1. Run /commit-and-push <branch-name> if not yet committed
2. Continue to the next step
```

**If some criteria not met:**
```
⚠️ Step ${step-number} - INCOMPLETE

Met (X/Y):
- [List of met criteria]

Not Met:
- [List with specific guidance for each]

Please address the remaining items and run /validate-step again.
```

**If no criteria met:**
```
❌ Step ${step-number} - NOT COMPLETE

None of the success criteria are met yet.

Required:
- [List all criteria with guidance]

Recommendation:
Run /execute-step to implement the step requirements.
```

### 9. Code Quality Check (As Code Reviewer)

In addition to checking explicit success criteria, perform a quick code quality check:

**Check for:**
- Lint errors: Run `npm run lint`
- Code patterns: Match project conventions
- Test quality: Tests are well-written and meaningful
- Code cleanliness: No debug logs, commented code, or obvious issues

**Report any quality issues:**
```
📋 Code Quality Notes:

✓ No lint errors
✓ Tests are well-structured
⚠️ Found 2 console.log statements (suggest removing)
✓ Code follows project patterns
```

### 10. Memory System Integration

Document the validation in `.github/memory/scratch/working-notes.md`:

```markdown
## Validating Step ${step-number} - [timestamp]

### Success Criteria Check
- ✅ Criterion 1: [description] - PASSED
- ✅ Criterion 2: [description] - PASSED
- ❌ Criterion 3: [description] - FAILED
  - Issue: [what's wrong]
  - Fix needed: [guidance]

### Code Quality Check
- Lint: [status]
- Tests: [count passing/failing]
- Patterns: [conformance]

### Overall Status
[Complete/Incomplete with guidance]
```

## Success Indicators

You've successfully validated the step when:
- ✅ Step number was provided and parsed correctly
- ✅ Exercise issue was found
- ✅ Specific step section was located
- ✅ Success criteria were extracted
- ✅ Each criterion was checked systematically
- ✅ Clear status report provided (all met, some met, none met)
- ✅ Specific guidance given for any unmet criteria
- ✅ Code quality check performed

## Example Workflow

```
User runs: /validate-step 5-1

Agent:
"Validating Step 5-1...

Finding exercise issue...
✓ Found Issue #15: Exercise: TODO Application Development

Locating Step 5-1...
✓ Found: Step 5-1: Implement DELETE endpoint

Success Criteria:
1. Tests exist for DELETE /api/todos/:id endpoint
2. All tests passing (minimum 15 tests)
3. DELETE endpoint returns 204 for valid IDs
4. DELETE endpoint returns 404 for non-existent IDs
5. No lint errors

Checking criteria systematically...

✅ Tests exist for DELETE endpoint
   Evidence: Found describe('DELETE /api/todos/:id') in __tests__/app.test.js

✅ All tests passing
   Evidence: Ran npm test - 16/16 tests passing

✅ DELETE returns 204 for valid IDs
   Evidence: Test 'should delete todo and return 204' passing

✅ DELETE returns 404 for non-existent IDs
   Evidence: Test 'should return 404 for non-existent todo' passing

✅ No lint errors
   Evidence: Ran npm run lint - 0 errors

📋 Code Quality Notes:
✓ No lint errors
✓ Tests are well-structured with clear descriptions
✓ Code follows RESTful patterns
✓ Proper error handling implemented

✅ Step 5-1 - ALL SUCCESS CRITERIA MET!

The step is complete. You can:
1. Run /commit-and-push <branch-name> if not yet committed
2. Continue to the next step
"
```

## Error Handling

### If step not found in issue
```
❌ Could not find Step ${step-number} in the exercise issue.

Available steps found:
- Step 5-0: Setup
- Step 5-1: Implement DELETE
- Step 5-2: Add validation

Please verify the step number and try again.
```

### If success criteria section missing
```
⚠️ Found Step ${step-number} but no explicit success criteria listed.

Based on the step activities, implied criteria:
- [Inferred criteria from activities]

Checking against these...
```

### If issue not found
```
❌ Could not find the exercise issue.

Please ensure:
1. There's an open issue with "Exercise:" in the title
2. You have gh CLI access to the repository

Try running: gh issue list --state open
```

## References

- See `.github/copilot-instructions.md` for Workflow Utilities (gh CLI commands)
- See `docs/testing-guidelines.md` for test validation patterns
- See `.github/memory/patterns-discovered.md` for code patterns to check against

## Agent Mode

This prompt automatically switches you to **code-reviewer** mode to leverage systematic validation and code quality expertise throughout the validation process.
