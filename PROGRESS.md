# PROGRESS — Knowledge Map & Gap Log

Legend: 🟢 solid · 🟡 shaky (revisit) · 🔴 gap (must fix) · ⬜ not yet tested

## D1 — Agentic Architecture & Orchestration (27%)
- 🟢 Workflows vs. agents (definition & when to use each)
- 🟢 Agent loop: act → observe feedback → decide; stopping conditions (no tool_use / max iterations)
- 🟢 Decision rule: path predictable → workflow; discoverable only via feedback → agent
- 🟢 Costs of agents: tokens, latency, less determinism, harder to debug
- 🟢 Prompt chaining (trades latency for accuracy; gates between steps; risk = error propagation)
- 🟢 Routing (incl. independent optimization + model-to-difficulty matching)
- 🟢 Parallelization: sectioning (split, all-of-N, for speed/focus) vs voting (repeat same task, for reliability)
- 🟢 Orchestrator-workers (runtime dynamic decomposition; orchestrator delegates + aggregates)
- 🟢 Evaluator-optimizer (generate→critique→revise LOOP; needs clear actionable criteria)
- 🟡 Pattern boundaries: routing(one path) vs parallelization(all paths); parallelization(design-time)
      vs orchestrator-workers(runtime); chaining(linear gate) vs evaluator-optimizer(loop) — corrected,
      revisit to confirm sticky
- 🟢 When NOT to build an agent: start simplest; add complexity only when a single
      call measurably fails; agent earned only when path is unpredictable AND value
      justifies cost. (Briefly mis-placed "debugging" as simple, self-corrected.)

## D2 — Tool Design & MCP Integration (18%)
- 🟢 Tool schema design & descriptions (model sees: name + input_schema + description;
      describe like onboarding docs; enum/pattern/example to prevent garbage values)
