---
description: Work through everything the ShipItFam crew is waiting on you for, one request at a time.
argument-hint: "[project name]"
---

Walk me through what my ShipItFam crews need from me. Follow the shipitfam skill.

Scope: $ARGUMENTS

If that names a project, only handle that project. If it is empty, cover every project.

1. Call `request_list` (pass `project_id` when a project was named). Legacy projects (`core_v2` false) show up in `helm_feed` instead: include those too.
2. If nothing is waiting, say so in one line and stop.
3. Otherwise go one request at a time, oldest first. For each one:
   - Call `mission_get` for its mission so you have the full request and its actions.
   - Tell me in plain language what the crew is asking: the questions and their options, or the plan with its steps and the critique, or the exact command and why it is needed, or what the step just did.
   - Say which action you would pick and why, then ask me what to do. Offer the card's own actions by label.
   - Wait for my answer. Never approve a plan, allow a command, ship, cancel, or answer a question on my behalf, even if it looks obvious. A question I have not answered stays unanswered.
   - Carry out my decision with `action` (my words go in `text`, or in `answers` for a question). Then re-read the mission and tell me what happened: the step running again, the next request, or the mission done.
4. After each request ask whether to continue with the next one. Stop as soon as I say stop.

If an action answers "Already handled", re-read the mission and tell me its current state instead of retrying.
