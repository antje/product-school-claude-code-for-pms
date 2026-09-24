# 01 · Origin Story: prompts

**Context:** Rook Industries makes software for superheroes and the
people who handle them.

There are two products. Rook Dispatch is the one that gets a
superhero to where they're needed when there's an emergency, it
works out who is close enough and free enough to help, and gets hold
of them. Rook Supply keeps a responder's equipment serviceable and
accounted for, so a handler is never guessing whether the gear will
hold.

You joined two weeks ago as PM on Rook Dispatch. Release 4.2 shipped
on 12 August, shortly before you arrived. Responders have stopped
answering their phones the way they used to, and complaints have
gone up sharply. You were not in the room for any of it.

At the end of the session, ask Claude Code to save the prompts you
wrote yourself below, not the starter prompt. The closing slide has
the exact prompt to paste. By Module 6 this file is a prompt library
built from your own questions.

---

### 1.

In 00-rook/code/dispatch-routing/, simulate routing.score() for Farlight, Meteor Mite, The Undertow, and Vesper using the pre-4.2 weights (0.45/0.40/0.15) versus the 4.2 weights (0.60/0.25/0.15), holding proximity and capability match constant. How much of their collapse in callout-history.csv is explained by the weight rebalance alone, separate from the shorter timeout?

### 2.

For Ironvale, Nightwell, Stormwrack, Sgt. Falkirk, The Drift, and The Longcast, whose offer volume in callout-history.csv is flat or rising despite tickets claiming a dry spell, look for anything in offer.py, the 4.2 changelog, or the tickets/interviews that points to a push-delivery problem (the 4.2 notes already mention a 'duplicate push notification on re-offer' defect). Is there a pattern by device, region, or capability tag that separates this group from the confirmed-starved group?

> **Correction, 10 Sept.** This prompt is left exactly as I ran it, but one
> claim inside it is false. There is no "duplicate push notification on
> re-offer" defect anywhere in the repo. The 4.2 changelog lists three items
> only: the ranking reweight, the timeout cut from 90s to 60s, and console
> filter persistence. "Mobile push reliability" was 4.1, on 16 June. The
> push-delivery theory this prompt was built on is also refuted by the data:
> Nightwell accepted 14 callouts in the week she reported hearing nothing for
> ten days. A phone that is silent cannot be answered fourteen times. See
> [4.2-investigation.md](../4.2-investigation.md).

### 3.

Draft a short brief for the 4.2 regroup: the two-population split, the capability-tag/proximity-weighting risk for specialists, and the decline-vs-timeout scoring question Marcus raised in Slack that history.py already answers. For each, state what data or code change would confirm or kill the hypothesis, so the meeting produces decisions, not more speculation.

> **Correction, 10 Sept.** "Already answers" understated it. `history.py:35-39`
> does score a timeout identically to a refusal, but the `TODO(wen, 2019)` I
> attributed that rationale to sits at lines 30-34 and asks a different
> question: whether the score should decay back toward neutral. It never does,
> and that is the mechanism, not a documentation gap.