- 🟢 tool_use / tool_result message blocks (linked by id; result goes in a USER message;
      must append assistant's tool_use msg back into history first)
- 🟢 Multi-turn tool use (full round-trip flow)
- 🟢 isError handling (`is_error: true` boolean + descriptive content; distinguish
      failure from valid-empty result)
- 🟢 MCP primitives: tools (model-controlled), resources (app-controlled read-only data),
      prompts (user-controlled templates → slash commands). Mnemonic: model/app/user controls it.
- 🟡 MCP in Claude Code / server scoping: local (default; you+this project),
      project (.mcp.json in repo; whole team), user (you across all projects).
      Trap: "shared with team" = PROJECT scope, not "global"/user. Corrected — re-test next time.
      PRECEDENCE (verified, docs): local > project > user > plugin > claude.ai connector.
      Winner-takes-all by name — entire entry from highest scope used, NO field merge.
      (Yash initially flipped to local>user>project — project beats user.)

## D3 — Claude Code Config & Workflows (20%)
- 🟢 CLAUDE.md: loaded every session as a USER message AFTER system prompt (NOT system prompt;
      context not enforcement → use hooks for hard guarantees). Scopes: managed>user>project>local,
      concatenated broad→specific. Keep <200 lines (token cost + better adherence). @imports (depth 4).
      Path-specific → .claude/rules/ (paths frontmatter); multi-step procedures → skills. /init, /memory.
- 🟢 Slash / custom commands: native (.claude/commands/ project = git-shared; ~/.claude/
      commands/ user) vs MCP prompts (external server, cross-client, /mcp__server__name).
      Commands support $ARGUMENTS, ! bash, @ files, frontmatter (description/allowed-tools/model).
- 🟢 Hooks: scripts that fire at lifecycle events, run deterministically (enforcement layer).
      Events: SessionStart, UserPromptSubmit, PreToolUse(block✓), PostToolUse(react only),
      Stop(force-continue✓), SubagentStop, PreCompact, SessionEnd, Notification.
      Configured in settings.json. Structure: event→matcher(tool name)→hooks(handlers).
      EXIT CODES: 0=ok, 2=BLOCK(stderr→Claude), other(incl 1)=non-blocking. KEY TRAP: 1 ≠ block.
      Mental model: LLM decides, tools act, hooks=tripwires on tool calls (Edit/Write = tools).
      Initial slip: said Stop for post-edit format (→PostToolUse) and missed exit-1-not-blocking; both fixed.
- 🟢 Skills (SKILL.md): folder + SKILL.md (frontmatter + body). PROGRESSIVE DISCLOSURE — only
      `description` always in context; full body loads on invoke (vs CLAUDE.md full every session).
      Frontmatter: description (REQUIRED, drives auto-invocation — vague=never fires), allowed-tools
      (PRE-APPROVES, doesn't restrict), disable-model-invocation:true (user-only, e.g. /deploy),
      user-invocable:false (Claude-only), context:fork (isolated subagent, no convo history).
      Command name = directory name. Skills & custom commands have merged. Locations: ~/.claude/skills
      (user), .claude/skills (project/git), plugins. THE BIG THREE: CLAUDE.md=always-on facts;
      skill=on-demand procedures; hook=deterministic enforcement.
- 🟢 Settings & permissions: settings.json. Precedence (high→low): managed/enterprise >
      CLI args > local project > shared project > user. Permission lists: allow/ask/deny
      (DENY WINS). Rule syntax Bash(git push:*), Edit(src/**). Modes: default, acceptEdits,
      plan (read-only), bypassPermissions. Managed ENFORCES, CLAUDE.md GUIDES. permissions.deny
      = static pattern; PreToolUse hook = programmable judgment.
- 🟢 GitHub integration / SDK: GitHub Action (/install-github-app, tag @claude in issue/PR, runs
      headless in CI). Agent SDK (TS/Python) = build custom agents programmatically (headless
      claude -p), control tools/MCP/permissions/hooks.

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

### Session 1 (cont.) — 2026-06-12
- Completed ALL FIVE workflow patterns: chaining, routing, parallelization
  (sectioning/voting), orchestrator-workers, evaluator-optimizer. All 🟢.
- Misconceptions caught & corrected: (1) thought parallelization count was
  "dynamic / agent-like" — it's fixed/workflow; (2) routing vs parallelization
  confusion (one path vs all paths); (3) chaining-check vs evaluator loop
  (linear gate vs feedback loop). Logged as 🟡 to re-test next session.
- Strong on stopping conditions & design-time-vs-runtime decomposition.
- HOMEWORK: "when NOT to build an agent" + a pattern-matching warm-up next time.

### Session 2 — 2026-06-14
- Capstone pattern-matching: 5/5 (self-corrected E chaining vs evaluator).
- Homework reviewed: closed out "when NOT to build an agent" (🟢).
- **D1 COMPLETE — all 10 sub-topics green.** Boundary discriminations held up
  on cold recall (the 🟡 items from Session 1 are now solid).
- NEXT: D2 — Tool Design & MCP. Start with tool schema design & tool_use/
  tool_result blocks, then MCP primitives.

### Session 3 — 2026-06-16
- D2 COMPLETE (tool design, message blocks, is_error, MCP primitives, scoping/precedence).
- D3 COMPLETE — switched to TEACH-FIRST (Yash rusty on D3), then quizzed. Covered CLAUDE.md,
  custom commands, hooks, skills, settings/permissions, GitHub/SDK. All 🟢.
  Slips caught & fixed: Stop vs PostToolUse for post-edit formatting; exit-1-isn't-blocking
  (must be exit 2); description (not disable-model-invocation) as main reason a skill won't auto-fire.
- Great conceptual question from Yash: "is editing a file the LLM or a tool?" → tool. Mental
  model locked: LLM decides, tools act, hooks = tripwires on tool calls.
- 3 of 5 domains done (D1+D2+D3 = 65% of exam).
- NEXT: D4 — Prompt Engineering & Structured Output (20%). Likely teach-first again.
- HOMEWORK: think about WHY XML tags help Claude parse a prompt, and what "prefill" is.
