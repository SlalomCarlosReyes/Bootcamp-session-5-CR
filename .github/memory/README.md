# Development Memory System

## Purpose

This memory system tracks patterns, decisions, and lessons learned during development. It helps AI assistants provide better context-aware suggestions by understanding what you've discovered, what works, and what doesn't in this specific codebase.

## Memory Types

This project uses two complementary memory systems:

### 1. Persistent Memory (`.github/copilot-instructions.md`)
- **What it contains**: Foundational principles, workflows, and established guidelines
- **When to update**: When establishing new team standards or core development practices
- **Example content**: TDD workflow, commit conventions, testing scope, agent usage patterns
- **Characteristics**: Stable, rarely changes, defines "how we work"

### 2. Working Memory (`.github/memory/`)
- **What it contains**: Discoveries, patterns, decisions made during active development
- **When to update**: Throughout development as you learn and discover
- **Example content**: API patterns found, debugging insights, service initialization quirks
- **Characteristics**: Dynamic, frequently updated, captures "what we learned"

## Directory Structure

```
.github/memory/
├── README.md                     # This file - explains the system
├── session-notes.md              # Historical summaries (COMMITTED)
├── patterns-discovered.md        # Accumulated code patterns (COMMITTED)
└── scratch/
    ├── .gitignore                # Ignores all scratch files
    └── working-notes.md          # Active session notes (NOT COMMITTED)
```

### File Purposes

#### `session-notes.md` (Committed to Git)
**Purpose**: Historical record of completed development sessions

**When to use**:
- At the END of a development session
- After completing a feature, bug fix, or significant task
- When wrapping up work for the day

**What to include**:
- Session name and date
- What was accomplished
- Key findings and decisions made
- Outcomes and results

**Why it's committed**: Provides a searchable history of project evolution for the entire team

---

#### `patterns-discovered.md` (Committed to Git)
**Purpose**: Catalog of recurring code patterns, anti-patterns, and architectural decisions

**When to use**:
- When you discover a pattern that appears multiple times in the codebase
- After making an architectural decision that should be followed consistently
- When you find a solution to a problem that might recur

**What to include**:
- Pattern name and context
- Problem it solves
- Solution with code examples
- Related files where pattern appears

**Why it's committed**: Creates shared knowledge base for consistent code patterns across the team

---

#### `scratch/working-notes.md` (NOT Committed - Ephemeral)
**Purpose**: Active scratchpad for current session work-in-progress

**When to use**:
- DURING active development (not at the end)
- While debugging issues
- When running TDD cycles
- During lint fixing sessions
- While exploring code or testing hypotheses

**What to include**:
- Current task description
- Approach being taken
- Key findings as you discover them
- Decisions made in the moment
- Blockers encountered
- Next steps
- Any rough notes or observations

**Why it's NOT committed**: Keeps git history clean; temporary notes don't belong in permanent history

**Important**: At session end, extract valuable insights and move them to `session-notes.md` or `patterns-discovered.md`

## Using the Memory System in Workflows

### TDD Workflow
1. **Before starting**: Check `patterns-discovered.md` for relevant test patterns
2. **During RED phase**: Note test failures in `scratch/working-notes.md`
3. **During GREEN phase**: Document approach in working notes
4. **After REFACTOR**: If you discover a pattern, add it to `patterns-discovered.md`
5. **End of session**: Summarize the feature work in `session-notes.md`

### Code Quality (Lint Fixing) Workflow
1. **Before starting**: Check `patterns-discovered.md` for code style patterns
2. **During fixing**: Track systematic issues in `scratch/working-notes.md`
3. **If pattern emerges**: Document the pattern in `patterns-discovered.md`
4. **End of session**: Add summary to `session-notes.md` if significant refactoring occurred

### Debugging Workflow
1. **Problem encountered**: Document in `scratch/working-notes.md` immediately
2. **While investigating**: Track hypotheses and test results in working notes
3. **Root cause found**: Document the issue and solution
4. **If it's a pattern**: Add to `patterns-discovered.md` so it's not repeated
5. **End of session**: Summarize debugging session in `session-notes.md`

## How AI Uses This System

When you ask AI assistants (like GitHub Copilot) for help, they will:

1. **Read persistent instructions** from `.github/copilot-instructions.md` for workflow guidance
2. **Check patterns-discovered.md** to understand established code patterns
3. **Review session-notes.md** to understand recent project history
4. **Consider scratch/working-notes.md** (if you share it) for immediate context

This allows AI to provide suggestions that:
- Match your existing code patterns
- Avoid previously identified anti-patterns
- Build on recent decisions
- Understand the current state of development

## Best Practices

### DO:
✅ Update `scratch/working-notes.md` frequently during active work
✅ Extract key insights into `session-notes.md` at end of session
✅ Document patterns when they appear 2+ times
✅ Keep pattern examples concise but complete
✅ Date your session summaries
✅ Include file paths in pattern documentation
✅ Clear or archive old working notes after extracting insights

### DON'T:
❌ Commit `scratch/` directory contents to git
❌ Put sensitive data (passwords, keys) in any memory files
❌ Write novels - keep entries concise
❌ Duplicate information between files
❌ Let working notes accumulate without summarizing
❌ Document one-off fixes as patterns

## Example Workflow

**Monday Morning - Starting Feature Work**:
1. Open `scratch/working-notes.md`
2. Document current task: "Add delete functionality"
3. Note approach: "TDD - write tests first"

**Throughout the Day**:
- Update working notes with findings
- Add code snippets of challenges faced
- Document decisions made

**Monday Evening - Wrapping Up**:
1. Review `scratch/working-notes.md`
2. Extract key accomplishments → Add to `session-notes.md`
3. If you found a pattern → Add to `patterns-discovered.md`
4. Clear or archive working notes for next session

## Getting Started

1. Use `scratch/working-notes.md` as your daily scratchpad
2. At the end of each significant session, add a summary to `session-notes.md`
3. When you notice a pattern, document it in `patterns-discovered.md`
4. Reference these files when asking AI for help to provide better context

The memory system works best when used consistently - make it part of your daily development rhythm!
