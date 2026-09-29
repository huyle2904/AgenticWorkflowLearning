# Practitioner experiences: many parallel coding-agent sessions and supervisor/orchestrator agents (DRAFT v1, being extended)

Method note: Reddit, HN (news.ycombinator.com, hn.algolia.com), X, Medium, Substack, personal blogs and dev.to were blocked for direct fetch in this environment (egress proxy), and WebSearch refuses reddit.com. Only github.com, anthropic.com, code.claude.com and raw.githubusercontent.com could be fetched. Most blog/HN/X material below therefore comes from search-engine result summaries (which can paraphrase) - flagged "(snippet)". Items fetched directly are flagged "(fetched)". Everything is anecdote unless it says "measured".

## 1. How do practitioners describe "I am the bottleneck" and what do they do about it?

### Takeaway
Nearly every practitioner converges on the same diagnosis: agents parallelize generation, but the human's review, decision, and context-holding stay single-threaded, so the practical ceiling is ~2-4 foreground agents (up to ~5-8 with heavy tooling), with review/verification becoming the bottleneck rather than coding.

### Cited Findings
- Simon Willison (Oct 2025): can only focus on reviewing and landing one significant change at a time; parallel agents are useful for research/PoC tasks that do not modify code you plan to keep (snippet) — [Simon Willison](https://simonwillison.net/2025/Oct/5/parallel-coding-agents/)
- Willison later: can run four agents at once but "by 11am" he is wiped out; recommends limiting how much code agents generate per day to what you can review (snippet of secondary summary) — [search summary of Willison's writing](https://simonwillison.net/tags/agentic-engineering/)
- Addy Osmani, "Your parallel Agent limit": cognitive bandwidth doesn't parallelize - the agent generates, but you still evaluate/decide/trust/integrate single-threaded; start with two agents, add more, and by noon you're accepting output without careful review; limit depends on task complexity, spec quality, session length (snippet) — [Osmani](https://addyosmani.com/blog/cognitive-parallel-agents/)
- Pragmatic Engineer: only senior+ engineers reported using parallel agents successfully; Armin Ronacher said he kicks off parallel agents less than he used to: "it's only so much my mind can review" (snippet) — [Pragmatic Engineer](https://blog.pragmaticengineer.com/new-trend-programming-by-kicking-off-parallel-ai-agents/)
- Armin Ronacher, "Agent Psychosis" (18 Jan 2026): "when I watch someone at 3am, running their tenth parallel agent session, telling me they've never been more productive — in that moment I don't see productivity"; agents are "amazing" but "massive slop machines if you turn off your brain" (snippet quoting) — [Ronacher](https://lucumr.pocoo.org/2026/1/18/agent-psychosis/)
- Kilo Code write-up (response to Ronacher): "we're living in the age where the bottleneck is shifting from writing code to verifying it. The practical workflow is much less dramatic: 2-4 agents in the foreground, a handful in the background, and a strong verification loop on top" (snippet) — [Kilo blog](https://blog.kilo.ai/p/how-7-kilo-code-engineers-run-up)
- Axios (4 Apr 2026): power users report agents "operate like slot machines"; peak productivity means many agents in parallel, which "requires near-constant context switching, which humans aren't great at" (snippet) — [Axios](https://www.axios.com/2026/04/04/ai-agents-burnout-addiction-claude-code-openclaw)
- Steve Yegge on Gas Town: it "churns through implementation plans so quickly that you have to do a LOT of design and planning to keep the engine fed"; Maggie Appleton draws the conclusion that design/planning becomes the bottleneck once agents do the coding (snippet) — [Appleton](https://maggieappleton.com/gastown)
- HN, Superset (10-parallel-agents terminal) thread: once you have 5-10 agents "the bottleneck often becomes remembering what each one is doing and actual human review"; Superset added issue/task tracking so work moves issue -> agent -> diff -> PR -> review (snippet) — [HN 46368739](https://news.ycombinator.com/item?id=46368739)
- HN (Meetless Agent, "MLA"): claims the max concurrent sessions a dev could manage was ~4 when manually reviewing each; tool injects approved project decisions and flags contradictions across sessions (vendor claim) (snippet) — [HN](https://news.ycombinator.com/item?id=49405261)
- Community sizing guides: "2-4 parallel agents is the sweet spot for most developers"; "trouble starts at the fourth agent - you no longer know which one is waiting for a review, which one finished, which one crashed"; beyond ~8 even with a board review becomes the bottleneck (snippet of several blog/guide sites, low rigor) — [AgentsRoom guide](https://agentsroom.dev/blog/run-coding-agents-in-parallel)
- Boris Cherny (Claude Code): 5 terminal Claudes numbered tabs 1-5 + 5-10 on claude.ai; "Spin up 3-5 git worktrees at once ... the single biggest productivity unlock"; iTerm2 system notifications when a session finishes or needs input (snippet/X) — [Cherny on X](https://x.com/bcherny/status/2017742743125299476)
- Peter Steinberger ("Just talk to it", Oct 2025): 3-8 parallel Codex instances in a 3x3 terminal grid, mostly in the SAME folder (no worktrees), short prompts, ~20% of time on refactoring by agents (snippet) — [steipete](https://steipete.me/posts/just-talk-to-it)

### Inferences
- The consensus sizing is about attention, not compute: the useful WIP limit is the number of sessions whose state you can keep in your head (about 3-5), so any supervisor design should aim at reducing what the human must remember per session, not at raising the session count.

### Gaps
- Reddit threads (r/ClaudeCode, r/ClaudeAI etc.) could not be reached; only snippet-level evidence.

(Draft continues; see later sections being added.)
