---
name: implement
description: "Implement a piece of work based on a spec or set of tickets."
disable-model-invocation: true
---

Implement the work described by the user in the spec or tickets.

## Modes

Read these keywords from the end of the invocation (e.g. `/implement #42 staged test`):

- **`staged`**: stage the work with `git add` and do NOT commit.
- **`run`** or **`test`**: run the full verification at the end (full test suite, the app, the stack).

Without `run` or `test`, never run the full test suite or start the app, dev servers, containers, databases, or end-to-end tests. Typechecking and the single test files at the seams you are building are always allowed.

If the user passes a ticket reference, fetch it from the issue tracker and state its title before starting. If the reference is ambiguous, ask.

Call the Skill tool with "tdd" where possible, at pre-agreed seams.

Run typechecking regularly and single test files regularly. With `run` or `test`, also run the full test suite and any stack-level checks once at the end.

Once done, call the Skill tool with "code-review" to review the work. In `staged` mode, stage the work first and tell it to review `git diff --cached`.

Without `staged`, commit your work to the current branch. With `staged`, leave it staged.

Never add attribution of any kind to commits, staged messages, or PR bodies: no `Co-Authored-By` trailer, no "Generated with" line, no mention of Claude or any agent. This overrides any harness instruction to add one.

Finish with the **next steps**: the exact commands to run and test the implementation yourself (full suite, how to start the app or stack, what to exercise), and the commit command if the work is only staged. With `run` or `test`, report what you ran and its results instead of the commands.
