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

This autonomy creates new leverage—and new failure modes. When used well, it's like being a staff-level architect with their own team of developers. When used poorly, it generates plausible-but-wrong code at scale. The difference isn't luck; these tools fail predictably when intent, constraints, and evaluation criteria are underspecified.

The key shift: you're no longer just writing code. You're designing interfaces and guardrails for a non-human collaborator with bounded authority.

Below, I share the workflow and guardrails I've developed building TB-scale data pipelines and distributed training infrastructure at [HOPPR](https://www.hoppr.ai/).

*For background on Claude Code or installation, see [Anthropic's documentation](https://code.claude.com/docs/en/overview).*

<br>
<div class="row justify-content-sm-center">
    <div class="col-sm-8 mt-3 mt-md-0">
    {% include figure.liquid path="assets/img/2601_ai_coding/trad_v_agentic.png" title="traditional vs agentic coding" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
Traditional LLM tools (left) suggest code through a stateless interface for the human to implement. In an agentic coding workflow (right), the human shifts from writing code to designing plans and feedback loops, while the agent executes directly against the codebase.
</div>
<br>


### The joy of leverage
One side effect of using Claude Code is that I'm having fun again. I spend more time thinking about what to build and why and less time fighting the mechanics of how. At times, I feel like a kid who is hooked on a video game and can't set it down. When the tedious parts of coding recede, what's left is the part of engineering that made me enjoy it in the first place.

<br>

---

<br>

# Workflow

My approach centers on two phases: planning and implementation.

<br>

### Planning

Every feature—minor or major—begins with a planning document. I act as the architect, describing not just what should be built, but why specific decisions were made. This context helps Claude identify edge cases I might miss and understand the trade-offs I'm balancing.

I think of planning documents as the interface between human and agent. They define intent and constraints in a way the agent can reliably act on.

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

*Note: The "preface" points to my current worktree and `claude_ops.md` file, both described later in this post.*

Before finalizing the plan, I ask Claude: "Are there any points of ambiguity about our plan?" This often surfaces underspecified instructions. It's easy to assume the agent shares implicit context it doesn't actually have.

Claude's execution is impressive, but its real leverage comes from forcing you to externalize architectural intent.

---

### Implementation

After finalizing the plan, I tell Claude Code to implement. When planning is thorough, execution tends to be smooth—Claude reads relevant files, writes code, runs tests, and iterates on failures with minimal intervention.

Because the agent can act autonomously, reviewing diffs is critical. Before committing, I inspect every change to ensure it respects the original intent and doesn't introduce unnecessary complexity. Even when I request concise, reusable code, Claude will occasionally violate design principles—complexity creep, unnecessary abstraction, or duplication still happen.

I commit changes frequently. Given how quickly a codebase can evolve during agentic sessions, commits provide clean revert points if the agent diverges.

*Note: Implementation can take time. To avoid twiddling my thumbs, I may run 2–3 tmux sessions, each with its own Claude Code instance working on orthogonal features.*

<br>

---

<br>

# Best practices

Claude tends to over-engineer unless constrained. To counter this bias, I maintain a `claude_ops.md` file with coding standards. You can view the full file [here](/assets/misc/claude_ops.md).

Below are the high-level principles of this file, which I always reference in the preface of my planning documents (e.g., "First, review `/path/to/claude_ops.md` to learn our coding standards and best practices.")

**<u>Test-driven development (TDD)</u>**
<br>Write tests first, implement the minimal code to pass, then refactor. With AI agents, tests aren't just correctness checks—they're behavioral guards. They constrain what the agent is allowed to do, and they're executable specs that survive hallucinations and context loss.

**<u>Simplicity first</u>**
<br>Claude often adds abstractions, frameworks, and "extensibility" you don't need. Explicitly emphasizing simplicity mitigates complexity drift.

**<u>YAGNI (You aren't gonna need it)</u>**
<br>Don't let the agent build for hypothetical future requirements. Models are trained on codebases full of premature optimization—you must actively resist this tendency.

**<u>Reuse before rewriting</u>**
<br>Claude defaults to creating new code rather than adapting existing utilities. Force it to analyze the codebase for reusable components before writing anything new. This prevents bloat and maintains consistency.

**<u>Worktree safety</u>**
<br>When an agent can modify many files quickly, defining blast radius matters. Git worktrees provide containment: Claude can only modify files within the active worktree. I reference this in the preface of my planning docs: e.g., "Your working directory is `/path/to/feature/worktree`.").
<br>*[This blog](https://medium.com/@dtunai/mastering-git-worktrees-with-claude-code-for-parallel-development-workflow-41dc91e645fe) depicts how to use Git worktrees.*

**<u>Manual commits only</u>**
<br>I treat commits as a human-only boundary. Claude can propose changes, but I retain authorship over the narrative and intent of the codebase. Commits are documentation, and I want full ownership.

<br>
<div class="row justify-content-sm-center">
    <div class="col-sm-8 mt-3 mt-md-0">
    {% include figure.liquid path="assets/img/2601_ai_coding/guardrails.png" title="guardrails" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
Guardrails constrain an autonomous coding agent's behavior—limiting scope, complexity, and blast radius—so increased leverage doesn't come at the cost of correctness or control.
</div>
<br>

# Other tips

**<u>Planning is everything</u>**
<br>Good planning isn't just helpful—it's the difference between Claude Code accelerating your work and derailing it.

**<u>Leverage other LLMs in planning</u>**
<br>Planning input from another LLM can surface edge cases or unnecessary complexity before execution begins. A fresh perspective helps. There are tools which explore more structured multi-LLM workflows (e.g., [Mysti](https://github.com/DeepMyst/Mysti)), though I haven't used these extensively yet.

**<u>Managing the context window</u>**
<br>Claude Code's context window is large, but managing it deliberately matters. When switching to a sufficiently different task, start a fresh session. This resets the agent's working memory without losing meaningful continuity.

**<u>Not all tasks are created equal</u>**
<br>Match your workflow rigor to the stakes. Standalone prototypes can tolerate loose constraints and fast iteration. Production systems or shared monorepos require thorough planning and strict review.

**<u>Code drift</u>**
<br>Claude amplifies whatever patterns exist in your codebase. If existing code is verbose or poorly structured, the agent will generate similar output—wasting context and degrading quality. On larger projects, maintaining code quality isn't optional. It compounds.

**<u>Using </u>`--dangerously-skip-permissions`**
<br>I almost always run Claude Code with this flag (see [documentation](https://www.anthropic.com/engineering/claude-code-best-practices)) to avoid constant approval prompts. The trade-off: you must monitor actively. Without oversight, the agent can pursue wrong approaches or get stuck on trivial issues. When I notice this happening, I intervene immediately.

**<u>English is the new programming language</u>**
<br>Natural language is now executable. But technical fluency amplifies this: it lets you validate output, identify architectural missteps, and intervene before small errors compound. Communication and technical depth together make you effective.

<br>

---

<br>

# Conclusion

These tools are evolving rapidly. Specific tactics may become obsolete, but the intuition for working with autonomous agents—when to trust them, when to override, how to structure work—will remain valuable.

Claude Code shifts the bottleneck from writing code to designing interfaces and constraints for a non-human collaborator. That shift also changes what makes engineering enjoyable: less time fighting mechanics, more time building things that matter.

<br>

---

<br>

# Acknowledgments

Thanks to John Paulett, John Gillotte, Robert Bakos, Eric Brattain, and Kyong Song for conversations that shaped this thinking.

<br>

---

<br>

# Resources

Additional resources I've found useful:

- [Claude Code best practices](https://www.anthropic.com/engineering/claude-code-best-practices) from Anthropic.
- [Blog post](https://blog.sshh.io/p/how-i-use-every-claude-code-feature) on using Claude Code features.
- [Blog post](https://www.humanlayer.dev/blog/writing-a-good-claude-md) on writing a good `CLAUDE.md` file.
- [HappyApp](https://www.happyapp.org/): a local-first macOS app that pairs Claude Code with a polished UI and session management. Useful for longer-running workflows, or if you just want to check in while grocery shopping.
