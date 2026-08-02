# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository state

This repository currently contains **only a documentation scaffold** under `docs/` — there is no application source code, package manifest, build tooling, linter, or test suite yet. Do not assume a tech stack, framework, or commands like `npm install`/`npm test` exist; verify with `Glob`/`Bash` before suggesting or running any build/lint/test command. When source code is eventually added, this file should be updated with the real commands and architecture.

## Documentation structure

Docs live in `docs/` as an Obsidian-style knowledge base, written primarily in Thai. Each folder has an `index.md` that explains its purpose and cross-links to related folders using wiki-links (`[[../path/index|label]]`). The folders encode the project's intended workflow as a numbered pipeline — each stage's output feeds the next:

```
01-requirements/01-spec   → what the system must do (source of truth)
01-requirements/02-plan   → roadmap/timeline derived from spec
01-requirements/03-task   → concrete task breakdown derived from plan
02-design/01-prototypes   → UI/UX mockups, referencing 01-spec
02-design/02-technical    → architecture, DB schema, API design — the blueprint for implementation
03-testing/01-test-plan   → test cases derived from 01-spec and 02-design
03-testing/02-test-result → actual pass/fail results and bugs found
04-retrospectives         → lessons learned per phase/sprint, informed by 02-test-result and 05-log
05-log                    → chronological changelog and decision log
00-archived               → superseded/cancelled docs — never delete docs, move them here instead
```

When adding or editing documentation, place it in the folder matching its stage in this pipeline, follow the existing `index.md` wiki-link convention when cross-referencing other sections, and write in Thai to match the existing content unless told otherwise.

## Git conventions

- Remote `origin` is `https://github.com/Plabuchill/Demo-project.git`.
- `.claude/settings.local.json` currently allow-lists `git commit`/`git push` — treat these as still requiring the same confirmation discipline as any other push/publish action described in your operating instructions, not as blanket pre-authorization.
