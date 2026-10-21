---
name: to-spec
description: "Turn the current conversation into a spec and publish it to the project issue tracker: no interview, just synthesis of what you've already discussed."
disable-model-invocation: true
---

This skill takes the current conversation context and codebase understanding and produces a spec. Do NOT interview the user; just synthesize what you already know.

The issue tracker and triage label vocabulary should have been provided to you. If not, tell the user to run `/setup-matt-pocock-skills`.

## Process

1. Explore the repo to understand the current state of the codebase, if you haven't already. Use the project's domain glossary vocabulary throughout the spec, and respect any ADRs in the area you're touching. Look for the project's docstring convention (in `.claude/rules/`, `AGENTS.md`, `CLAUDE.md`, or a coding-standards file) and note the existing types and endpoints the feature will touch.

2. Sketch out the seams at which you're going to test the feature. Existing seams should be preferred to new ones. Use the highest seam possible. If new seams are needed, propose them at the highest point you can. The fewer seams across the codebase, the better - the ideal number is one.

Check with the user that these seams match their expectations.

3. Write the spec using the template below, then publish it to the project issue tracker. Apply the `ready-for-agent` triage label - no need for additional triage.

<spec-template>

## Problem Statement

The problem that the user is facing, from the user's perspective.

## Solution

The solution to the problem, from the user's perspective.

## User Stories

A LONG, numbered list of user stories. Each user story should be in the format of:

1. As an <actor>, I want a <feature>, so that <benefit>

<user-story-example>
1. As a mobile bank customer, I want to see balance on my accounts, so that I can make better informed decisions about my spending
</user-story-example>

This list of user stories should be extremely extensive and cover all aspects of the feature.

## Implementation Decisions

A list of implementation decisions that were made. This can include:

- The modules that will be built/modified
- The interfaces of those modules that will be modified
- Technical clarifications from the developer
- Architectural decisions
- Schema changes
- API contracts
- Specific interactions

Do NOT include specific file paths or code snippets. They may end up being outdated very quickly. Contracts are the exception: they go in the Contracts section below.

Exception: if a prototype produced a snippet that encodes a decision more precisely than prose can (state machine, reducer, schema, type shape), inline it within the relevant decision and note briefly that it came from a prototype. Trim to the decision-rich parts, not a working demo, just the important bits.

## Contracts

The contracts this spec introduces or changes. Contracts only: never a function body, never a file path. Write them in the project's own language. Omit a subsection the spec has nothing for.

For a contract that already exists and changes, write its full new version, mark it **(changed)**, and add one line saying what differs.

### Types

Only the types and schemas this spec introduces or changes (e.g. Zod or Pydantic schemas, DTOs, enums), written out in full. An existing type is referred to by name, never repeated. Types come first so each one is defined before anything that references it.

### Endpoints

One block per endpoint:

<endpoint-example>
```
POST /teams/:teamId/rooms/:roomId/sync
Auth: team members with the `admin` role
Path params: teamId, roomId
Query: { force?: boolean }
Body: SyncRoomBody
Responses:
  200: Room
  401: UpworkAuthError, when the team's OAuth token is invalid
  404: NotFound, when the room does not exist for this team
```
</endpoint-example>

Request and response shapes are types from the Types subsection or existing types by name. Every error response the endpoint can return is listed, with its condition.

### Functions & Methods

The full signature and docstring of each function or method, with no body. Name it by its owning module or class (e.g. `RoomSyncService.syncRoom`), not by file path.

Docstrings follow the project's documented convention. If the project documents none, use this one, one line per section, without restating types from the signature:

<docstring-example>
```ts
class RoomSyncService {
  /**
   * @params teamId: the team the room belongs to; roomId: the Upwork room ID
   * @return the synced room, or throws UpworkAuthError if the team's OAuth token is invalid
   * Fetches the room from Upwork and upserts it with its latest messages.
   */
  syncRoom(teamId: string, roomId: string): Promise<Room>;
}
```
</docstring-example>

## Testing Decisions

A list of testing decisions that were made. Include:

- A description of what makes a good test (only test external behavior, not implementation details)
- Which modules will be tested
- Prior art for the tests (i.e. similar types of tests in the codebase)

## Out of Scope

A description of the things that are out of scope for this spec.

## Further Notes

Any further notes about the feature.

</spec-template>
