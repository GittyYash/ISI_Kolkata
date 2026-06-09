# PROGRESS — Knowledge Map & Gap Log

Legend: 🟢 solid · 🟡 shaky (revisit) · 🔴 gap (must fix) · ⬜ not yet tested

## D1 — Agentic Architecture & Orchestration (27%)
- 🟢 Workflows vs. agents (definition & when to use each)
- 🟢 Agent loop: act → observe feedback → decide; stopping conditions (no tool_use / max iterations)
- 🟢 Decision rule: path predictable → workflow; discoverable only via feedback → agent
- 🟢 Costs of agents: tokens, latency, less determinism, harder to debug
- ⬜ Prompt chaining (scenario teed up, not yet answered)
- 🟢 Routing (incl. independent optimization + model-to-difficulty matching)
- ⬜ Parallelization (sectioning vs. voting)
- ⬜ Orchestrator-workers
- ⬜ Evaluator-optimizer
- ⬜ When NOT to build an agent / cost-complexity tradeoffs

## D2 — Tool Design & MCP Integration (18%)
- ⬜ Tool schema design & descriptions
- ⬜ tool_use / tool_result message blocks
- ⬜ Multi-turn tool use
- ⬜ isError handling
- ⬜ MCP primitives: tools, resources, prompts
- ⬜ MCP in Claude Code / server scoping

## D3 — Claude Code Config & Workflows (20%)
- ⬜ CLAUDE.md
- ⬜ Slash / custom commands
- ⬜ Hooks (events, matchers, exit codes, gotchas)
- ⬜ Skills (SKILL.md frontmatter)
- ⬜ Settings & permissions
- ⬜ GitHub integration / SDK

## D4 — Prompt Engineering & Structured Output (20%)
- ⬜ Prompt structure & XML tags
- ⬜ Few-shot / CoT / prefill
- ⬜ Structured & JSON output
- ⬜ Tool use for structured data
- ⬜ Output reliability

## D5 — Context Management & Reliability (15%)
- ⬜ Context windows
- ⬜ Prompt caching
- ⬜ Long-context strategies
- ⬜ Retries / error handling
- ⬜ Evaluation & guardrails

---

## Session log

### Session 1 — 2026-06-09
- Covered D1 foundations: workflow vs agent, agent loop & stopping conditions,
  the predictable-path decision rule, costs of agents, routing pattern.
- All solid (🟢). Strong reasoning; only slip was "deterministic" vs
  "predictable" (corrected) and didn't yet know model-to-difficulty routing
  trick (taught).
- NEXT: prompt chaining (scenario already posed), then parallelization,
  orchestrator-workers, evaluator-optimizer.
