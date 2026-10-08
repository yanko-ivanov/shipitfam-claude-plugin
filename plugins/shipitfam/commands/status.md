---
description: Where your ShipItFam projects stand. What needs you, what is running, what just finished.
argument-hint: "[project name]"
---

Give me a status report on my ShipItFam projects. Follow the shipitfam skill. This is read-only: do not answer, approve, cancel or start anything.

Scope: $ARGUMENTS

If that names a project, report on that one project. If it is empty, cover every project.

1. Call `project_list`. Note each project's `core_v2` flag.
2. For each project on core v2, call `mission_list`. For a legacy project, call `helm_feed` with its `project_id` instead.
3. Call `request_list` once to catch everything waiting on me across projects.
4. Report in plain language, in this order:
   - **Needs you**: one line per waiting request, with the project, the mission and what is being asked.
   - **Running**: one line per mission in progress, with the current step ("Step 3 of 6: ...").
   - **Recently done**: finished missions, with the PR link or preview link when there is one.
   - **Heads-up**: any crew notice (provisioning, crew asleep, crew offline) or failed preview.
5. If nothing is waiting or running, say so in one line and offer to start a mission with /shipitfam:new-mission.

Keep it short. No raw JSON. End by pointing at /shipitfam:inbox if anything needs me.
