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

_Built from the starter, with criteria 5 and 6 added, as `review-checklist`; renamed afterwards to `review-product-brief`. Skill: [.claude/skills/review-product-brief/SKILL.md](../.claude/skills/review-product-brief/SKILL.md). Run 1 on my own brief: [run-1-own-brief.txt](run-1-own-brief.txt), 5 flags. Run 2 on the four template briefs: [run-2-template-briefs.txt](run-2-template-briefs.txt), matches the answer key exactly._

### 1.

I want to save a habit as something I can reuse. When I review a brief before it goes any further, here's what I actually check for:
- It names who owns it
- It says how we'll know it worked
- The scope at the end matches the scope at the start
- It explains the problem before it proposes a fix
- Every number used as evidence says where it came from, so someone could reopen the source
- Anything not yet known is written as a question, not as a fact

The last two are mine. Four modules of this course were spent catching numbers that sounded right and cited nothing, and a guess written as a finding that ended up in a committed file. Turn that into a skill called review-checklist, so I can point it at any brief and get the same check every time, without me explaining it again. Report one line per criterion and quote the words behind every flag.

### 2.

### 3.
