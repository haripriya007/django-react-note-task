# Codex Prompt

## Prompt 1 — Find and verify a suitable public repository

I want to select an existing public GitHub repository for a focused full-stack change. It must genuinely use React, Django REST Framework, and a SQL database.

Find three candidates and verify from their source code—not just the README—that each contains:
- React entry points, components/pages, and an API client;
- Django models, serializers, views/viewsets, URL routing, and authentication/permission configuration;
- SQL database configuration, migrations, and data-access patterns;
- test suites and documented local setup.

For each candidate, report the repository URL, license and reuse considerations, last meaningful activity, stack evidence with exact paths, setup/test commands, database engine, test quality, likely setup blockers, and any obvious secret or real-data exposure risks. Do not reproduce secret values if found; report only file/path and risk.

Recommend one repository and justify the choice based on maintainability, ability to run tests, feature scope, and safety. Do not clone, modify, or publish anything until I approve the candidate.

## Prompt 2 — Build a source-backed architecture map

For the approved repository, inspect the code and map one existing user flow end to end.

Identify exact files and symbols for:
- React route/page, component state, form/event handlers, and API client;
- HTTP method, URL, request payload, response shape, authentication headers, and error handling;
- Django URL route, view/viewset, serializer/validation, permission checks, and model/queryset;
- SQL tables/relationships, constraints, indexes, migrations, and transaction behavior where relevant;
- frontend/backend tests and commands to run them.

Trace the actual request from UI to API to database and back. Cite file paths and function/class names. Distinguish facts proven by code from assumptions. Flag insecure patterns, duplicated logic, missing tests, and setup blockers. Do not edit files yet.

## Prompt 3 — Establish the baseline before any change

Before proposing a feature, determine how to reproduce the current application locally.

Inspect README instructions, dependency lockfiles, environment templates, package scripts, Django settings, database configuration, migrations, and test configuration. Identify the required Python/Node/database versions and environment variables without printing secret values.

Propose exact commands for baseline backend tests, Django checks, migration consistency, frontend tests, lint if configured, and production build. Use the repository's intended database. If it is unavailable, report the blocker and propose a safe isolated test strategy without silently changing production settings.

Run only commands available and safe in this environment. Capture exact command output and label each check PASS, FAIL, BLOCKED, or NOT RUN. Record pre-existing failures so they are not misattributed to our changes. Do not modify application code yet.

## Prompt 4 — Find a real gap and recommend a specific feature

Inspect the repository's existing user flows, models, API endpoints, and UI. Identify three possible improvements that would solve a real user problem in this application. Do not choose a feature only because it is easy to implement.

For each option, describe the current behavior and its limitation, then the exact desired behavior after the change. Show the likely React, Django REST Framework, and SQL areas involved, citing actual repository paths and symbols. State which database changes are genuinely necessary; do not invent a schema change just to touch the database if the feature can be implemented correctly with the existing schema.

Compare user value, codebase fit, complexity, security/privacy implications, migration risk, backward compatibility, and testability. Recommend one feature and explain why. Do not edit code yet; wait for approval of the feature.

## Prompt 5 — Define the approved change as a before-and-after specification

For the feature I approved, document exactly how the application behaves before the change and how it must behave afterward. Base every statement on the inspected repository and clearly label assumptions.

Include:
- **Current behavior:** the actual UI flow, API call, backend logic, and data persistence path, with file paths and symbols.
- **New behavior:** the exact user action, expected UI response, API request, server-side processing, database operation, and response reflected in the UI.
- **Acceptance criteria:** numbered, observable pass/fail conditions for normal use, invalid input, authentication/authorization, missing records, repeat actions, network/API errors, and relevant boundary cases.
- **Data behavior:** whether existing tables are sufficient or a schema change is needed; what happens to existing rows and concurrent/repeated requests.
- **Compatibility:** existing endpoints, clients, records, and flows that must continue working.

Do not write vague requirements such as “improve error handling.” Specify the error condition, expected HTTP status/body, and expected UI behavior. Do not change code yet.

