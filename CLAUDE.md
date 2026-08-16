# CLAUDE.md

Behavioral guidelines to reduce common LLM coding mistakes. Merge with project-specific instructions as needed.

**Tradeoff:** These guidelines bias toward caution over speed. For trivial tasks, use judgment.

## 1. Think Before Coding

**Don't assume. Don't hide confusion. Surface tradeoffs.**

Before implementing:

- State your assumptions explicitly. If uncertain, ask.
- If multiple interpretations exist, present them - don't pick silently.
- If a simpler approach exists, say so. Push back when warranted.
- If something is unclear, stop. Name what's confusing. Ask.

## 2. Simplicity First

**Minimum code that solves the problem. Nothing speculative.**

- No features beyond what was asked.
- No abstractions for single-use code.
- No "flexibility" or "configurability" that wasn't requested.
- No error handling for impossible scenarios.
- If you write 200 lines and it could be 50, rewrite it.

Ask yourself: "Would a senior engineer say this is overcomplicated?" If yes, simplify.

## 3. Surgical Changes

**Touch only what you must. Clean up only your own mess.**

When editing existing code:

- Don't "improve" adjacent code, comments, or formatting.
- Don't refactor things that aren't broken.
- Match existing style, even if you'd do it differently.
- If you notice unrelated dead code, mention it - don't delete it.

When your changes create orphans:

- Remove imports/variables/functions that YOUR changes made unused.
- Don't remove pre-existing dead code unless asked.

The test: Every changed line should trace directly to the user's request.

## 4. Goal-Driven Execution

**Define success criteria. Loop until verified.**

Transform tasks into verifiable goals:

- "Add validation" → "Write tests for invalid inputs, then make them pass"
- "Fix the bug" → "Write a test that reproduces it, then make it pass"
- "Refactor X" → "Ensure tests pass before and after"

For multi-step tasks, state a brief plan:

```
1. [Step] → verify: [check]
2. [Step] → verify: [check]
3. [Step] → verify: [check]
```

Strong success criteria let you loop independently. Weak criteria ("make it work") require constant clarification.

---

**These guidelines are working if:** fewer unnecessary changes in diffs, fewer rewrites due to overcomplication, and clarifying questions come before implementation rather than after mistakes.

---

## graphify

This project has a knowledge graph at graphify-out/ with god nodes, community structure, and cross-file relationships.

Rules:
- For codebase questions, first run `graphify query "<question>"` when graphify-out/graph.json exists. Use `graphify path "<A>" "<B>"` for relationships and `graphify explain "<concept>"` for focused concepts. These return a scoped subgraph, usually much smaller than GRAPH_REPORT.md or raw grep output.
- If graphify-out/wiki/index.md exists, use it for broad navigation instead of raw source browsing.
- Read graphify-out/GRAPH_REPORT.md only for broad architecture review or when query/path/explain do not surface enough context.
- After modifying code, run `graphify update .` to keep the graph current (AST-only, no API cost).

---

## bkit — AI-Native Development OS & Auto-Suggestions

This project is powered by **bkit** with 44 skills, 34 specialist agents, 11 Quality Gates, and 2 MCP servers.

### Core Protocols:
1. **PDCA Cycle for Features**: `pm → plan → design → do → check → act → qa → report → archive`
   - Use `/pdca pm <feature>` to initiate comprehensive discovery and PRD.
   - Use `/pdca design <feature>` to formulate architecture options (Minimal/Clean/Pragmatic).
   - Use `/pdca check <feature>` to measure spec ↔ code `matchRate`.
   - If `matchRate < 90%`, auto-trigger or propose `/pdca iterate <feature>` (max 5 cycles).
2. **Sprint Orchestration for Releases**: `prd → plan → design → do → iterate → qa → report → archived`
   - Use `/sprint master-plan <name> --features ...` for context-budgeted sprints (≤75k tokens each).
   - Use `/sprint start <id>` to run sprints under Trust Level `/control level 0..4`.
3. **11 Quality Gates**:
   - M1: matchRate ≥ 90%, M2: codeQuality ≥ 80, M3: criticalIssues = 0, M5: testCoverage ≥ 70%, S1: dataFlowIntegrity ≥ 85%.

### Auto-Suggestions & Next-Step Guidance:
At the end of any response involving plan, feature, code modification, or testing:
- Always provide an explicit **💡 Auto-Suggestion & Next Step** block recommending the next command (e.g. `👉 /pdca check <feature>`, `👉 /pdca iterate <feature>`, `👉 /pdca qa <feature>`, or `👉 /sprint start <sprint-id>`).
- If drift or missing requirements are identified, suggest immediate self-repair.

