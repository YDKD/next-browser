---
description: >-
  Analyze an agent transcript for generalizable learnings about next-browser
  CLI usage, then propose SKILL.md updates.
argument-hint: "<path-to-transcript.jsonl>"
---

# synthesize-transcript

Read the transcript at:

$ARGUMENTS

Then analyze how the agent used `next-browser` commands. Your goal is to
find **generalizable learnings** — things that would help any agent using
this tool — and propose targeted additions to `SKILL.md`.

## Process

1. **Read the transcript** in chunks (it's large). Focus on assistant
   messages and tool calls/results involving `next-browser` commands.

2. **Identify friction and patterns.** Look for:
   - Commands that failed or produced unexpected results
   - Misunderstandings about how a command works
   - Workflows the agent discovered through trial and error
   - Repeated mistakes that guidance would prevent

3. **Filter ruthlessly for overfitting.** For each candidate learning, ask:
   - Would a different agent on a different project hit this same issue?
   - Is this a property of the *tool* or a property of *this debugging session*?
   - Is this already obvious from the command's existing docs?
   - Is this prescribing a specific workflow vs documenting tool behavior?

   **Discard** anything that is:
   - A workflow pattern specific to one task (e.g., "copy dirs for before/after")
   - A workaround for a bug that should be fixed in code instead
   - Advice an agent could derive from reading existing docs
   - Specific to a particular project, page, or debugging scenario

   **Keep** only things that are:
   - Genuine tool constraints any agent would hit (e.g., eval quirks, Playwright behavior)
   - Non-obvious failure modes with clear mitigations
   - Command interactions that aren't documented

4. **Present your findings.** For each learning, show:
   - What happened in the transcript (brief)
   - The proposed SKILL.md addition (exact text)
   - Why it's generalizable (one sentence)

   Ask the user to approve or reject each one before editing SKILL.md.

5. **Consider new Scenarios.** Beyond command-level learnings, check
   whether the transcript reveals a scenario not covered in the existing
   `## Scenarios` section of SKILL.md. A scenario earns its place only if
   it requires **domain knowledge an agent wouldn't derive on its own** —
   non-obvious mental models, ordering constraints, or decision frameworks
   that go beyond "use these commands and compare results."

   Ask yourself: could an agent figure out this workflow just by reading
   the existing command docs and applying basic debugging instincts? If
   yes, it's not a scenario — it's just competent tool use. If no — if it
   requires understanding something about React, Next.js, or the PPR model
   that isn't in the CLI docs — then it's a candidate.

   Present scenario candidates the same way as command learnings: evidence
   from the transcript, proposed text, and why an agent couldn't get there
   alone. Ask for approval before adding.

6. **Apply approved changes** to the repo-root `SKILL.md` — add
   command-level learnings inline to the relevant command section, and
   scenarios to the `## Scenarios` section.

## Anti-patterns

- Don't add "tips" or "best practices" sections. Guidance belongs next to
  the command it's about.
- Don't add workflow recipes ("first do X, then Y, then Z"). Document tool
  behavior, not agent strategy.
- Don't inflate existing docs with caveats that rarely apply.
- When in doubt, leave it out. A wrong or noisy addition is worse than a
  missing one.