## Prompt 6 — Produce the exact file-by-file code change plan

Before implementation, trace the approved feature through the actual source and create a change matrix. Do not give a generic architecture plan. Every row must refer to a real path and a real class, function, component, or test in this repository.

For every proposed file, include:
1. **Current code:** what it does now and the relevant class/function/component.
2. **Required edit:** the exact logic to add, modify, or remove—not just “update the API” or “handle errors.”
3. **Reason:** which approved acceptance criterion requires the edit.
4. **Data/API contract:** exact field names, payload and response shapes, HTTP method/path/status codes, or schema operations as applicable.
5. **Before → after flow:** what happens today and what will happen after the change.
6. **Failure/security behavior:** validation, ownership/permissions, retries, duplicate actions, and error cases relevant to that file.
7. **Proof:** the specific test or command that will verify the edit.

Organize the matrix by database/model and migration, serializer/validation, view/URL/permissions, React API client, React component/state, tests, and documentation as relevant. If a layer does not need a change, explicitly say why. Include a compact end-to-end sequence from user action to persisted database state and back to the rendered UI. Identify risks and migration/rollback implications. Wait for approval before editing.

## Prompt 7 — Implement the approved database and Django API changes

Implement the approved backend plan in the actual repository. Do not stop at recommendations, pseudocode, or snippets for me to copy.

First restate the approved backend file list and each concrete edit. Then modify only the necessary files:
- model/schema and migration, only if the feature requires a data-model change;
- serializer or validation logic for the exact accepted fields and invalid inputs;
- view/viewset/service/queryset logic to perform the required operation;
- URL routing and HTTP method wiring;
- authentication and object-level permissions so users cannot read or modify another user's data;
- backend tests for success, validation failure, permissions/ownership, missing records, persistence, and regression cases as applicable.

Use the repository's conventions and database engine. Preserve existing API contracts unless the approved specification explicitly changes them. Define actual response codes and payloads consistently. Handle transactions and uniqueness/race conditions if relevant. Do not weaken security, switch database engines to bypass a failure, add unrelated refactors, or leave TODOs in place of required behavior.

After implementation, show the real diff grouped by file. For each file, state precisely what code changed and how it implements the acceptance criteria. Trace the request from URL to permission checks, validation, database operation, and response. Run the relevant backend checks and report actual results; do not claim success if tests have not run.

## Prompt 8 — Implement the React UI changes and connect them to the API

Implement the approved frontend plan in the actual repository. Do not create a disconnected demo or fake success state.

First identify the exact component/page, event handler, API client function, state variables, and frontend test files that will change. Then make the concrete edits:
- add or modify the user control and event handler that triggers the feature;
- call the exact backend endpoint using the agreed HTTP method and request payload;
- process the real response and update/refetch UI state according to the API contract;
- show pending, success, validation, permission, not-found, network, and server-error states as relevant;
- prevent accidental duplicate actions and handle stale/in-flight responses where relevant;
- preserve existing navigation, layout, accessibility, and unrelated behavior;
- add/update tests for the user action, request payload, state transitions, and API success/failure.

Actually edit the source files. Then show the real diff and explain the concrete before-and-after behavior. Map each changed frontend file to the approved acceptance criteria and backend contract. Run the available frontend tests/build and report exact results. Do not claim the feature is complete if the UI is not connected to the real API.

## Prompt 9 — Perform a security and correctness review

Act as a senior engineer reviewing code you did not write. Inspect the actual diff and surrounding code; do not trust previous summaries.

Check:
- authentication and object-level authorization at every affected endpoint;
- strict input validation, unexpected fields, type coercion, and error/status consistency;
- SQL/data integrity, migration safety, default/null behavior, transaction/race concerns;
- API compatibility and accidental information disclosure;
- React state races, stale responses, duplicate submissions, unhandled errors, and accessibility;
- regressions, overengineering, unrelated edits, dependency changes, debug code, unsafe logging, secrets, real data, and generated artifacts.

Report findings by severity with exact file paths and line references. For each finding, explain exploit/impact, reproduction, and the smallest safe fix. Do not modify anything during the review; ask for approval on a remediation plan first.

