---
description: Review a finished ShipItFam mission (preview, PR) and ship it, send it back for changes, or hold it.
argument-hint: "[project name or mission title]"
---

Help me review and ship finished ShipItFam work. Follow the shipitfam skill.

Which one: $ARGUMENTS

1. Find the mission. Call `project_list`, then `mission_list` for the project (every core-v2 project when none was named; legacy projects have no missions). Candidates are missions waiting on "Ship it?" (an open request whose body type is `ship`) and recently done missions with a `pr_url`. If more than one fits, list them and ask me which. If none fits, say what is waiting instead and stop.
2. Call `mission_get` and show me what I am about to ship: the goal, a one-line summary per step, and any assumptions the crew flagged.
3. Give me the ways to look at it: the preview link (the project's preview bar `url`, or the `open_preview` action), and the PR link or diff when there is one. Say if the preview is not ready or failed. An `app:` link is a screen in the ShipItFam app: tell me where to open it.
4. If the project's `push_mode` is `off`, nothing can ship: say so, and offer to link a repo (`project_update` with `repo_url`; the first remote switches `push_mode` to `manual`) only if I want that.
5. Ask me what to do: **Ship**, **Revise** (and what should change), or **Don't ship**. Do nothing until I answer. Never ship on my behalf.
6. Carry it out with `action`: the card's ship id, its revise id with my words in `text`, or its don't-ship id. Then re-read the mission and report the result: the PR link or branch after a ship, or the fix-up step the crew started after a revise.
