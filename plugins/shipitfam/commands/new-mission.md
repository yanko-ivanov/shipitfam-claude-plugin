---
description: Start a ShipItFam mission. Say what you want built and the crew takes it from there.
argument-hint: "[project name:] what you want built"
---

Start a ShipItFam mission for me. Follow the shipitfam skill.

What I want: $ARGUMENTS

1. Call `project_list`. Pick the project: the one I named at the start of the request, else the only project if I have just one. If it is ambiguous, ask me which. Never guess a project.
2. Check the project's `core_v2` flag.
   - I have no project at all: missions need a project created from a starter. Call `project_starter_list`, show me the few starters that fit what I asked for (title and one line each), recommend one and say why, and ask which one I want and what to call the project. Wait for my answer, then call `project_create` with `name` and `starter_key`, and carry on with the new project's `id`. Do not create a project I have not agreed to. Follow the "Create a project" section of the skill for the result and the errors.
   - The project I picked has `core_v2` false: it is an existing legacy project and has no missions. Create the work with `task_create` (title and description from my request) and start the crew with `session_start`. Tell me that is what you did and why, and offer to create a starter project (`project_starter_list`, then `project_create`) if I want missions.
3. On core v2, call `mission_create` with:
   - `title`: my goal in my own words, under 200 characters.
   - `text`: any extra detail I gave. Do not pad it or reinterpret it.
   - Leave `quick` off by default so the crew writes a spec and a plan I can approve. Use `quick: true` with an `agent_type` (see `project_agent_type_list`) only when I asked for a small change with no planning.
   If my request is too vague to make a goal out of, ask one short question first.
4. Call `mission_get` on the new mission and tell me in two or three lines: what was created, what the crew does first (for example "writes a spec and may ask you questions"), and how I will know it needs me. If the project has no running crew yet (its `provisioning_status` is not `ready`), say what that status means and that the mission waits until a crew picks it up. Point me at /shipitfam:status and /shipitfam:inbox.

Do not wait around for the crew to finish.
