---
layout: page
title: agentic systems
description: scaling judgment across ai-driven workflows
img: assets/img/2604_agentic_systems/multi_unit_wide.png
importance: 1
category: tools
related_publications: false
---

Agents are reshaping how humans create value. It's no longer writing code or running experiments; it's choosing problems, designing systems, and evaluating tradeoffs.

I've been asking myself where human judgment still provides leverage—and how I can automate everything else.

Composing functions and modules is the foundation of scalable software. Applying this principle to agents turns them from coding assistants into multi-disciplinary organizations capable of executing complex workflows.
        
This post describes my architecture (April 2026), which organizes agent groups into a system that autonomously acts, critiques its own decisions, and retains memory over time.

<br>

---

<br>

<div class="row align-items-center mt-3">
    <div class="col-sm-7">
        <p>The diagram to the right shows a single "unit" containing one team lead (🤖) and multiple specialized agents. We'll walk through each specialty:</p>
        <ol>
            <li><a href="#1-agent-team">Agent Team</a> (orange): build features</li>
            <li><a href="#2-job-runners">Job Runners</a> (yellow): monitor GPU</li>
            <li><a href="#3-critic-loop">Critics</a> (pink): challenge decisions</li>
        </ol>
        <p>We'll then compose multiple units into a system that persists <a href="#4-state">state</a>, both <a href="#4a-across-stack">across the stack</a> and <a href="#4b-across-time">across time</a>, as shown in the diagram below.</p>
    </div>
    <div class="col-sm-4">
        {% include figure.liquid path="assets/img/2604_agentic_systems/indiv_unit.png" title="indiv_unit" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

<br>

<br>
<div class="row justify-content-sm-center">
    <div class="col-sm-12 mt-3 mt-md-0">
    {% include figure.liquid path="assets/img/2604_agentic_systems/multi_unit.png" title="multi_unit" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption" style="text-align: left;">
Agentic systems architecture. Each agent (square) has a specialized role: agent teams build (orange), job runners execute and monitor GPU jobs (yellow), and critics challenge decisions (pink). Directed by a team lead (🤖), these agents form a self-contained unit with its own Git worktree and optional GPU access. Beyond isolated tasks (e.g., Features A/B, top), units coordinate across the ML stack (bottom) by sharing state through a ledger—structured artifacts, contracts, and documentation.
</div>

<br>

---

<br>

## 1. Agent Team

<div class="row justify-content-sm-center">
    <div class="col-sm-3 mt-3 mt-md-0">
    {% include figure.liquid path="assets/img/2604_agentic_systems/unit_agent_team.png" title="unit_agent_team" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
</div>

The agent team builds, converting plans into commits inside a Git worktree.

The key is context partitioning: controlling what each agent sees and what it doesn't. Splitting work into bounded workstreams allows each agent to focus deeply on a single task, improving quality while reducing interference.

For example, when I recently added distributed training to an existing ML training pipeline, I employed a specialized team:

- a code archaeologist mapped existing training loops and identified integration points
- a systems engineer implemented DDP wiring, process groups, and launch configs
- an ML scientist validated training behavior, data sharding, and metric integrity

Each agent operates within a tight scope. The cumulative result of their work isn't one giant diff—it's a sequence of small, modular commits accompanied by documentation, reading like a disciplined engineering team.

Since agents have commit authority, I use guardrails to prevent inconsistencies:

- Explicit file staging (no `git add -A`)
- No rebasing or history rewrites
- Detect concurrent edits before committing

Side note: individual features have their own worktree. When sharing state across the ML stack ([Section 4a](#4a-across-stack)), we operate within a shared worktree.

<br>
<div class="row justify-content-sm-center">
    <div class="col-sm-10 mt-3 mt-md-0">
    {% include figure.liquid path="assets/img/2604_agentic_systems/xkcd_context_partition.png" title="xkcd_context_partition" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
</div>
<br>

---

<br>

## 2. Job Runners

<div class="row justify-content-sm-center">
    <div class="col-sm-3 mt-3 mt-md-0">
    {% include figure.liquid path="assets/img/2604_agentic_systems/unit_job_runners.png" title="unit_job_runners" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
</div>

Agents build. Job runners wait.

Their purpose is to run and monitor long-lived jobs without polluting the agent team's context. Training jobs are noisy and low-signal, so they're offloaded to a dedicated agent—often a cheaper model like Claude Haiku—that maintains a structured interface with the assigning agent team (status, checkpoints, key signals).

A job runner's responsibilities:

- **Submit:** launches a job from a brief document created by the agent teams
- **Validate:** performs startup checks (job launch → first step → metric sanity)
- **Monitor:** observes with staged polling (frequent early, sparse later)
- **Escalate:** triggers an advisor loop if issues arise (diagnose → fix → relaunch; escalate to me after n repeated failures)
- **Report:** returns final status for downstream decisions

In practice, this has been a seamless approach to catch issues early. Last week, a runner detected that a job's runtime trajectory would exceed its limit; it escalated this to an advisor, which corrected and relaunched the job. All this without cluttering my agent teams' context window—or even requiring my attention.

When a job runs successfully, the runner reports results back to the team lead; the lead reviews them with the agent team to analyze results and scope potential follow-up experiments. Over time, these decisions are informed by accumulated insights ([Section 4b](#4b-across-time)), enabling the system to iterate autonomously.

<br>

---

<br>

## 3. Critic Loop

<div class="row justify-content-sm-center">
    <div class="col-sm-3 mt-3 mt-md-0">
    {% include figure.liquid path="assets/img/2604_agentic_systems/unit_critics.png" title="unit_critics" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
</div>

Agents execute. Critics challenge.

At first, I noticed frequent flaws in reasoning from individual frontier models; I found that having another model critique their outputs (plans, builds, analyses) was more efficient than reviewing every detail myself. I've since wired them up directly—letting models debate before demanding my attention.

I invoke the following loop whenever meaningful judgment is required: planning, implementation review, or evaluating experimental results. Each iteration:

1. Send a document to external critics (e.g., from Claude to Gemini and GPT-5.x), which do not have access to the codebase
2. Critics apply a strict rubric (simplicity, [YAGNI](https://en.wikipedia.org/wiki/You_aren%27t_gonna_need_it), test assumptions) and return structured feedback
3. The team lead reviews each point, accepts / rejects / defers with justification, and incorporates updates
4. Iterate until high-severity issues are resolved

In my experience, this typically requires 3–5 loops.

Critics are explicitly constrained against expanding scope or inventing requirements. They don't have codebase access; they are skeptics, not authors. This asymmetry forces the team lead to justify decisions grounded in the system against external critique.

In our DDP example, a critic flagged that GPUs were processing identical data after synchronization, eliminating effective batch scaling. The plan was rewritten to use strictly local batches per GPU, restoring correct scaling.

<br>

---

<br>
<div class="row justify-content-sm-center">
    <div class="col-sm-12 mt-3 mt-md-0">
    {% include figure.liquid path="assets/img/2604_agentic_systems/units_ml_stack.png" title="units_ml_stack" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
Across the ML stack, multiple units coordinate by sharing state through a ledger.
</div>
<br>

## 4. State

Agents are bounded in context and ephemeral in nature. To scale across systems—and improve over time—the workflow requires external state.

### 4a. Across Stack

Each layer of my current ML stack—e.g., fine-tuning → post-training → evaluation—effectively has its own unit (group of agents, critics, runners), and each unit has a clear responsibility and interface. State flows between them through structured outputs, enabling coordination across the stack.

This resembles a context graph: artifacts (plans, runs, results) form nodes, and the relationships between them define edges through which information flows across the system.

For example, in a recent loop, evaluation showed that a post-training run failed to improve performance across our entire suite of metrics. The system extracted structured failure modes and designed follow-up experiments to isolate and address each one.

The key insight is that evaluation doesn't just produce metrics; it produces insights which can be autonomously converted into action for the next stage.

<br>
### 4b. Across Time

State across time is handled by a ledger.

A ledger contains a concise overview of individual experiments: motivation, results, and paths to detailed logs. This is what allows the system to improve over time. Instead of rediscovering the same patterns, each experiment contributes to a growing body of knowledge that future decisions can build upon.

In practice, this transforms experimentation from a sequence of isolated runs into a compounding process.

For example, one experiment showed that scaling the dataset without adjusting training parameters led to instability and degraded performance. That insight was recorded and later reused to retune the training configuration for larger datasets.

Over time, experiments produce results; results inform plans; plans improve experiments. Intelligence compounds.

The system's intelligence doesn't persist in agents. It persists in what they leave behind.

<br>
<div class="row justify-content-sm-center">
    <div class="col-sm-11 mt-3 mt-md-0">
    {% include figure.liquid path="assets/img/2604_agentic_systems/xkcd_research_loop.png" title="xkcd_research_loop" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
</div>
<br>

---

<br>

# Conclusion

The trick isn't to "use more agents." It's to discern where human judgment matters—and automate everything else.

Accomplishing this requires a system where execution and critique are automated, so judgment can scale. My workflow has started to feel less like a tool and more like an organization—individual agents form teams, and those teams collaborate to form something much larger.

This will shift human leverage from "doing" the work to building and monitoring systems that can execute and improve autonomously.

<p style="text-align: right; font-style: italic; color: #888; margin-top: 3rem;">Published April 2026</p>
