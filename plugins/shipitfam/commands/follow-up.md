---
description: Follow up on a ShipItFam mission, steer the crew while it works, or leave it a note. Asks for changes on finished work.
argument-hint: "[mission title or project:] what to change"
---

Send a follow-up to my ShipItFam crew. Follow the shipitfam skill.

What I want: $ARGUMENTS

1. Find the mission. Call `project_list`, then `mission_list` for the project I named (every core-v2 project if none was named). Match the mission by the words I used. If more than one fits, list them (title and chip) and ask me which. If none fits, say so and offer /shipitfam:new-mission instead.
2. Call `mission_get` on it and read its `chip`, its steps and its `can`. If I want a change to one step, find that step's `id`. If I did not say what to change, ask one short question.
3. Choose how the follow-up goes, by where the mission is:
   - Done: `mission_comment` reopens it and queues the follow-up work.
   - Open and a step is running: with that step's `step_id` it stops the step and restarts it with my text. Without a `step_id` it is a note that every later step reads. Tell me which one you are doing.
   - A plan, question, risky command or review is waiting on me: a comment does not answer it and the mission stays blocked. Take it through /shipitfam:inbox instead (`request_answer`: a revision carries what to change).
   - "Ship it?" is waiting: a comment on a finished step is only kept as a note. Revise the ship request through /shipitfam:inbox so the change lands before it ships.
   - Cancelled: a comment is only a note and does not reopen it. Offer /shipitfam:new-mission.
4. Call `mission_comment` with `mission_id`, `text` (my words, written as I would say them to the crew) and `step_id` only if my point is about that step.
5. Relay the server's `message` and say what now happens: the mission is reopened and queued where, the running step restarts, a fix-up step was added, or it stays a note (`note_only`). Claim no more than it says.

Do not wait around for the crew to finish.
