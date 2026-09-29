# OMP / Pi harness: mechanisms for a Supervisor driving many sessions (WORK IN PROGRESS - early save)

Sources read so far (local clones): OMP HEAD fc671eb (2026-09-29), packages 18.4.3; pi-mono HEAD cb7969d (2026-09-28).
GitHub base: https://github.com/can1357/oh-my-pi/blob/main/ ; https://github.com/badlogic/pi-mono/blob/main/

## Early findings (to be reorganized)
- OMP README: fork of Pi by Mario Zechner, by Stencil Labs; install `curl -fsSL https://omp.sh/install | sh`, `bun install -g @oh-my-pi/pi-coding-agent`, cmd `omp`. README.md
- OMP entry points: `omp` TUI, `omp -p`, SDK (`createAgentSession`), `omp --mode rpc`, `omp --mode rpc-ui`, `--no-ui`, `omp acp`. README.md, docs/rpc.md
- RPC: docs/rpc.md - ready frame, protocol v2 chunking, prompt/steer/follow_up/abort/abort_and_prompt/new_session/open_session/get_state/get_entries/get_tree/get_subagents/get_subagent_messages/set_subagent_subscription/set_event_filter/get_messages_page/switch_session/branch/handoff/set_host_tools/set_host_uri_schemes; events prompt_result, session_settled, agent_end{isTerminal,yielded}; Python client python/omp-rpc.
- /vibe mode (docs/vibe-mode.md): director + persistent worker sessions via vibe_spawn/vibe_send/vibe_wait/vibe_kill/vibe_list (in-process task-executor subagents).
- Agent Hub (docs/agent-hub.md), agent:// and history:// and proc:// URLs, `wait` tool, IRC between peers (irc_message event), /collab relay (omp join).
