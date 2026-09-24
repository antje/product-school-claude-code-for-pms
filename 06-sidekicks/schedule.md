# Scheduling review-checklist

Documented, not switched on. Turning it on creates a recurring cloud agent on
my own Claude account, running every week against a repo built for a
fictional scenario. That is the wrong thing to leave running for coursework,
so this file is the schedule written down in full and the one prompt that
turns it on.

## The prompt that turns it on

```
Schedule review-checklist to run every Monday morning, and let me know what
it finds. Nothing needs to be ready for it to fire today. I'm setting the
habit, not waiting on the result.
```

## What it would do

| | |
|---|---|
| **When** | Every Monday, 03:00 UTC, so the result is waiting at the start of the week in Chicago and Berlin |
| **What it runs** | `review-checklist`, from `.claude/skills/review-checklist/SKILL.md`, unchanged |
| **On what** | Every `.md` and `.txt` in `06-sidekicks/briefs/`, plus `05-super-speed/brief.md` |
| **Where it writes** | Overwrites `06-sidekicks/scheduled-run-output.txt` with that week's report |
| **Who hears about it** | Me, with the closing line: briefs checked, briefs flagged, total flags |
| **What it never does** | Edit a brief. The skill reports only; the author decides what to change |

## What a week looks like

`scheduled-run-output.txt` is the shape of a real run: four briefs, four
flags, one each, every flag quoting the words behind it. Run 2 of my skill
matched it, but I ran it after reading that file, so the match proves
little. The real test is the skill running in a session opened in this
repo, without the answer key in view.

## Why it earns a schedule

The failure this course traced was a check nobody owned, so it only
happened when somebody went looking. On a schedule, the check happens
whether or not anyone is paying attention that week.
