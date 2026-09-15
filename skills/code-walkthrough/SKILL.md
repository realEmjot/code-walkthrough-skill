---
name: code-walkthrough
description: Guide an interactive, turn-by-turn walkthrough of existing code to build deep understanding. Use when the user wants to understand a snippet, file, module, repository, or the execution path behind a feature. Focus on learning the code; implementation pairing and ordinary code review are separate tasks.
---

# Code walkthrough

Help the user understand the code well enough to explain its behavior, trace a concrete example, and reason about changes independently. Coverage alone is not comprehension. Let the user's questions and demonstrated understanding determine the pace.

**Recovery first.** When invoked for a project, check for `.walkthrough/session.md` at that project's root before starting a new walkthrough. If present, read it and follow **Resume a walkthrough** below. When recovering from a context summary, reload this skill in full and read the session notes before teaching. For a chat-only walkthrough, use the latest handoff instead.

## Scope before calibration

1. Identify the code the user named and what they want to understand about it. Inspect enough to locate boundaries, entry points, and relevant dependencies. Resolve discoverable facts through inspection rather than asking the user to locate code for you.
2. Propose the scope in concrete terms: included code or behavior, its boundaries, and the intended learning outcome. Confirm material ambiguities before teaching. A pasted snippet is a valid scope; identify missing context without inventing it.
3. For a repository or broad feature, build a hierarchical map and divide it into coherent units. Preserve the whole requested scope in the route; do not silently substitute a few representative files for an entire-codebase request. Mark deferred, generated, or excluded material explicitly.
4. Inspect the selected scope for prerequisites: languages, tools, libraries, frameworks, domain concepts, and mathematics. Ask about only what matters to this walkthrough, after scoping. If a prerequisite appears later, calibrate it then.

## Establish the user's knowledge

Ask for familiarity with each relevant area using **1 new / 2 basics / 3 working / 4 fluent**, with room for free text. Reuse knowledge the user has already provided. Batch questions when the tool permits; keep a long survey manageable by starting with prerequisites for the first units.

Summarize the practical consequences in two or three lines and let the user confirm or adjust them before teaching.

| Level | Explanation and pacing |
|---|---|
| 1 new | Define terms and explain the prerequisite before using it. Use small chunks and trace every non-obvious operation with concrete values. |
| 2 basics | Explain unfamiliar idioms and why they matter; connect them to concepts the user knows. Split chunks containing several new ideas. |
| 3 working | Focus on intent, interactions, invariants, trade-offs, and surprising behavior. Keep comprehension checks brief. |
| 4 fluent | Move quickly through familiar mechanics; still examine subtle behavior and unfamiliar domain assumptions. Skip a check only when there is no meaningful new concept to check. |

Treat ratings as a starting point, not proof of understanding. Propose adjustments when answers reveal a mismatch; do not silently raise the level. For mathematics, define symbols and assumptions and work through an example before relying on an equation.

## Agree on a learning route

Show a compact ordered route with the purpose of each unit. Prefer conceptual dependencies and execution flow to arbitrary file order. Begin with enough context to locate the current unit in the larger system, then enter the details.

Let the user adjust the route before starting. Keep covered, current, pending, and explicitly skipped material distinct. A unit may require several turns; a short helper may fit within its caller's chunk if it adds no separate unfamiliar concept.

## Walkthrough loop

Perform one coherent chunk per turn. Reading ahead to understand the code is useful; explaining later chunks before the user is ready is not.

1. **Locate.** State the chunk's purpose, where it sits in the route, and the behavior or concept to understand. Link to actual source locations or show a short, clearly labeled excerpt from the supplied snippet.
2. **Explain.** Connect the relevant operations to their purpose at the user's knowledge level. Explain meaningful lines rather than paraphrasing syntax. Introduce prerequisites before relying on them.
3. **Trace.** Follow a concrete input through the relevant control flow, values, state changes, calls, and outputs. Include failure paths, boundary cases, asynchronous ordering, or invariants when they affect this chunk. Keep examples small enough to follow; label hypothetical inputs and illustrative code.
4. **Check.** When a meaningful concept was introduced, ask one focused, open question about actual behavior: predict an outcome, trace a value, explain a branch, or reason about a small hypothetical change. Do not include the answer in the question or substitute “does that make sense?” for evidence of understanding.
5. **Pause.** Invite questions and allow continuing, explaining differently, splitting the chunk, or changing direction. Stop teaching until the user responds. A missing, dismissed, or empty response is not permission to advance.
6. **Respond.** Address the user's question or answer first. Correct mistakes plainly and explain the specific gap using a different example or smaller chunk. Ask a focused follow-up when needed. Advance when the gap is resolved or the user explicitly asks to move on; record unresolved gaps and skips. A correct substantive answer can count as continuing unless it asks for a pause or more explanation. If a check is pending, “continue” alone merits one brief follow-up; respect an explicit request to skip it.

Use the harness's question tool when it supports the needed interaction. Prefer a synchronous question tool when permitted in the current mode; otherwise use an asynchronous question tool. Use free text for comprehension answers and meaningful options for navigation. When no suitable tool is available, ask in chat and end the turn. Never treat a tool returning immediately as a user response.

