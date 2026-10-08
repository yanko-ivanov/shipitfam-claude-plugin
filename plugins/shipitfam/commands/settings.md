---
description: See and change how much your ShipItFam crew asks you, per project. Plan approval, risky commands, pauses after each step, and keep working.
argument-hint: "[project name] [what to change]"
---

Show me, and change if I ask, how much my ShipItFam crew checks with me. Follow the shipitfam skill.

Scope: $ARGUMENTS

1. Call `project_list`. Pick the project I named, or the only project if I have one. If it is ambiguous, ask me which. A project with `core_v2` false has no settings to change: say so.
2. Call `project_get` and show me the four settings of that project in plain words, each with what it does right now:
   - **Plan approval** (`approve_plan`): the crew stops after planning and waits for my yes before it writes code. Off, it runs the plan straight away.
   - **Risky commands** (`approve_risky`): the crew asks before a shell command outside a short safe list (build, test, lint, git commit). Off, it asks only for the irreversible ones (rm -rf, sudo, git push, publishing, ssh, docker).
   - **Pause after every step** (`pause_after_step`): the crew stops after each work step with "Step N done. Continue?" so I can review each one.
   - **Keep working** (`keep_working`): while a mission waits for me, the crew moves on to the next one in the queue. Off, it waits for my answer first.
3. If I did not say what to change, ask. You may suggest a mix for what I describe, for example "tight": plan approval on, risky commands on, pause after every step on, keep working off; or "hands off": plan approval off, pause off, keep working on, risky commands still on.
4. Before you change anything, say in one line what the change will do. Turning plan approval or risky-command approval off needs me to have asked for exactly that. If my wish is broad ("make it more autonomous"), propose the specific flags and wait for my yes. Never loosen a setting on your own.
5. Call `project_settings_set` with only the flags that change, then read back the values it returns and confirm them in a line.

Whether finished work is pushed to a git remote is not one of these four: it is the project's push mode (`project_update`). Mention it only if I ask.
