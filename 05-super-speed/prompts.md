# 05 · Super Speed — prompts

**Context:** You joined Rook two weeks ago as PM on Dispatch.
Release 4.2 shipped on 12 August, just before you arrived, and
landed badly. You've spent three sessions finding out what went
wrong: the two piles of feedback that didn't agree, the numbers that
hid four people inside an average, and the code that settled it —
the shorter timeout is what did it.

At the end of the session, ask Claude Code to save the prompts you
wrote yourself below — not the starter prompt. The closing slide has
the exact prompt to paste. By Module 6 this file is a prompt library
built from your own questions.

---

## Round 1 — the brief for Helen

_Lab A step 1 is mine to write this session; the deck gives guidelines, not a starter. Output: [brief.md](brief.md)._

### 1.

Read 05-super-speed/director-request.txt. That is what Helen wants from me today. Then go back through 00-rook/company/ — the glossary, Priya's handover note, who-does-what.xlsx, the Slack thread — for the exact terms and names Rook actually uses, so this reads like it came from inside the company. Use 4.2-investigation.md, 03-rewind/prompts.md and 04-x-ray-vision/prompts.md for findings rather than re-deriving anything. Do not use 4.2-regroup-brief.md; it is out of date.

Draft a one-page brief for what we would build instead of quietly changing the timeout back. Ground every section in one pair: The Undertow and his handler Desmond Okafor.

Their facts, to use rather than restate in the abstract:
- Six steady weeks: 11 to 13 offers a week, nine or ten taken.
- Release week: the phone rang 11 times, he caught 4. Worst miss rate in the fleet that week.
- Then 4 offers, then 1, then 1. He took none of the last three.
- Okafor filed T-005 on 19 Aug marked Low, then T-019 on 31 Aug marked High. Nothing changed between them.
- The Undertow filed T-013 himself from his phone on 26 Aug: "nothing again this week. starting to wonder if im still even in the system." That sentence is the brief's problem statement. Quote it.

From Module 4, and the brief has to be built on these rather than general "make it visible" language:
- Nothing in the routing code logs anything. Seven distinct outcomes produce no event at all, including a responder ranked below the fold and a callout nobody took. There is no log, emit, notify or audit call in the folder.
- All four crossed 0.2 in release week. A score-crossed-threshold alert to the handler would have surfaced them by 16 August. Okafor filed Low on the 19th and High on the 31st and nothing happened in between.
- If we reset him it has to be to 1.0, not to NEUTRAL_SCORE. Everyone else is pinned at the ceiling, so 0.5 leaves him below all fifteen others for seven more perfect weeks.
- record_declined is called with only the responder. offer.py already knows whether it was a decline or a timeout, whether the phone ever acknowledged the push, his position in the list, and how long the callout had been open, and throws all of it away. That is why nobody can answer T-013. One line per offer would make it answerable.

Helen opens by saying engineering could ship the small fix this afternoon, and that she does not want that. So the brief has to earn its place: open with one short paragraph on what the quick fix would and would not do. Reverting the timeout to 90 seconds helps The Undertow catch the one offer he gets a week. It does not give him more offers, because his score stays where it is and the score decides who gets asked. Say that plainly and early, because it is the reason Helen is asking for anything at all.

Wen's 2019 note asked whether the score should ease back toward neutral on its own. Helen wants that come back to properly, so take a position rather than restating the question. Answer it: yes or no, and what follows either way. Note that decay alone would not lift The Undertow now, at roughly 0.02 a day against 0.12 a miss, so it is a fix for the next person and needs pairing with something for this one.

Three sections, in this order:

1. Who it is for. The Undertow and Okafor. Say exactly what each can see today when a callout goes to someone else. The answer is nothing, so be specific about what nothing means for each: Okafor watching a card that never changes, The Undertow with a phone that does not ring and no way to tell why.

2. What changes for them. What Okafor would notice on the console, and what The Undertow would feel differently about on his phone. Tie it to the trap: from the floor he needs 13 accepts in a row to reach where everyone else sits, 7 to reach a neutral that is still below all of them, and he gets about one offer a week to do it with. T-013 should have been answerable the day he filed it.

3. What it deliberately does not do. Explicit: does not change the ranking weights, does not silently reset anybody's score, does not promise him work, and is not a setting somebody flips.

Keep it rough. Helen said twice she would rather see something real than something tidy: no executive summary, no headings that exist to look complete, no polish passes. One page, plain sentences, and stop.

Nothing that cannot be evidenced from the investigation. Where something needs Wen, Marcus or Ravi to confirm it, say so in the brief rather than asserting it. The deploy-reset theory is a hypothesis, not a fact, so it belongs in open questions if at all.

---

## Round 2 — is the brief enough to build from

_Lab A step 2. Three questions aimed at what an engineer would refuse to start without. Run in this order: the first can invalidate the other two._

### 2.

Read 05-super-speed/brief.md as the engineer who would have to build it. Start with the thing that blocks everything else: the brief assumes a responder's score is a durable number you can watch cross a threshold, but history.py keeps _scores in memory and never saves it. Work out what breaks in the brief if the score resets on every deploy. Does the 0.2 alert become meaningless? Does the lift survive until Friday? Tell me which parts of the brief are unbuildable until Wen answers that, and which I could build anyway.

### 3.

The brief says somebody lifts The Undertow's score and it gets written down. That is a sentence, not a spec. Tell me what an engineer still needs decided: who is allowed to do it, Okafor or only Marcus, what value it sets, whether it can be undone, what the record of it looks like and who can see it, and what happens if the same responder is lifted twice in a month. Then tell me which of those I can decide myself and which need Helen or Wen.

### 4.

Take the brief through Kip, who handles Meteor Mite and The Gale in the same city, one starved and one overwhelmed. The brief assumes one handler watching one card. Tell me what it does not say: who the alert goes to when a handler has several responders and what stops it becoming noise, what The Gale sees when Meteor Mite is lifted and starts outranking him, and who in the brief is named as the person who loses callouts. Do not add features. Name the decisions the brief is silent on and who has to make each one.


---

## Round 3 — make the fix visible for him, not for a generic user

_Lab B step 2, run against [prototype.html](prototype.html)._

### 5.

The prototype shows him being starved and then lifted. It does not show the other thing that happened to him. On 31 August, after two weeks of nothing, one callout finally came and it was gone before he finished reading who it was for. His handler filed T-019 about it that day. Build that moment on his phone: the offer arriving, the sixty seconds running out, and what he is left looking at afterwards. Then show what this design puts on that screen instead of nothing.

### 6.

Right now the phone only has good news on it: you are still active, and later, you are back in the order. Build the honest version. It is a week in September, he has been put back in the order, and still nothing came up near him. What does the screen say then? If it cannot say anything true and useful on a bad week, it is a demo rather than a product, and I would rather find that out now.

### 7.

Put the two Augusts side by side for The Undertow, same dates, same rows. On the left, what actually happened: 12 August he is second in the order, 16 August he is fourteenth, Okafor files T-005 as Low on the 19th, The Undertow writes to us himself on the 26th, T-019 goes in as High on the 31st, and nothing changes at any point. On the right, the same month with this built: the alert on the 16th, Okafor acting, his phone answering him on the 26th. One person, one month, twice. That is the screen I want Helen looking at.
