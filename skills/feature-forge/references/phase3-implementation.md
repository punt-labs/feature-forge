# Phase 3: Implementation & Verification

## Engineering Hive-Mind Setup

### Initialize Implementation Hive

Use hierarchical topology for implementation (queen coordinates, workers implement):

```
mcp__claude-flow__hive-mind_init(queenId: "forge-impl-queen", topology: "hierarchical")
```

Spawn workers based on scope:
- Small feature (1-3 beads): 1-2 workers
- Medium feature (4-8 beads): 2-3 workers
- Large feature (9+ beads): 3-5 workers

```
mcp__claude-flow__hive-mind_spawn(count: <N>, agentType: "worker", prefix: "forge-impl", role: "worker")
```

### Worker Assignment

The queen (orchestrator) assigns beads to workers based on:
1. Dependency order (`bd ready` shows unblocked work)
2. File proximity (related files go to same worker)
3. Skill alignment (frontend vs backend vs data)

## Implementation Loop

For each bead, follow this cycle:

### 1. Claim the Bead
```bash
bd update <id> --status in_progress
```

### 2. Read Requirements
Retrieve the bead's description to understand:
- What to implement
- Success criteria
- Files to modify

### 3. Implement

Write code following these principles:
- Minimal changes to achieve the goal
- Follow existing code patterns and style
- Add tests alongside implementation
- Handle error cases identified in SPARC plan

### 4. Test

Run the project's quality gates as defined in its CLAUDE.md. Typical gates include:
- Linting and formatting
- Type checking
- Unit and integration tests

Run tests for the affected area first, then the full suite.

### 5. Verify Success Criteria

Check each success criterion from the bead description. Mark as done only when all criteria are met.

### 6. Close the Bead
```bash
bd close <id>
```

### 7. Check for Newly Unblocked Work
```bash
bd ready
```

## Acceptance Testing

When the feature involves UI changes or user-facing behavior, verify with appropriate acceptance testing:
- Navigate through all new/modified flows
- Test the happy path
- Test error paths
- Test edge cases from SPARC refinement section
- Verify accessibility if applicable

If browser-based testing tools are available (e.g., Chrome DevTools MCP, Playwright), use them for automated UI verification.

## Ralph-Loop Integration

### When to Use Ralph-Loop

Ralph-loop is ideal for beads where:
- Success criteria are clearly testable (tests pass, linter clean)
- The implementation may need several attempts
- The completion state is unambiguous

### Crafting the Prompt

Include in the ralph-loop prompt:
1. The bead's full description and context
2. Specific files to modify
3. Success criteria that can be verified programmatically
4. Test commands to run

### Crafting the Completion Promise

The promise should be:
- Verifiable by running a command
- Unambiguous (either true or false)
- Tied to the actual success criteria

Good promises:
- "ALL TESTS PASS"
- "LINTER CLEAN AND TESTS PASS"
- "FEATURE COMPLETE AND ALL ACCEPTANCE CRITERIA MET"

Bad promises:
- "DONE" (too vague)
- "LOOKS GOOD" (subjective)
- "MOSTLY WORKING" (not unambiguous)

### Max Iterations

Set based on complexity:
- Simple bug fix: 3-5 iterations
- New function with tests: 5-10 iterations
- Complex feature with UI: 10-15 iterations
- Multi-file refactor: 10-20 iterations

## Final Verification

After all beads are implemented:

### 1. Full Quality Gates
Run all quality gates defined in the project's CLAUDE.md.

### 2. Open Bead Check
```bash
bd list --status=open
```
All feature-related beads should be closed.

### 3. Feature Walkthrough
Do a complete walkthrough of the feature:
- Test happy path
- Test error paths
- Test edge cases from SPARC refinement section

### 4. Git Hygiene

Review all changes before committing:
```bash
git diff --stat
git diff
```

Ensure:
- No debug code left in
- No commented-out code
- No unintended file changes
- All new files are necessary

## Failure Handling

### Test Failures
If tests fail after implementation:
1. Analyze the failure
2. If it's in the new code: fix it
3. If it's a pre-existing failure: note it and move on
4. If it's a regression: investigate and fix before closing the bead

### Bead Blocked
If a bead can't be implemented as planned:
1. Keep it as `in_progress`
2. Create a new bead describing the blocker
3. Add the blocker as a dependency
4. Move to the next unblocked bead

### Scope Discovery
If implementation reveals more work is needed:
1. Create new beads for discovered work
2. Set appropriate dependencies
3. Inform the user about scope change
4. Continue with unblocked work
