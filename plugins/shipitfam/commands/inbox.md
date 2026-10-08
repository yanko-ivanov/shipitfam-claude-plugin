---
description: Work through everything the ShipItFam crew is waiting on you for, one request at a time. Plans, questions, risky commands, step reviews, "Ship it?" and failures.
argument-hint: "[project name]"
---

Walk me through what my ShipItFam crews need from me. Follow the shipitfam skill.

Scope: $ARGUMENTS

If that names a project, only handle that project (find its id with `project_list`). If it is empty, cover every project.

1. Call `inbox` (pass `project_id` when a project was named).
2. If nothing is waiting, say so in one line, point at /shipitfam:status for what the crew is doing, and stop.
3. Otherwise go one request at a time, oldest first (`asked_at`). For each one:
   - Call `request_get` with its `mission_id` so you have the whole request, not the teaser.
   - Tell me in plain language what the crew is asking: the plan's steps and critique, or the questions and their choices, or the exact command and why it is needed, or what the step did, or what "Ship it?" will push, or why the step failed. If the result says `redacted: true`, tell me part of it is hidden and that I have to open it in the ShipItFam app.
   - Say which option you would pick and why, then ask me what to do. Offer the request's own options by their labels. Wait for my answer.
   - Do not approve a plan, allow a command, ship, skip or cancel on my behalf, even if it looks obvious, unless I already told you to in this conversation. Do not answer the crew's questions for me.
   - Carry out my decision with `request_answer`: the option's `action`, my words in `text`, or in `answers` for a question. Then tell me what changed from the mission it returns: the step running again, the next request, or the mission done.
4. If the failure is "Claude is not logged in" and it has a `use_login` option, offer it by name (the project whose login it reuses) and, on my yes, call `request_use_login` with the request and the option's `credential_id`. A `fix_login` option is mine to do in the ShipItFam app: tell me where.
5. After each request ask whether to go on with the next one. Stop as soon as I say stop.

If `request_answer` says "Already handled", look again with `inbox` and report the current state instead of retrying.
