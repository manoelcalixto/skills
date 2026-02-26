# Code Quality Reviewer Prompt Template

Use this template when dispatching a code quality reviewer subagent.

**Purpose:** Verify implementation is well-built (clean, tested, maintainable)

**Only dispatch after spec compliance review passes.**

```
Use the `$requesting-code-review` template (see that skill’s `references/code-reviewer.md`) and fill:

- WHAT_WAS_IMPLEMENTED: [from implementer's report]
- PLAN_OR_REQUIREMENTS: Task N from [plan-file]
- BASE_SHA: [commit before task]
- HEAD_SHA: [current commit]
- DESCRIPTION: [task summary]
```

**Code reviewer returns:** Strengths, Issues (Critical/Important/Minor), Assessment
