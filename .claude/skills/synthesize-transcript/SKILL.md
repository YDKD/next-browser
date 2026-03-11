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

5. **Apply approved changes** to `SKILL.md` — add them inline to the
   relevant command section, not as a separate "tips" block.

## Anti-patterns

- Don't add "tips" or "best practices" sections. Guidance belongs next to
  the command it's about.
- Don't add workflow recipes ("first do X, then Y, then Z"). Document tool
  behavior, not agent strategy.
- Don't inflate existing docs with caveats that rarely apply.
- When in doubt, leave it out. A wrong or noisy addition is worse than a
  missing one.
