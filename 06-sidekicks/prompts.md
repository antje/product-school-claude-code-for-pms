# 06 · Sidekicks — prompts

**Context:** You joined Rook two weeks ago as PM on Dispatch.
Release 4.2 shipped on 12 August, just before you arrived, and
landed badly. You've spent five sessions on it: finding out what
went wrong, then building the fix nobody had — a working prototype,
by hand, in one sitting, for one room.

At the end of the session, ask Claude Code to save the prompts you
wrote yourself below — not the starter prompt. The closing slide has
the exact prompt to paste. By Module 6 this file is a prompt library
built from your own questions.

---

## Round 1 — the skill

_Built from the starter, with criteria 5 and 6 added. Skill: [.claude/skills/review-checklist/SKILL.md](../.claude/skills/review-checklist/SKILL.md). Run 1 on my own brief: [run-1-own-brief.txt](run-1-own-brief.txt), 5 flags. Run 2 on the four template briefs: [run-2-template-briefs.txt](run-2-template-briefs.txt), matched the answer key, but was run after reading it._

**The starter, as the course gave it** (for reference; not mine):

> I want to save a habit as something I can reuse. When I review a brief before it goes any further, here's what I actually check for:
> - It names who owns it
> - It says how we'll know it worked
> - The scope at the end matches the scope at the start
> - It explains the problem before it proposes a fix
>
> Turn that into a skill called review-checklist, so I can point it at any brief and get the same check every time, without me explaining it again.

**The version I used**, with my two criteria added:

### 1.

I want to save a habit as something I can reuse. When I review a brief before it goes any further, here's what I actually check for:
- It names who owns it
- It says how we'll know it worked
- The scope at the end matches the scope at the start
- It explains the problem before it proposes a fix
- Every number used as evidence says where it came from, so someone could reopen the source
- Anything not yet known is written as a question, not as a fact

The last two are mine. Four modules of this course were spent catching numbers that sounded right and cited nothing, and a guess written as a finding that ended up in a committed file. Turn that into a skill called review-checklist, so I can point it at any brief and get the same check every time, without me explaining it again. Report one line per criterion and quote the words behind every flag.

---

## Round 2 — the peer check

_Lab step 2, the swap. Run on Melissa's Module 5 brief; her brief and the results stay outside this repo because the brief is hers. Result: 3 flags, owner, unsourced evidence numbers, and a modelled figure stated as fact._

### 2.

Can you do the peer check, run the skill on this one: brief-melissa.md

---

## Round 3 — the schedule

_The deck's starter prompt, recorded as the switch that turns the schedule on. Documented in [schedule.md](schedule.md) rather than run, so no live job was created._

### 3.

Schedule review-checklist to run every Monday morning, and let me know what it finds. Nothing needs to be ready for it to fire today. I'm setting the habit, not waiting on the result.

---

## Round 4 — the final report

_Written against the deck's five requirements for the report and the course's four grading criteria. Output: [final-report.html](../final-report.html). The first version, built before this prompt existed, is kept as [final-report-v0.html](../final-report-v0.html)._

### 4.

Read CLAUDE.md, 4.2-investigation.md, all six prompts.md files, 05-super-speed/brief.md and 06-sidekicks/. Then build final-report.html at the repo root: a presentation I can open in front of the class and walk through in five minutes, and that Helen could read on her own afterwards. It tells one story. It is not a tour of the repo.

Order it like this:

1. What I'd do about 4.2, first, before any evidence: the asks in order, what each one fixes, and what I'm deliberately not doing and why.
2. What 4.2 did, in numbers people can take in at a glance, with one chart from callout-history.csv that shows the four responders against everyone else.
3. How I found it, one section per module. Each section names the technique the module taught, gives one real finding, and quotes the exact prompt I wrote that produced that finding, verbatim, from that module's prompts.md. The prompt must have produced the finding shown. If my strongest prompt and my strongest finding don't belong together, choose a pair that does.
4. The fix, shown and not described: embed prototype.html so it can be clicked inside the page, and say in a sentence what to click first.
5. How my review-checklist skill keeps this from happening again. Make the link to 4.2 explicit: the metric everyone watched, fleet acceptance, recovered while four people disappeared, and that is exactly what my success-measure criterion now rejects. Report the skill's runs honestly, including anything run by hand or after reading an answer key.
6. What I got wrong and how I caught it.
7. What's still open, who has to answer it, and what answer would change the plan.

Every heading states its point, not its topic. Every number comes from a file in this repo and says which one; recompute each one from the source before it goes in, and leave out any you can't trace. Anything modelled or unconfirmed says so beside it.

Make it look like a product people would want to use, not a document: large numbers, real charts, the prototype live on the page, and one moment of play that comes from the story itself, not decoration. Keep one visual identity with the prototype. It works in light and dark mode and on a phone. Plain English, short sentences, no dashes, first person.

Make it a single self-contained file that works opened locally and on GitHub Pages. Open it in a browser once, fix anything broken, then commit and push.
