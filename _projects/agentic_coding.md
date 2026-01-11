---
layout: page
title: agentic coding workflows
description: practices for building with autonomous ai agents
img: assets/img/2601_ai_coding/guardrails_wide.png
importance: 1
category: tools
related_publications: false
---

<br>

[Claude Code](https://github.com/anthropics/claude-code) is a terminal-native AI coding agent that can execute commands, modify files, and reason over entire codebases autonomously. It's part of a broader shift toward agentic coding tools—systems that act directly on your repository rather than just suggesting code through a chat interface.

This autonomy creates new leverage—and new failure modes. When used well, it can feel like being a staff-level architect with a team of senior developers at your command. When used poorly, it generates plausible-but-wrong code at scale.

The difference isn't luck; these tools fail predictably when intent, constraints, and evaluation criteria are underspecified.

Below, I share the workflow and guardrails I've developed building TB-scale data pipelines and distributed training infrastructure at [HOPPR](https://www.hoppr.ai/).

*For background on Claude Code or installation, see [Anthropic's documentation](https://code.claude.com/docs/en/overview).*


Unlike traditional LLM tools (e.g., ChatGPT) that suggest code through a stateless interface, Claude Code can execute commands, modify files directly, and reason over your entire codebase. That autonomy is what makes it powerful—and what makes its failure modes different.

Claude Code isn't just a faster way to write code; it's a new kind of collaborator. Using it effectively requires designing interfaces, constraints, and feedback loops for a non-human agent with bounded authority.

Below, I share the workflow, guardrails, and practices I've found essential—from planning and implementation to managing complexity and avoiding common failure modes.

<br>
<div class="row justify-content-sm-center">
    <div class="col-sm-7 mt-3 mt-md-0">
    {% include figure.liquid path="assets/img/2601_ai_coding/trad_v_agentic.png" title="traditional vs agentic coding" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
In an agentic coding workflow, the human shifts from writing code to designing plans and feedback loops, while the agent executes directly against the codebase.
</div>
<br>


### The joy of leverage
One side effect of working with Claude Code is that I'm having fun again. My time is spent thinking about what to build and why, with less time fighting the mechanics of how. At times, I feel like a kid who is hooked on a video game and can't set it down. When the tedious parts recede, what's left is the part of engineering that made me enjoy it in the first place.

<br>

---

<br>

# Workflow

### Planning

Every feature—minor or major—begins with a planning document. I act as the architect, describing not just what should be built, but why specific decisions were made. This context helps Claude identify edge cases I might miss and understand the trade-offs I'm intentionally balancing.

I think of planning documents as the interface between human and agent. It defines intent, constraints, and trade-offs in a way the agent can reliably act on.

For complex features, this iterative planning phase can take hours. That time is almost always cheaper than debugging a misaligned implementation later.

Here's an example of the initial planning document:

```
### plan_01.md

[preface]

Your task is to...
1. Review the plan below
2. Assess its viability within the existing repository
3. Provide an updated planning document (plan_02.md) for review

[plan]
```

*Note*: The "preface" points to my current worktree and `claude_ops.md` file, both described later in this post.

When I'm satisfied with the plan, I ask Claude: "Are there any points of ambiguity about our plan?" Even when a spec is clear in my head, it's easy to assume the agent shares implicit context it doesn't actually have.

Claude's execution is impressive, but its real leverage comes from forcing you to externalize architectural intent.

---

### Implementation

After finalizing the plan, I tell Claude Code to implement. When planning is thorough, execution tends to be smooth—Claude reads relevant files, writes code, runs tests, and iterates on failures with minimal intervention.

Because the agent can act autonomously, reviewing diffs is non-negotiable. Before committing, I inspect every change to ensure it respects the original intent and doesn't introduce unnecessary complexity. Even when I request concise, reusable code, Claude will occasionally violate design principles—complexity creep, unnecessary abstraction, or duplication still happen.

I commit changes frequently. Given how quickly a codebase can evolve during agentic sessions, commits provide clean revert points if things go off the rails.

*Note:* Implementation can take time. To avoid twiddling my thumbs, I may run 2–3 tmux sessions, each with its own Claude Code instance working on separate features. This works best when tasks are truly orthogonal.

<br>

---

<br>

# Best practices

Left unchecked, Claude optimizes for code that looks impressive rather than code that's appropriate.
To counter this, I maintain a `claude_ops.md` file with project-specific coding standards. You can view the full file [here](/assets/misc/claude_ops.md).

Below are the high-level principles of this file, which I always reference in the preface of my planning documents (e.g., "First, review `/path/to/claude_ops.md` to learn our coding standards and best practices.")

**<u>Test-driven development (TDD)</u>**
<br>Write tests first, implement the minimal code to pass, then refactor. With AI agents, tests aren't just correctness checks—they're behavioral guards. They constrain what the agent is allowed to do and act as executable specifications that survive hallucinations, context loss, and creative over-interpretation.

**<u>Simplicity first</u>**
<br>AI agents love over-engineering. Left unchecked, Claude will add abstractions, frameworks, and "extensibility" you don't need. Explicit simplicity constraints reduce solution space and prevent complexity drift.

**<u>YAGNI (You aren't gonna need it)</u>**
<br>Don't let the agent build for hypothetical future requirements. Models are trained on codebases full of premature optimization—you must actively resist this tendency.

**<u>Reuse before rewriting</u>**
<br>Claude defaults to creating new code rather than adapting existing utilities. Force it to analyze the codebase for reusable components before writing anything new. This prevents bloat and maintains consistency.

**<u>Worktree safety</u>**
<br>When collaborating with an agent that can modify many files quickly, defining blast radius matters. Git worktrees provide containment: never allow Claude to delete or modify files outside the active branch or worktree. [This blog](https://medium.com/@dtunai/mastering-git-worktrees-with-claude-code-for-parallel-development-workflow-41dc91e645fe) demonstrates parallel development using Git worktrees. I also reference worktrees in the preface of my planning docs (e.g., "Your working directory for this task is `/path/to/feature/worktree`.")

**<u>Manual commits only</u>**
<br>I treat commits as a human-only boundary. Claude can propose changes, but I retain authorship over the narrative and intent of the codebase. Commits are documentation, and I want full ownership of that record.

<br>
<div class="row justify-content-sm-center">
    <div class="col-sm-8 mt-3 mt-md-0">
    {% include figure.liquid path="assets/img/2601_ai_coding/guardrails.png" title="guardrails" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
Guardrails constrain an autonomous coding agent’s behavior---limiting scope, complexity, and blast radius---so increased leverage doesn’t come at the cost of correctness or control.
</div>
<br>

# Other tips

**<u>Planning is everything</u>**
<br>Good planning isn't just helpful—it's the difference between Claude Code accelerating your work and derailing it.

**<u>Leverage other LLMs in planning</u>**
<br>Using another LLM during planning can surface edge cases or unnecessary complexity before execution begins. A fresh perspective often helps. There are tools which explore more structured multi-LLM workflows (e.g., [Mysti](https://github.com/DeepMyst/Mysti)), though I haven't used these extensively yet.

**<u>Managing the context window</u>**
<br>Claude Code's context window is large, but managing it intentionally matters. If a task shifts to a sufficiently different problem, start a fresh session rather than letting context bloat. This isn't giving up continuity—it's an intentional reset of the agent's working memory.

**<u>Not all tasks are created equal</u>**
<br>Different tasks require different levels of precision. Standalone dashboards or prototypes? Lower precision. Existing monorepo or production data pipeline? Higher precision. Calibrate your workflow accordingly.

**<u>Code drift</u>**
<br>Claude doesn't just reflect your codebase—it amplifies it. If the codebase is low quality or overly verbose, output quality degrades and context is wasted. On larger projects, maintaining code quality isn't optional—it compounds in both directions.

**<u>Using </u>`--dangerously-skip-permissions`**
<br>I almost always run Claude Code with this flag (see [documentation](https://www.anthropic.com/engineering/claude-code-best-practices)) to avoid constant approval prompts. The trade-off is accountability: if I'm not actively monitoring, the agent can go off the rails or get stuck debugging trivial environment issues. When that happens, I intervene immediately.

**<u>English is the new programming language</u>**
<br>Natural language is now executable. But without technical literacy, you may struggle to validate the output or identify when Claude Code is going off the rails. To be effective, you still need both---for now.

<br>

---

<br>

# Conclusion

These tools are evolving rapidly. Many of these tactics may be obsolete in months. But the intuition for how to work with autonomous coding agents—when to trust them, when to override them, and how to structure work effectively—will remain valuable.

Claude Code gives you leverage, but it also shifts responsibility. The bottleneck moves from writing code to designing the interfaces, constraints, and evaluation criteria that guide an autonomous collaborator.

At its best, Claude Code returns engineering to what made it fun in the first place: spending your time building things that matter, not fighting the mechanics of how.

<br>

---

<br>

# Acknowledgments

Thank you to the folks who have been thoughtful sounding boards in agentic coding workflows: John Paulett, John Gillotte, Robert Bakos, Eric Brattain, Kyong Song.

<br>

---

<br>

# Resources

Additional resources I've found useful:

- [Claude Code best practices](https://www.anthropic.com/engineering/claude-code-best-practices) from Anthropic.
- [Blog post](https://blog.sshh.io/p/how-i-use-every-claude-code-feature) on using Claude Code features.
- [Blog post](https://www.humanlayer.dev/blog/writing-a-good-claude-md) on writing a good `CLAUDE.md` file.
- [HappyApp](https://www.happyapp.org/): a local-first macOS app that pairs Claude Code with a polished UI and session management. Useful for longer-running workflows, or if you just want to check in while grocery shopping.
