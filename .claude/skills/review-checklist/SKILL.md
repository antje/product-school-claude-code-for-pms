---
name: review-checklist
description: Checks a product brief or one-pager for the six things a brief must cover before it goes any further. Use when asked to review, check or run review-checklist on a brief, a one-pager, or a folder of them. Reports one line per criterion and quotes the text behind every flag.
---

# review-checklist

Check a brief against six criteria and report what is missing. Same check
every time, whoever runs it.

## What to point it at

A single file, or a folder. For a folder, check every `.md` and `.txt` file
in it, one after another, and total the flags at the end.

## The six criteria

Check each one against the brief's own text. Pass or flag, nothing in
between.

1. **Names who owns it.** A named person or team who is responsible for the
   work getting done. Flag if the brief only says who has to *decide* things,
   or parks it for "whoever picks it up", or leaves the owner blank.

2. **Says how we'll know it worked.** Something observable that would move
   if the work succeeded, and would not move if it failed. Flag if the brief
   describes what gets built but never what changes as a result. Also flag a
   measure that could move for another reason: fewer complaints, for
   instance, also happens when the people affected stop complaining.

3. **The scope at the end matches the scope at the start.** Compare what the
   opening says is being built with what the rest of the brief says. Flag if
   items get added along the way, or if something the opening endorses is
   never placed in or out of scope.

4. **Explains the problem before it proposes a fix.** Flag if the solution
   appears before the reader knows what is wrong.

5. **Every number used as evidence says where it came from.** A number that
   supports the argument needs a source the reader could reopen: a file, a
   ticket, a named report, or one line pointing to where the evidence lives.
   Does not apply to numbers the brief is *proposing*, like a threshold or a
   target. Flag if evidence numbers appear with no way to check them.

6. **Anything not yet known is written as a question, not as a fact.** That
   includes results from a model, a simulation or a forecast: the assumption
   they depend on belongs beside them. Flag an assumption, estimate, modelled
   result or prediction stated as if it were observed. This is how an
   unchecked guess ends up in someone else's decision.

## How to report

For each brief, this exact shape:

```
<filename>
  owner named ................... yes | NO: flagged
  success measure ............... yes | NO: flagged
  scope stays bounded ........... yes | NO: flagged
  problem stated before fix ..... yes | NO: flagged
  evidence numbers sourced ...... yes | n/a | NO: flagged
  unknowns written as unknowns .. yes | NO: flagged
  -> N flags: <one line per flag, quoting the words that caused it>
```

Then one closing line: `<n> briefs checked, <n> flagged, <n> flags total.`

## Rules

- Quote the brief. Every flag names the exact words that caused it, so the
  author can find it in seconds.
- Do not fix the brief. Report only. The author decides what to change.
- Do not flag style, length or tone. Only the six criteria.
- If a criterion genuinely does not apply, say `n/a` rather than passing it.
