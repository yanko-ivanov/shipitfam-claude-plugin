---
description: Where your ShipItFam projects stand. What needs you, what is running, what is queued, what just finished, and why a queue is stuck.
argument-hint: "[project name]"
---

Give me a status report on my ShipItFam projects. Follow the shipitfam skill. This is read-only: do not answer, approve, cancel, comment, wake or start anything.

Scope: $ARGUMENTS

If that names a project, report on that one project. If it is empty, cover every project.

1. Call `project_list`. A project with `core_v2` false cannot run missions: mention it in one line and skip it.
2. Call `mission_list` for each project on core v2.
3. Call `inbox` once to catch everything waiting on me across projects.
4. For a mission that has been working a long time, or a step that failed, call `job_log` with its `log_job_id` and say in a line what the crew is doing or why it stopped.
5. Report in plain language, in this order:
   - **Needs you**: one line per waiting request, with the project, the mission and what is being asked.
   - **Running**: one line per mission in progress, with its `headline` and `progress` ("Step 3 of 6: ...").
   - **Up next**: the queue in the order the crew will take it, so I can see what is ahead of what.
   - **Recently done**: finished missions, with the PR link or preview link when there is one.
   - **Heads-up**: a crew `notice` (provisioning, no crew, asleep, offline) with the tool that fixes it (`project_provisioning_status` you may call to show the phase; `project_provision` and `project_wake` you only offer, never call here), a preview that `failed`, and the common one: a mission waiting on me while others queue behind it, which `keep_working` would change (/shipitfam:settings).
6. If nothing is waiting or running, say so in one line and offer /shipitfam:new-mission.

Keep it short. No raw JSON. End by pointing at /shipitfam:inbox if anything needs me.
