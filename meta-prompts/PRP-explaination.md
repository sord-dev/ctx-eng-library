# PRP (Product Requirements Prompt) - Complete Guide

## What is a PRP?

A PRP is like a PRD (Product Requirements Document) but specifically crafted to give AI coding assistants ALL the context they need to implement features successfully in one pass. It's the core of "Context Engineering" - the practice of engineering comprehensive context rather than relying on prompt tricks.

## Why PRPs Work Better Than Regular Prompts

**Traditional Prompting:**
- "Build me a web scraper" 
- AI has to guess patterns, dependencies, error handling
- Back-and-forth clarification cycles
- Often produces code that doesn't match your codebase style

**Context Engineering with PRPs:**
- Comprehensive blueprint with all context upfront
- AI follows your exact patterns and conventions  
- Includes validation loops so AI can test and fix its own code
- One-pass implementation that actually works

## PRP Structure Template

```markdown
# [Feature Name] - Product Requirements Prompt

name: "[Descriptive Name with Key Tech Stack]"
description: |
  Brief overview of what you're building and why it's better than alternatives

## Purpose
What this solves and why it matters

## Core Principles
1. **Context is King**: Include ALL necessary documentation, examples, and caveats
2. **Validation Loops**: Provide executable tests/lints the AI can run and fix
3. **Information Dense**: Use keywords and patterns from the codebase
4. **Progressive Success**: Start simple, validate, then enhance
5. **Global rules**: Be sure to follow all rules in CLAUDE.md (if you have one)

---

## Goal
[Specific, measurable end state - not vague]

## Why
- **Business value**: [What problem this solves]
- **Integration**: [How it fits with existing systems]
- **Problems solved**: [Specific pain points addressed]
- **User impact**: [Who benefits and how]

## What
[User-visible behavior and technical requirements]

### Success Criteria
- [ ] [Specific measurable outcomes]
- [ ] [Each checkbox is testable]
- [ ] [Include performance/quality metrics]

## All Needed Context

### Documentation & References
```yaml
# MUST READ - Include these in your context window
- url: [Official API docs URL]
  why: [Specific sections/methods you'll need]
  
- url: [Library documentation URL]
  section: [Specific section about common pitfalls]
  critical: [Key insight that prevents common errors]
  
- file: [path/to/example.py] 
  why: [Pattern to follow, gotchas to avoid]

- docfile: [path/to/internal/docs.md]
  why: [Internal docs the user pasted into project]
```

### Current Codebase tree
```bash
# Run `tree` in project root - shows AI what exists
project/
├── src/
│   ├── existing_module.py
│   └── patterns_to_follow.py
├── tests/
│   └── existing_tests.py
└── requirements.txt
```

### Desired Codebase tree with files to be added
```bash  
project/
├── src/
│   ├── existing_module.py
│   ├── new_feature.py          # Main implementation
│   └── utils.py               # Helper functions
├── tests/
│   ├── existing_tests.py
│   └── test_new_feature.py    # Comprehensive tests
├── requirements.txt           # Updated dependencies
└── README.md                  # Updated docs
```

### Known Gotchas & Library Quirks
```python
# CRITICAL: [Library name] requires [specific setup]
# Example: FastAPI requires async functions for endpoints
# Example: This ORM doesn't support batch inserts over 1000 records  
# Example: We use pydantic v2 syntax, not v1
# GOTCHA: [Common mistake and how to avoid it]
# PATTERN: [How we do things in this codebase]
```

## Implementation Blueprint

### Data models and structure
```python
# Show the exact models/classes needed
from pydantic import BaseModel

class ExampleModel(BaseModel):
    field: str
    another_field: int = 0
    
# Include validation patterns, inheritance hierarchy, etc
```

### List of tasks to be completed
```yaml
Task 1: [Clear, specific task]
CREATE src/new_file.py:
  - PATTERN: Follow [specific example file] structure
  - MODIFY [specific aspect] 
  - PRESERVE [important existing behavior]

MODIFY src/existing_file.py:
  - FIND pattern: "[specific code pattern to locate]"
  - INJECT after line containing "[specific landmark]"
  - PRESERVE existing method signatures

Task 2: [Next logical task]
...

Task N: [Final task]
```

### Per task pseudocode
```python
# Task 1 - Don't write full code, show critical logic
async def new_feature(param: str) -> Result:
    # PATTERN: Always validate input first (see src/validators.py)
    validated = validate_input(param)  # raises ValidationError
    
    # GOTCHA: This library requires connection pooling
    async with get_connection() as conn:  # see src/db/pool.py
        # CRITICAL: API returns 429 if >10 req/sec
        await rate_limiter.acquire()
        result = await external_api.call(validated)
    
    # PATTERN: Standardized response format
    return format_response(result)  # see src/utils/responses.py
```

### Integration Points
```yaml
ENVIRONMENT:
  - add to: .env
  - vars: |
      API_KEY=your_key_here
      TIMEOUT=30

CONFIG:
  - add to: config/settings.py
  - pattern: "FEATURE_ENABLED = bool(os.getenv('FEATURE_ENABLED', True))"

ROUTES:
  - add to: src/api/routes.py
  - pattern: "router.include_router(feature_router, prefix='/feature')"
```

## Validation Loop

### Level 1: Syntax & Style
```bash
# Run these FIRST - fix any errors before proceeding
ruff check src/ tests/ --fix     # Auto-fix what's possible
mypy src/ tests/                 # Type checking

# Expected: No errors. If errors, READ the error and fix.
```

### Level 2: Unit Tests
```python
# test_new_feature.py - Show exact test patterns to follow
def test_happy_path():
    """Basic functionality works"""
    result = new_feature("valid_input")
    assert result.status == "success"
    assert "expected_data" in result.data

def test_validation_error():
    """Invalid input raises ValidationError""" 
    with pytest.raises(ValidationError, match="specific error message"):
        new_feature("")

def test_external_api_failure():
    """Handles external failures gracefully"""
    with mock.patch('external_api.call', side_effect=APIError("Service down")):
        result = new_feature("valid") 
        assert result.status == "error"
        assert "service unavailable" in result.message.lower()
```

```bash
# Run and iterate until passing:
pytest tests/ -v --cov=src --cov-report=term-missing

# Expected: >90% coverage, all tests pass
# If failing: Read error, understand root cause, fix code, re-run
```

### Level 3: Integration Test  
```bash
# Manual testing commands that should work
curl -X POST http://localhost:8000/endpoint \
  -H "Content-Type: application/json" \
  -d '{"param": "test_value"}'

# Expected: {"status": "success", "data": {...}}
# Test error cases too - invalid input, network failures
```

## Final Validation Checklist
- [ ] All tests pass: `pytest tests/ -v`  
- [ ] No linting errors: `ruff check src/ tests/`
- [ ] No type errors: `mypy src/ tests/`
- [ ] Manual integration test successful
- [ ] Error cases handled gracefully with clear messages
- [ ] Documentation updated
- [ ] Dependencies added to requirements.txt

---

## Anti-Patterns to Avoid
- ❌ Don't create new patterns when existing ones work
- ❌ Don't skip validation because "it should work"
- ❌ Don't ignore failing tests - fix them  
- ❌ Don't use sync functions in async context
- ❌ Don't hardcode values that should be config
- ❌ Don't catch generic exceptions - be specific

## Confidence Score: X/10

Rate 1-10 how confident you are this will work in one pass:
- 8-10: Clear requirements, good examples, well-understood domain
- 5-7: Some ambiguity or complex integration points
- 1-4: Major unknowns or cutting-edge tech

Explain your reasoning.
```

## Key Principles for Writing Good PRPs

### 1. Context is King
Include EVERYTHING the AI needs:
- Exact documentation URLs with specific sections
- Code examples from your codebase to follow
- Library quirks and gotchas
- Your specific patterns and conventions

### 2. Make it Executable  
Validation loops must be commands AI can actually run:
```bash
# Good - AI can run this
pytest tests/ -v

# Bad - AI can't verify this
"Make sure it works"
```

### 3. Progressive Success
Start with simple working version, then enhance:
- Task 1: Basic functionality 
- Task 2: Error handling
- Task 3: Advanced features
- Task 4: Integration

### 4. Information Dense
Use keywords from your domain:
- Exact function names to call
- Specific error types to catch  
- Library-specific patterns

### 5. Show, Don't Tell
```python
# Good - shows exact pattern
async with get_connection() as conn:
    result = await conn.execute(query)

# Bad - vague instruction  
"Use database connection properly"
```

## When to Use PRPs

**Perfect for PRPs:**
- New features with clear requirements
- Integrations with external APIs
- Code that follows established patterns
- Anything with testable outcomes

**Not ideal for PRPs:**  
- Exploratory/research work
- UI design iterations
- Architecture decisions
- "Figure out what to build" problems

## PRP vs Regular Prompting

| Aspect | Regular Prompt | PRP |
|--------|---------------|-----|
| Context | Minimal | Comprehensive |  
| Success Rate | ~30% | ~90% |
| Iterations | Many | Usually 1 |
| Code Quality | Inconsistent | Matches codebase |
| Testing | Often skipped | Built-in validation |

The investment in writing a good PRP pays off massively in implementation quality and speed.