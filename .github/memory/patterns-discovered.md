# Patterns Discovered

## Purpose
This file catalogs recurring code patterns, architectural decisions, and best practices discovered during development. Use this to maintain consistency and avoid repeating mistakes.

**Note**: This file is committed to git as shared team knowledge.

---

## Pattern Template

Use this template when documenting a new pattern:

```markdown
## [Pattern Name]

**Context**: When/where this pattern applies

**Problem**: What problem does this solve?

**Solution**: How to implement it

**Example**:
\`\`\`javascript
// Code example showing the pattern
\`\`\`

**Related Files**:
- path/to/file1.js
- path/to/file2.js

**Notes**: Additional context or variations
```

---

## Documented Patterns

## Service Initialization - Empty Array vs Null

**Context**: When initializing service data structures that will hold collections

**Problem**: Inconsistent initialization can lead to undefined behavior when methods expect arrays but receive null/undefined values. Some array methods (map, filter, forEach) will throw errors on null.

**Solution**: 
- Initialize collection properties with empty arrays `[]` instead of `null` or leaving undefined
- This allows array methods to work immediately without null checks
- Reduces defensive programming throughout the codebase

**Example**:
```javascript
// ❌ Problematic approach
class TodoService {
  constructor() {
    this.todos = null; // Requires null checks before using array methods
  }
  
  getAll() {
    return this.todos || []; // Defensive coding needed everywhere
  }
}

// ✅ Better approach
class TodoService {
  constructor() {
    this.todos = []; // Safe to use array methods immediately
  }
  
  getAll() {
    return this.todos; // No defensive coding needed
  }
}
```

**Related Files**:
- `packages/backend/src/services/` (any service managing collections)
- `packages/frontend/src/services/` (frontend service layer)

**Notes**: 
- This pattern applies to any in-memory data structure representing a collection
- For optional collections in API responses, empty array is still preferred over null
- Maintains consistency with JavaScript's falsy behavior (empty array is truthy)

---

<!-- Add new patterns below in reverse chronological order (newest first) -->
