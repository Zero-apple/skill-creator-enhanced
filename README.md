# Skill Creator

Create, test, and iteratively improve Claude Code skills. The core workflow follows RED-GREEN-REFACTOR: draft a skill → run test cases → human review → improve → repeat.

## Use When

- Creating a new Claude Code skill from scratch
- Improving or debugging an existing skill
- Running evals to measure skill performance
- Optimizing a skill's description for better triggering accuracy

## Do Not Use When

- You just want to invoke an existing skill (use it directly)
- You're doing a one-off task that doesn't need to be reusable
- You're writing a simple prompt or alias (skills are for multi-step workflows)

## Core Workflow

1. **Capture intent** — What should the skill do? When should it trigger? What's the expected output?
2. **Write a draft** — SKILL.md with frontmatter (name + description required), plus README.md
3. **Create test cases** — 2-3 realistic prompts in `evals/evals.json`
4. **Run evals** — Spawn with-skill and baseline runs in parallel, grade results, launch viewer
5. **Human review** — User inspects outputs, leaves feedback
6. **Improve** — Generalize from feedback, keep prompts lean, explain the why
7. **Repeat** until satisfied
8. **Git push** — README auto-updated, commit & push to remote with confirmation

## Before You Start: Is This a Large Skill?

Not every skill needs complex architecture. Use the complexity scorecard to decide:

| Score | Complexity | Recommended Structure |
|-------|-----------|----------------------|
| 0-1 | Simple | Single `SKILL.md` file |
| 2-3 | Medium | `SKILL.md` + `resources/` (templates, references) |
| 4-6 | Large | Full architecture patterns — see below |

Each of these counts as 1 point: SKILL.md will exceed 200 lines, 5+ independent sub-tasks, external MCP/API dependencies, cross-step data dependencies, cross-turn execution, structured long-form output.

**Principle**: Start simple and upgrade as complexity grows. Don't over-engineer in the first draft.

For large skills (4+ points), read `references/architecture-patterns.md` which documents six proven patterns:

- **Orchestration-Execution Separation** — Decouple workflow control from task logic
- **File State Machine** — Survive auto-compact by persisting intermediate results
- **Incremental Report Assembly** — Append results as they complete, don't wait until the end
- **Overview-Detail-Review Structure** — Three-layer report organization for long outputs
- **Query-Analysis Constraint Separation** — Split execution rules from reasoning rules
- **Auto-Apply, Ask on Failure** — Automate prerequisites, only interrupt on failure

## Resources

- `SKILL.md` — Core instructions: when to trigger, how to run the create-test-review-improve loop
- `references/schemas.md` — JSON schemas for evals.json, grading.json, and benchmark.json
- `references/architecture-patterns.md` — **Architecture patterns for large/complex skills** (orchestration-execution separation, file state machines, incremental report assembly). Includes a complexity scorecard for deciding when these patterns apply.
- `agents/grader.md` — Subagent instructions for evaluating assertions against outputs
- `agents/comparator.md` — Subagent instructions for blind A/B comparison
- `agents/analyzer.md` — Subagent instructions for analyzing benchmark results
- `eval-viewer/generate_review.py` — Launch the human review viewer
- `scripts/` — Benchmark aggregation, skill packaging, description optimization

## Example Prompts

- "Create a skill that audits Dataphin account permissions — it needs to query MCP tools, run 10 independent checks, and generate an HTML report."
- "I have a draft skill for PDF extraction. Help me run evals and improve it."
- "My skill triggers too often on unrelated queries. Optimize its description."
- "Review this skill's test results and help me fix the failing cases."
