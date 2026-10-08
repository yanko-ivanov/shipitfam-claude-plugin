---
description: Queue a ShipItFam mission. Say what you want built and it lands at the end of the project's queue.
argument-hint: "[project name:] what you want built"
---

Queue a ShipItFam mission for me. Follow the shipitfam skill.

What I want: $ARGUMENTS

1. Call `project_list`. Pick the project: the one I named at the start of the request, else the only project if I have just one. If it is ambiguous, ask me which. Never guess a project.
2. Check the project's `core_v2` flag.
   - I have no project at all: missions need a project created from a starter. Call `project_starter_list`, show me the few starters that fit what I asked for (title and one line each), recommend one and say why, and ask which one I want and what to call the project. Wait for my answer, then call `project_create` with `name` and `starter_key`, and carry on with the new project's `id`. Do not create a project I have not agreed to. Follow the "Create a project" part of the skill for the result and the errors.
   - The project I picked has `core_v2` false: it cannot run missions. Say so and offer a new project from a starter. Do not work around it.
3. Call `mission_create` with:
   - `title`: my goal in my own words, under 200 characters.
   - `text`: any extra detail I gave. Do not pad it or reinterpret it.
   - Leave `quick` off by default, so the crew writes a spec and a plan. Use `quick: true` (with an `agent_type` from `project_agent_type_list` if I asked for a certain kind of worker) only when I asked for a small change with no planning.
   If my request is too vague to make a goal out of, ask one short question first.
4. Call `mission_list` for the project and find the new mission in it, so you can tell me where it stands.
5. Tell me in three lines or fewer:
   - what was queued, and where: how many missions are ahead of it (they run oldest first, one at a time, and cannot be reordered, only cancelled);
   - what the crew does first, and whether it will stop for me: questions from the spec, and a plan to approve when the project has `approve_plan` on (see /shipitfam:settings and /shipitfam:inbox);
   - any crew `notice` that `mission_list` still shows, with the tool that fixes it. Offer it and wait for my yes before calling `project_wake` or `project_provision`.

Do not wait around for the crew to finish.
