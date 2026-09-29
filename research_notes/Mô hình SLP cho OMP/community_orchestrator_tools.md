# Community & official tools for "one human -> one orchestrator -> many per-task agent sessions" (SLP-style)

> STATUS: DRAFT IN PROGRESS (saved early; will be extended). Date of research: 2026-09-29. Stars/forks figures come from GitHub repo pages fetched via WebFetch on that date (rounded by GitHub's UI). Dates for releases from GitHub atom feeds/raw files where noted.

## KQ1. Which tools implement the exact pattern "human talks to ONE supervisor agent that dispatches to and monitors per-task sessions"?

### Takeaway
(draft) Gas Town (Mayor), Tmux-Orchestrator (Orchestrator->PM->Engineer), multi-agent-shogun (Shogun->Karo->Ashigaru), Overstory (archived; coordinator->lead->workers), agent-deck "Conductor", and Claude Code's own Agent Teams (lead + teammates) implement the pattern to different degrees. Gas Town ships built-in `omp` and `pi` agent presets.

### Cited Findings

#### Gas Town (steveyegge/gastown) - closest match; has native `omp` preset
- Repo: https://github.com/steveyegge/gastown ; 18.2k stars, 1.7k forks, MIT, not archived, 7,770 commits (WebFetch of repo page, 2026-09-29) — [repo](https://github.com/steveyegge/gastown)
- Latest release in the Atom feed: v1.2.1, 2026-06-06 (feed returned only one entry) — [releases.atom](https://github.com/steveyegge/gastown/releases.atom). (A first WebFetch summary printed "2024" dates; that was a summarisation error, the atom feed timestamp is 2026-06-06.)
- The Mayor is "a Claude Code instance with full context about your workspace, projects, and agents. Start here - just tell the Mayor what you want" — the human's single interface — [README](https://raw.githubusercontent.com/steveyegge/gastown/main/README.md)
- Roles: Mayor (town-level coordinator), Deacon (daemon beacon/patrol), Dogs, Boot (checks Deacon every 5 minutes), Witness (per-rig, monitors polecats, nudges/recovers), Refinery (per-rig merge queue, bisecting Bors-style), Polecats (workers, persistent identity/ephemeral sessions, git worktrees), Crew (human's own long-lived workspaces) — [README](https://raw.githubusercontent.com/steveyegge/gastown/main/README.md), [glossary](https://raw.githubusercontent.com/steveyegge/gastown/main/docs/glossary.md)
- Work model: work items are "beads" (Git-backed issues in Dolt), grouped in "convoys"; `gt sling <bead-id> <rig>` puts work on an agent's "hook" (a pinned bead); GUPP: "If there is work on your Hook, YOU MUST RUN IT." — [glossary](https://raw.githubusercontent.com/steveyegge/gastown/main/docs/glossary.md), [README](https://raw.githubusercontent.com/steveyegge/gastown/main/README.md)
- Built-in agent presets: `claude, gemini, codex, kiro, cursor, auggie, amp, opencode, copilot, pi, omp` — [README](https://raw.githubusercontent.com/steveyegge/gastown/main/README.md)
- Source `internal/config/agents.go` defines `AgentOmp`: "Oh My Pi (OMP) — Pi fork with hook-based lifecycle. Inspired by github.com/ProbabilityEngineer/pi-mono gastown integration."; Command `omp`, Args `--hook .omp/hooks/gastown-hook.ts`, SessionIDEnv `OMP_SESSION_ID`, NonInteractive PromptFlag `--prompt`, `SupportsForkSession: false`. `AgentPi`: command `pi`, Args `-e .pi/extensions/gastown-hooks.js`, `PI_SESSION_ID`, ready delay 8000 ms because "Pi's Node.js TUI takes several seconds to initialize before it can receive tmux input" — [agents.go](https://raw.githubusercontent.com/steveyegge/gastown/main/internal/config/agents.go)
- Communication: `gt mail send/inbox` (persistent mail stored as beads), `gt nudge <agent> "msg"` (tmux-based, "literal mode + debounce + separate Enter"; docs: "Never use raw tmux send-keys"), `gt peek`, `gt feed` TUI, `gt escalate` — [reference.md](https://raw.githubusercontent.com/steveyegge/gastown/main/docs/reference.md)
- Identity: env vars set in the tmux session at spawn: `GT_ROLE`, `GT_RIG`, `GT_POLECAT`, `BD_ACTOR` (e.g. `gastown/polecats/toast`), `GIT_AUTHOR_NAME` = same; agent beads have IDs like `gt-<rig>-polecat-<name>`; `gt doctor` verifies tmux session env — [reference.md](https://raw.githubusercontent.com/steveyegge/gastown/main/docs/reference.md), [architecture.md](https://raw.githubusercontent.com/steveyegge/gastown/main/docs/design/architecture.md)
- Two-level beads: town-level `~/gt/.beads` (hq-* prefix; Mayor mail, convoys, agent identity) and rig-level beads; `routes.jsonl` maps prefix -> rig; `rigs.json` registry — [architecture.md](https://raw.githubusercontent.com/steveyegge/gastown/main/docs/design/architecture.md)
- Docs warn: "4-10 agents become chaotic" without orchestration (via WebFetch summary of README); tmux required for full workflow — [README](https://github.com/steveyegge/gastown)

#### Tmux-Orchestrator (Jedward23) - the simple, original tmux pattern
- 1.8k stars, 331 forks, not archived, only ~9-10 commits, first commit 2025-06-17, last commit 2025-07-14 (so effectively dormant) — [repo](https://github.com/Jedward23/Tmux-Orchestrator), [commits](https://github.com/Jedward23/Tmux-Orchestrator/commits/main)
- Hierarchy Orchestrator -> Project Managers -> Engineers, all Claude Code in tmux windows; "Orchestrator - You interact here" — [README](https://raw.githubusercontent.com/Jedward23/Tmux-Orchestrator/main/README.md)
- Comms: `send-claude-message.sh session:window "msg"` does `tmux send-keys -t WINDOW "$MESSAGE"; sleep 0.5; tmux send-keys -t WINDOW Enter` — [script](https://raw.githubusercontent.com/Jedward23/Tmux-Orchestrator/main/send-claude-message.sh)
- Self-scheduling: `schedule_with_note.sh <min> "<note>" [target]` writes a note file then `nohup bash -c "sleep N && tmux send-keys -t TARGET 'Time for orchestrator check! ...'"`; script contains hard-coded `/Users/jasonedward/...` paths (not portable) — [script](https://raw.githubusercontent.com/Jedward23/Tmux-Orchestrator/main/schedule_with_note.sh)
- Identity = tmux `session:window` addresses only; no registry beyond tmux; git discipline via CLAUDE.md (commit every 30 min, feature branches) — [CLAUDE.md](https://raw.githubusercontent.com/Jedward23/Tmux-Orchestrator/main/CLAUDE.md)

#### multi-agent-shogun (yohey-w)
- 1.4k stars, 295 forks, MIT, v5.1.0 latest release named "Karo Traffic Control", 372 commits (WebFetch, 2026-09-29) — [repo](https://github.com/yohey-w/multi-agent-shogun)
- Shogun (talks to the human, delegates immediately) -> Karo (task distribution, QC, single writer of dashboard.md) -> 7 Ashigaru (parallel workers) + Gunshi (strategy) — [repo](https://github.com/yohey-w/multi-agent-shogun)
- YAML mailbox files in `queue/inbox/<agent>.yaml`, per-worker `queue/tasks/`, `queue/reports/`; "Message content is never sent through tmux — only a short 'you have mail' nudge"; `inotifywait` for event-driven wake-up; `flock` on writes — [repo](https://github.com/yohey-w/multi-agent-shogun)
- Busy/idle detection by parsing CLI-specific prompt patterns in tmux panes (`lib/agent_status.sh`) and Claude Stop hook — [repo](https://github.com/yohey-w/multi-agent-shogun)
- Multi-CLI: Claude Code, Codex, Copilot, Kimi Code, OpenCode, Cursor, Antigravity; ntfy for phone in/out — [repo](https://github.com/yohey-w/multi-agent-shogun)
- Known limitations (from README): single Karo bottleneck; nudge interrupts; inotify only on local FS — [repo](https://github.com/yohey-w/multi-agent-shogun)

#### Claude Code Agent Teams (official, experimental)
- Enable with `CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1`; "Agent teams are experimental and disabled by default" — [docs](https://code.claude.com/docs/en/agent-teams)
- Components: Team lead (main session), Teammates (separate Claude Code instances), Task list, Mailbox; each mailbox is a JSON file `~/.claude/teams/{team-name}/inboxes/{agent-name}.json`; team config `~/.claude/teams/{team}/config.json` (holds session IDs & tmux pane IDs); tasks in `~/.claude/tasks/{team-name}/`; team name = `session-` + first 8 chars of session ID — [docs](https://code.claude.com/docs/en/agent-teams)
- Task claiming uses file locking; dependencies auto-unblock; hooks `TeammateIdle`, `TaskCreated`, `TaskCompleted` (exit 2 = feedback) — [docs](https://code.claude.com/docs/en/agent-teams)
- Limitations: no session resumption with in-process teammates; task status can lag; one team per session; no nested teams; lead is fixed; split panes need tmux/iTerm2; teammates all Claude — [docs](https://code.claude.com/docs/en/agent-teams)
- Docs: "In every approach the workers are Claude sessions. To involve a different tool, expose it to Claude as an MCP server." — [agents overview](https://code.claude.com/docs/en/agents)

(more entries being added)

### Inferences
- (to be completed)

### Gaps
- (to be completed)