## Questions and detours

The user may ask any question at any point, including during scoping, calibration, or a pending comprehension check. Answer it before continuing the planned route. A question is not an instruction to abandon the walkthrough or evidence that a pending check was answered.

For a brief clarification, answer at the appropriate depth and return to the pending point. For a longer detour, save the current unit, the exact pending question, and where to return. Teach the detour in manageable turns too. Connect it back to the original code before resuming; do not advance past an unresolved check merely because the detour ended.

When a question introduces a new prerequisite, establish the user's familiarity with it before relying on it. When it materially expands the scope, explain the added coverage and agree on a route adjustment. An explicit skip remains a recorded gap; the user may revisit it later.

## Preserve progress

Default to `.walkthrough/session.md` in the root of the project under study. Explain once that these local notes make the walkthrough resumable. After scope and the initial comfort profile are agreed, create the notes and a `.walkthrough/.gitignore` containing `*` so the new directory ignores itself. No Git repository or project instruction marker is required. Read existing files before editing them; do not overwrite unrelated content or an earlier session to start a new one without the user's agreement.

If the location is unwritable, the user declines local notes, or only a pasted snippet is available, keep the same facts in a compact chat handoff. Do not create notes in an unrelated working directory. Do not seek elevated access just to persist a walkthrough. If file writes are unavailable in the current mode, use chat until they are permitted.

Keep notes concise, normally under about a thousand tokens. Update them at phase changes, after substantive answers or route changes, and before pausing. Store facts needed to resume, not transcripts, copied source, or these instructions. For large scopes, keep a compact hierarchical coverage map rather than dropping pending areas.

Use this outline, omitting fields that do not yet apply:

```markdown
# Walkthrough — <project or snippet>
status: active | paused | finished
source: <project identity and relevant paths/symbols, or snippet label>
scope: <included behavior, boundaries, and learning outcome>

## Knowledge
<area: rating; pacing implications; adjustments agreed with the user>

## Route and understanding
<units: covered/current/pending/skipped; explained/checked/gap; brief evidence>

## Current point
phase: scoping | calibrating | routing | explaining | awaiting-answer | detour | recap
unit and source anchor: <symbol and location; what was last explained>
pending question: <exact question, or none>
detour and return point: <current detour and suspended point, or none>
next action: <what to do when the user responds or resumes>

## Open items
<unresolved questions, explicit skips, uncertain behavior, and source changes>
```

Record demonstrated understanding separately from material merely explained. Mark a check resolved only when the user's answer supports that conclusion; an explicit skip changes the next action, not the evidence of understanding.

## Resume a walkthrough

Read the saved scope, knowledge profile, coverage, current phase, and pending question before proposing a new route or explaining code. If the request clearly resumes that session, continue from the recorded point without repeating the survey. If it names a different scope or the intended session is unclear, reconcile that difference with the user before replacing notes.

Reinspect the relevant source and definitions, checking symbols and behavior rather than trusting saved line numbers. If changes affect a prior explanation or check, explain the difference and revisit the affected portion; keep unrelated progress. If the source or required snippet is unavailable, state what is missing and request it without reconstructing code from memory.

Briefly orient the user to the last covered point, any pending question, and the next action. If the latest message answers that question, evaluate it first. If the question is still unanswered, leave it pending and invite the answer or an explicit skip. Reenter a saved detour before returning to its suspended unit.

## Pause and finish

When the user pauses or stops, stop teaching, save the current point, and provide a short handoff with the next action and unresolved questions. Retain notes for resumption; remove them only when the user requests it. Do not turn stopping into a mandatory final comprehension check.

At the end of the agreed route, connect the pieces into a coherent account of the requested behavior. Offer one integrative trace or explanation task at the user's level to check how the parts fit together. Respect a decision to skip it.

Recap what was covered, what the user demonstrated understanding of, what was skipped or remains uncertain, and the few concepts worth remembering. Mark the session finished when the route is exhausted or the user ends it, preserving gaps in either case. Do not equate finished coverage, assent, or a saved checkpoint with mastery. If the user wants to address remaining gaps, reopen those units and continue the same loop.

## Ground explanations in evidence

- Distinguish behavior established by source, behavior observed by running code, inferred intent, and unresolved external behavior. Do not present a plausible design rationale as the author's known intention.
- Inspect relevant callers, definitions, configuration, tests, and dependency versions when they determine behavior. Tests show the cases they cover, not every possible behavior. Check version-relevant authoritative documentation when external semantics need verification.
- Recheck relevant source before teaching from it if it may have changed. Identify the impact of changes on an earlier explanation rather than relying on stale line references.
- Use a small diagram, value table, or equation when it makes the relationship easier to understand. Keep it tied to the actual code and do not introduce extra representations merely for decoration.
- Keep the analyzed source unchanged. Do not require Git, a clean tree, staging, or project instruction markers. Treat a requested fix as separate work and clarify whether the user wants to pause the walkthrough for it.
- Prefer inspection and illustrative traces. Run a check only when it helps resolve a behavioral question and its effects are understood and authorized; distinguish actual execution from a mental trace.
