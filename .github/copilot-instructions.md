# Copilot Instructions for TODO Application

## Project Context

This is a full-stack TODO application with:
- **Frontend**: React application with modern UI components
- **Backend**: Express.js REST API
- **Development Focus**: Iterative, feedback-driven development approach
- **Current Phase**: Backend stabilization and frontend feature completion

## Documentation References

Consult these documentation files to understand the project structure and patterns:

- [docs/project-overview.md](../docs/project-overview.md) - Architecture, tech stack, and project structure
- [docs/testing-guidelines.md](../docs/testing-guidelines.md) - Test patterns and testing standards
- [docs/workflow-patterns.md](../docs/workflow-patterns.md) - Development workflow guidance

## Development Principles

Follow these core principles throughout development:

- **Test-Driven Development**: Implement the Red-Green-Refactor cycle consistently
- **Incremental Changes**: Make small, testable modifications rather than large rewrites
- **Systematic Debugging**: Use test failures as guides for understanding and fixing issues
- **Validation Before Commit**: Ensure all tests pass and no lint errors exist before committing

## Testing Scope

This project uses **unit tests and integration tests ONLY**:

### Testing Tools
- **Backend**: Jest + Supertest for API testing
- **Frontend**: React Testing Library for component unit/integration tests
- **Manual Testing**: Browser testing for full UI verification

### Important Testing Constraints
- **DO NOT** suggest or implement e2e test frameworks (Playwright, Cypress, Selenium)
- **DO NOT** suggest browser automation tools
- **Reason**: Keep lab focused on unit/integration tests without e2e complexity

### Testing Approach by Context

**Backend API Changes**:
- Write Jest tests FIRST, then implement (RED-GREEN-REFACTOR)
- Use Supertest for API endpoint testing
- Test error cases and edge conditions

**Frontend Component Features**:
- Write React Testing Library tests FIRST for component behavior
- Then implement the component logic (RED-GREEN-REFACTOR)
- Follow with manual browser testing for full UI flows

**This is true TDD**: Write the test first, watch it fail, then write code to pass the test.

## Workflow Patterns

Follow these structured workflows for different development tasks:

### 1. TDD Workflow
1. Write or fix tests for the desired functionality
2. Run tests and observe failures (RED)
3. Implement minimal code to make tests pass (GREEN)
4. Refactor code while keeping tests green (REFACTOR)
5. Validate all tests still pass

### 2. Code Quality Workflow
1. Run lint command to identify issues
2. Categorize issues by type and severity
3. Fix issues systematically (one category at a time)
4. Re-validate with lint after each fix batch
5. Commit once all issues are resolved

### 3. Integration Workflow
1. Identify the integration issue or requirement
2. Debug using existing tests or create new ones
3. Test the fix in isolation
4. Fix the implementation
5. Verify end-to-end functionality manually if needed

## Agent Usage

Use specialized agents for specific development tasks:

### tdd-developer Agent
- **When to use**: Test-related work and Red-Green-Refactor cycles
- **Expertise**: Writing tests first, implementing features to pass tests, refactoring
- **Usage**: Request this agent when working on new features or fixing bugs with TDD approach

### code-reviewer Agent
- **When to use**: Addressing lint errors and code quality improvements
- **Expertise**: Code quality, best practices, systematic issue resolution
- **Usage**: Request this agent for lint fixes, code reviews, or quality improvements

## Memory System

This project uses a two-tier memory system for tracking development discoveries and patterns:

### Persistent Memory
- **Location**: This file (`.github/copilot-instructions.md`)
- **Purpose**: Foundational principles, workflows, and established guidelines
- **Content**: Core development practices that rarely change
- **Examples**: TDD workflow, commit conventions, testing scope, agent usage

### Working Memory
- **Location**: `.github/memory/` directory
- **Purpose**: Track discoveries, patterns, and decisions made during active development
- **Structure**:
  - `README.md` - Explains the memory system and how to use it
  - `session-notes.md` - Historical summaries of completed sessions (committed)
  - `patterns-discovered.md` - Accumulated code patterns and architectural decisions (committed)
  - `scratch/working-notes.md` - Active session notes for work-in-progress (NOT committed)

### How to Use the Memory System

**During Active Development**:
- Take notes in `.github/memory/scratch/working-notes.md` as you work
- Document findings, decisions, and blockers in real-time
- Use it as your scratchpad for TDD cycles, debugging, and lint fixing

**At End of Session**:
- Summarize key accomplishments in `.github/memory/session-notes.md`
- Extract recurring patterns to `.github/memory/patterns-discovered.md`
- Clear or archive working notes for the next session

**When Providing Context to AI**:
- Reference `patterns-discovered.md` for established code patterns
- Share `session-notes.md` for recent project history
- Mention `working-notes.md` for immediate context on current work

**Benefits**:
- AI assistants read these files to provide context-aware suggestions
- Patterns are documented once and reused consistently
- Session history creates a searchable record of project evolution
- Working notes stay out of git history while preserving key insights

See [.github/memory/README.md](.github/memory/README.md) for comprehensive usage instructions.

## Workflow Utilities

### GitHub CLI Commands

Use these commands for workflow automation (available in all modes):

```bash
# List all open issues
gh issue list --state open

# View specific issue details
gh issue view <issue-number>

# View issue with all comments
gh issue view <issue-number> --comments
```

### Understanding Exercise Structure
- The main exercise issue will have "Exercise:" in the title
- Development steps are posted as comments on the main issue
- Use these commands when `/execute-step` or `/validate-step` prompts are invoked

## Git Workflow

### Conventional Commits

Use conventional commit format for all commits:

- `feat:` - New features
- `fix:` - Bug fixes
- `chore:` - Maintenance tasks (dependencies, configs)
- `docs:` - Documentation changes
- `test:` - Test additions or modifications
- `refactor:` - Code refactoring without feature changes
- `style:` - Code style changes (formatting, linting)

**Example**: `feat: add delete functionality to todo items`

### Branch Strategy

- **Feature branches**: `feature/<descriptive-name>`
- **Bug fixes**: `fix/<descriptive-name>`
- Create branches from main for new work

### Git Best Practices

1. **Always stage all changes before committing**:
   ```bash
   git add .
   ```

2. **Write clear commit messages**:
   ```bash
   git commit -m "feat: descriptive message about the change"
   ```

3. **Push to the correct branch**:
   ```bash
   git push origin <branch-name>
   ```

4. **Keep commits focused**: Each commit should represent one logical change