## Prompt 10 — Add tests that prove the acceptance criteria

Review the acceptance-criteria matrix and existing tests. Identify which criteria already have meaningful coverage and which do not.

Actually add or update the repository's test files; do not only list suggested test cases. Identify the exact test paths and test functions to add or modify. Add focused backend tests for valid requests, invalid payloads/types, authentication, ownership isolation, missing resources, persistence, and regression behavior as applicable. Add frontend tests for rendering, API payload, pending state, successful response, failed response, and accessibility-relevant behavior as applicable. Use the project's test conventions and isolated fixtures; do not rely on production data or shared state.

If the project supports integration tests, add or propose a test that exercises the actual UI/API/database path. Do not replace real database verification with mocks and then claim end-to-end coverage. Explain what each test proves and which requirements remain uncovered.

## Prompt 11 — Execute checks and capture evidence

Read the repository's documented commands and scripts, then run the relevant checks. Include exact commands for backend tests, Django checks, migration consistency, frontend tests, lint if configured, and production build.

Use the intended database engine for integration claims. If only SQLite tests can run while the application uses PostgreSQL or another SQL engine, clearly distinguish those results and explain what remains unverified.

For each command, record:
- exact command and working directory;
- actual exit status and concise output;
- PASS, FAIL, BLOCKED, or NOT RUN;
- failure diagnosis and next action;
- whether the result was run now or comes from historical notes.

Do not manufacture evidence files or write “passed” manually. Capture actual terminal output. Do not silently modify configuration just to get a green test run.

## Prompt 12 — Review maintainability, scope, and Git diff

Inspect the final working tree, staged and unstaged diffs, changed-file list, dependency lockfiles, migration, tests, and documentation.

Check that every change supports the approved feature. Identify unrelated refactoring, duplicated logic, dead code, weak names, missing types/validation, fragile tests, unnecessary dependencies, generated files, local environment files, database dumps, and secrets in current files or history.

Run whitespace/diff checks. Summarize the final diff by layer: database, API, UI, tests, docs. Report any pre-existing issues separately from issues introduced by this change. Do not commit or push yet. Propose cleanup actions and explain any destructive/history-rewriting implications before doing them.

## Prompt 13 — Document the implementation and AI workflow accurately

Create concise documentation for a developer who did not participate in the work:
- problem and acceptance criteria;
- architecture and request/data flow;
- files changed and key design choices;
- API contract and migration notes;
- setup and test commands;
- actual test/build results and limitations;
- known risks and next steps.

Maintain an AI engineering log from genuine activity: actual prompts, meaningful follow-ups, files changed, commands, errors/retries, results, and human approval/review decisions. Do not reconstruct missing history as verbatim prompts. Mark anything unavailable as unavailable. Keep the documentation aligned with the code and actual command output.

## Prompt 14 — Final reproducibility and release-readiness check

Review the project as a developer receiving it for the first time. Verify setup steps, environment configuration, migration sequence, test commands, API usage, and documented limitations. Check that no credentials, real user data, database dumps, `.env` files, bytecode, `node_modules`, or build outputs are unintentionally committed. Remember that deleting a secret from the latest version does not remove it from Git history.

Report release readiness as one of: READY, READY WITH LIMITATIONS, or NOT READY, with evidence for each conclusion. Include exact remaining blockers and the commands needed to resolve them. Do not commit, change remotes, rewrite history, or push until I approve the final publication plan.

---

## Required outputs from the workflow

By the end, the repository should contain only useful, accurate artifacts such as:
- a repository-specific feature specification and acceptance criteria;
- a small, reviewable code diff;
- a migration where needed;
- backend and frontend tests;
- actual command output/evidence;
- updated setup/API documentation;
- a truthful AI activity log;
- a list of verified behavior and outstanding limitations.

The essential deliverable is a traceable change: current behavior → approved desired behavior → exact file-level edits → tests → verified end-to-end result. The AI must identify what changed in the code and why, not merely describe an architecture or list suggestions.
