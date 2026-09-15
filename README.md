# code-walkthrough

An agent skill for understanding existing code, one manageable chunk at a time. Name a snippet, file, module, repository, or feature: the agent scopes the walkthrough with you, establishes your familiarity with the relevant technologies and concepts, and guides you through the code at your pace. Ask questions or change direction at any point.

The goal is comprehension: being able to explain the behavior, trace concrete inputs, and reason about changes yourself. Explanations adapt to your knowledge of the languages, tools, libraries, frameworks, domain concepts, and mathematics involved.

The workflow lives in one [SKILL.md](skills/code-walkthrough/SKILL.md), with optional display metadata in [agents/openai.yaml](skills/code-walkthrough/agents/openai.yaml). It requires an agent that reads skills; question tools improve the interaction, with chat as a fallback.

## Install

Install from GitHub using the [skills CLI](https://github.com/vercel-labs/skills):

```sh
npx skills add realEmjot/code-walkthrough-skill --skill code-walkthrough
```

Or install from a local checkout, running this from the repository root:

```sh
npx skills add . --skill code-walkthrough
```

The installer lets you select your agent. Add `--global` to install for your user rather than the current project. For a manual installation, copy the entire `skills/code-walkthrough` directory into your agent's skills directory.

## Usage

Invoke the skill by name where your agent supports it, or ask it to use `code-walkthrough`:

```text
Use code-walkthrough to help me understand the login flow, from the
request handler through session creation and error handling.
```

Other starting points include a pasted function, a module you are about to change, an unfamiliar algorithm, or a repository you are joining. You can ask for a smaller chunk, a different explanation, a prerequisite detour, or an explicit skip at any pause.

## What a session looks like

1. **Scope.** Identify the code and learning outcome, including the boundaries of a broad request.
2. **Knowledge check.** Rate relevant areas from 1 new to 4 fluent. The agent confirms how those ratings affect its explanations and revisits them as it learns more about your understanding.
3. **Learning route.** Agree on an order based on concepts and execution flow, with explicit coverage of pending and skipped material.
4. **Walkthrough.** Locate a chunk in the source, explain the meaningful operations, trace concrete values, and pause for questions and a focused comprehension check.
5. **Questions and detours.** Address your questions immediately while preserving the original return point and any unresolved check.
6. **Recap.** Connect the pieces and distinguish material explained from understanding demonstrated, including remaining gaps.

An explicit skip is respected and recorded. A missing response does not advance the walkthrough. The agent grounds explanations in source and distinguishes observed behavior, inferred intent, and uncertainty.

## What it writes into your project

The skill keeps the code being studied unchanged. By default it maintains:

- `.walkthrough/session.md` — scope, knowledge profile, route, coverage, pending questions, detour return point, and unresolved gaps.
- `.walkthrough/.gitignore` — contains `*` so the newly created notes directory ignores itself.

These notes are retained when you pause or finish and read when you resume. The agent rechecks relevant source before continuing, including changes that affect an earlier explanation. No Git repository, clean tree, staging, or project instruction marker is required.

For pasted snippets, unwritable locations, or when you decline local notes, the agent provides a compact chat handoff instead. Existing unrelated files are preserved.

## Repository layout

```text
README.md
LICENSE
skills/
└── code-walkthrough/
    ├── SKILL.md
    └── agents/
        └── openai.yaml
```

## Credits

The collaborative pacing, knowledge calibration, and comprehension-focused interaction are adapted from [realEmjot/pair-programming-skill](https://github.com/realEmjot/pair-programming-skill). This skill applies those ideas to exploring existing code.

## License

[MIT](LICENSE)
