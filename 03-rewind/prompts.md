# 03 · Rewind — prompts

**Context:** You joined Rook two weeks ago as PM on Dispatch.
Release 4.2 shipped on 12 August, just before you arrived, and
landed badly.

Last session you read four conversations and every support ticket
since 4.2 — and found the two piles did not agree.

At the end of the session, ask Claude Code to save the prompts you
wrote yourself below — not the starter prompt. The closing slide has
the exact prompt to paste. By Module 6 this file is a prompt library
built from your own questions.

---

## Round 1 — can I trust the number?

_Goal: before putting one number in front of Helen, find out whether the file is real, whether the number holds up however you slice it, and whether there's an innocent reason for the zeros._

### 1.

Before I cite anything from callout-history.csv, check what it can be reconciled against. Search everything in 00-rook/company/ — the Slack thread, Priya's handoff, the one-pagers, release history — for any acceptance rate, weekly volume, or responder count that should match this file. Do the 76–77% pre-release acceptance and ~172 pings/week agree with any figure anyone at Rook has quoted? Does the 16-responder roster match who-does-what.xlsx? And does Marcus's 19 Aug offer of a "rough pull" describe this file — same date range, same columns? If nothing external agrees with it, tell me that, and tell me which single number I'd need Ravi to confirm.

### 2.

Recompute the starvation finding under every reasonable alternative definition and show me how much it moves. Baseline: 6 weeks pre-release vs 3 weeks vs the single last week before. After-window: including vs excluding release week. Metric: pings sent, pings taken, or share of total weekly volume. Threshold: what counts as "starved" — under 2/week, under 25% of prior, or zero? Then tell me the version of the number that's most conservative and still true, and whether any choice flips the four-responder list — does Ashgrove or Halfmoon join it, does anyone drop out?

### 3.

Argue that the four zeros are not caused by routing. Read availability.py and offer.py and tell me every way a responder could show 0–1 pings sent in a week other than ranking last: marked unavailable, on leave, device unregistered, out of region, capability tags changed, handler paused them. For each, say whether the CSV could distinguish it from starvation, and whether anything in the interviews or tickets rules it in or out for Farlight, The Undertow, Vesper and Meteor Mite specifically. Then: what one field, if added to this export, would settle it?

### What this round found

- **Nothing else in the folder backs up this file.** No document anywhere quotes an acceptance rate, a weekly total, or a count of responders. The team roster only lists staff. The file runs later than the rough pull Marcus offered on 19 August, so it isn't that either. We don't know who made it.
- **The handlers' numbers don't match the file's numbers, for anyone.** Seven tickets from six handlers give counts that disagree with the file, including one saying "one callout since the start of the month" when the file shows about fourteen. Where they *do* agree is on direction: the four starved responders went down, and both sources say so. Use the pattern, not the exact figures.
- **The four-responder list doesn't change however you slice it.** Twenty-seven different ways of measuring, every threshold except "literally zero," same four names. The most cautious true figure is a 62% drop (Meteor Mite). The next-worst responder is down 20% (Ashgrove). Nobody joins the list, nobody leaves it.
- **The acceptance rate is recovering because the four have stopped counting.** Take them out and the other twelve went from 77% before (six-week average) to 74% now. The four used to get 28% of all offers; now they get under 2%. They've effectively left the denominator.
- **There's no innocent explanation.** Being marked unavailable or out of area gives you exactly zero offers, not the one or two most of them still get. Changing someone's tags can't zero them out. There's no "pause" button. All four handlers say the responder was available and waiting; Kip's two responders live in the same city and one is starved while the other is overwhelmed. The one thing we can't rule out from the data alone is a dead phone, but three of the four have demonstrably received offers since.
- **The one field that would settle it:** hours available per responder per week. Dispatch already records this; Supply reads it every day.
- **The number for Helen:** four of sixteen responders now get 62–100% fewer offers than before 12 August, while total offers are down only 4%. Say it as a ratio, not as "12 offers a week," until Ravi confirms the file.

---

## Round 2 — what only the two sources together can tell me

_Goal: the tickets and the data file were written by different people for different reasons. Ask what neither can answer alone._

### 4.

Seventeen tickets say "silence" for responders the file shows at record volume. Instead of picking a side, work out what each source would have to be measuring for both to be honest. Test three: (a) `pings_sent` counts every responder the callout *cascaded through*, so a busy responder's 18 includes 15 offers that withdrew in 60s before the phone showed anything; (b) the file is per-handler or per-region, not per-responder; (c) the tickets are about *accepted callouts* and the file's `pings_taken` is something else. For each, say what pattern the file would show if it were true, then check the file for that pattern — e.g. under (a), `sent − taken` should have jumped for the busy group after 12 Aug while `taken` stayed flat. Tell me which one the numbers support.

### 5.

Every "silence" ticket gives a duration — "six days," "ten days," "two weeks," "almost a month." For each of the 17, convert the claim into the number of callouts the handler is implying (zero, or one) and put it next to the file's `pings_taken` for the same span. Then group the tickets by how big the gap is. If the gaps cluster — say, everyone's implied count is `taken` minus a constant, or a constant fraction — the two sources are measuring the same thing on different scales and the tickets are usable as data. If the gaps are random, one source is noise. Show me the table.

### 6.

Cross the two sources into a 2×2: responder appears in tickets vs. not, and responder's file rows show a drop vs. not. Fill all four cells with names. Then, for the "dropped, no ticket" cell and the "ticket, no drop" cell specifically, read the interviews and the ticket text for why — are the silent-and-hurt ones handled by someone who said filing goes nowhere? Are the loud-and-fine ones handled by someone comparing against last year? Tell me what fraction of the real damage the ticket queue would have shown Nadia, and what fraction of the ticket queue is describing damage the file can't see.

### What this round found

- **There's no way to read the file that makes both it and the tickets true.** If "sent" were padded with offers that never reached the phone, the "taken" count for busy responders should be flat; it went up. If the file were grouped by handler, Kip's two responders would share numbers; they don't. "Taken" really does mean accepted. So the file and seventeen tickets flatly disagree.
- **The tickets don't describe a smaller version of what the file shows. They describe zero.** For the twelve busy responders, the handler's implied count is essentially nothing in 14 of 15 tickets, while the file says they were taking 7 to 29 callouts in the same span. For the four starved responders, three of four tickets match the file to within one callout. So the tickets confirm exactly the group the file already showed collapsing, and nothing else.
- **The four boxes:** hurt and wrote in: Farlight, The Undertow, Ashgrove, Halfmoon. Hurt and silent: **Vesper and Meteor Mite**, who only surfaced because Sofia happened to interview their handlers about the console. Neither handler thought of it as something to report; a quiet week doesn't feel like a bug. Fine (per the file) but wrote in: eight responders. Fine and silent: Bulwark, The Gale.
- **Nadia's queue showed four of the six hurt responders and missed the second- and third-worst.** Of 25 tickets, 4 describe harm the file confirms, 4 describe one lost offer the file makes plausible, and 17 describe harm the file can't see.
- **Something nobody has mentioned.** The weekly "taken" total is the closest thing we have to a count of callouts that got filled. It went from 132 a week to 96, 104, 108, 120 after release, while offers stayed flat. If that's right, about 25 incidents a week found nobody in the three weeks after 4.2. No ticket, interview or Slack message mentions an incident nobody attended. Either the file is wrong about this too, or it's the real question for Helen.

### The insight

**The two sources agree on the four starved responders and on nothing else, and that's the only reason to believe the number.** Every normal way of checking data against feedback fails here. But the agreement is independent: Dot and Kip described Vesper and Meteor Mite without knowing a release had happened, and the file put those two at the bottom without anyone filing a ticket. Two sources that disagree about twelve people and agree about four, for different reasons, are stronger evidence for the four than either alone.

Two things follow. Ticket counts don't measure who's hurt: the queue missed two of the six worst-off, because nothing happening doesn't generate a ticket. And the file may be hiding a bigger problem than the one being chased: if "taken" counts filled callouts, incidents are going unanswered, and nobody has said so.

---

## Round 3 — one person's month

_Goal: pick one responder who went quiet and follow them week by week. Then ask what I'd need to know if I had to explain it to them._

### 7.

Follow The Undertow through the whole file, one week at a time, from 29 June to 31 August. For each week give me the row (pings sent, pings taken), then say in one or two plain sentences what that week was like for him — how many times his phone went off, how many he took, and whether that's normal for him. Mark the week of 12 August. Where a ticket from his handler Desmond Okafor (T-005, 19 Aug; T-019, 31 Aug) or his own mobile ticket (T-013, 26 Aug) falls in a week, quote it next to the row and say whether it matches. End with what the file says happened to him and what it can't tell me — no jargon, no percentages, written the way I'd explain it to Helen in a hallway.

### 8.

Take The Undertow's release week: 11 offers, 4 taken. Using the numbers in config.py, walk his score forward one offer at a time, starting from where six weeks at 9-of-12 would have put him. Show the score after each accept and each miss that week, then through the 4/1, 1/0, 1/0 weeks. Tell me where he crossed below the other responders and whether, at any point after 12 August, there was a week where saying yes to everything offered would have been enough to climb back. If the answer is no, say so plainly: I need to be able to tell him it wasn't something he did.

### 9.

Assume nothing changes: same code, same weights, same 60-second timeout, and The Undertow says yes to every offer he gets from here on. Using the score arithmetic in history.py and his current rate of about one offer a week, how many weeks until he's back where he was in July? Then answer the same question if someone resets his score to NEUTRAL_SCORE today, and if the timeout goes back to 90 seconds but his score isn't touched. I want three numbers I can say out loud, and I want to know if one of them is "never."

### 10.

Put The Undertow's ten rows next to the other three who collapsed (Vesper, Meteor Mite, Farlight) and next to the two who didn't but dipped (Ashgrove, Halfmoon). Same weeks, same columns. For each of the six, mark the release week and say whether their release-week miss rate was above or below the cohort's 46%. Tell me whether the four who collapsed had a worse release week than the two who didn't, or whether they had a similar week and something else separated them. Then read Okafor's tickets against Pruitt's (Farlight) and Kip's interview (Meteor Mite): are these four handlers describing the same experience in the same order, or four different stories?

### 11.

Once somebody's gone quiet, what would have to happen for them to start getting pinged again?

### What this round found

_Caveat on all the score arithmetic: the code in the folder is stubs and the file has no score column. The constants in `config.py` were applied to his rows. The mechanism is high-confidence; the decimals are a model._

- **The Undertow's month, in one paragraph.** Through July, a dozen offers a week, nine or ten taken — one of the steadier records in the file. Release week the phone still rang eleven times but he caught only four. The week after, it rang four times. Then once a week, and when it rang he couldn't get to it in the 60 seconds. His handler's three tickets and his own land on exactly the three weeks the file shows him disappearing, and each says what the row says. (CSV lines 16, 32, 48, 64, 80, 96, 112, 128, 144, 160.)
- **Before 12 August the score was a tie for everyone.** Anyone accepting more than 60% climbs to the 1.00 ceiling and stays. All sixteen were at or above 60% every week all summer (one exception: Meteor Mite at exactly 6/10 on 13 July, which nets zero and clears the next week), so all sixteen sat at 1.00 going into release week. The recent-acceptance component decided nothing. 4.2 didn't change how it worked; it created the first week in years where it mattered.
- **He didn't do anything the others didn't do; he did slightly more of it in the one week it counted.** Cohort missed 46% of offers in release week; he missed 64%, the worst in the file. Seven misses at −0.12 each took him from 1.00 to somewhere between 0.16 and 0.48 depending on the order they arrived. The same order-dependence applies to everyone: Farlight and Meteor Mite could have ended anywhere from 0.28 to 0.60, and even busy responders like Nightwell (0.16–0.88) or Stormwrack (0.28–0.92) could have landed low if their accepts came before their misses. What separates him is that his *ceiling* for the week, 0.48, sits below everyone else's floor except three. The file cannot see the order, so it cannot say who actually finished lowest.
- **He could have climbed back only by never missing again for four straight weeks**, on offers that reach him last in the cascade with a minute to answer. He took 1 of 4, then 0 of 1, then 0 of 1, and hit the floor around 31 August. At one offer a week, each miss costs a week and a half of perfection to undo.
- **When it ends, three numbers:** nothing changes and he says yes to everything: 13 weeks (early December). Reset to `NEUTRAL_SCORE`: 7 weeks. Timeout back to 90s, score untouched: still 13; the timer helps him catch the offer, not get more of them. And **"never" is a real answer**: 2 in 5 missed is exactly break-even (3 × 0.08 = 2 × 0.12), and his actual rate since release week is 1 taken of 6 offered, so the score falls faster than it rises.
- **`NEUTRAL_SCORE` isn't neutral.** Everyone else is at 1.00. Resetting him to 0.50 leaves him below all of them for seven more perfect weeks. The reset that fixes it is to the ceiling, where the pack is.
- **Not just him, but not a clean story either.** Three of the four who collapsed had the three worst release weeks in the file (60–64% missed). Vesper had a 50% week, identical to Sgt. Bulwark's, and Bulwark got 10 offers the next week while Vesper got 5. The score alone doesn't separate them; something the file can't see does, most likely proximity, now 60% of the ranking and absent from the data.
- **Four handlers, one story, two of whom never told anyone.** Okafor filed three tickets in sequence. Pruitt filed once after checking availability herself. Kip told Meteor Mite to hang in there while watching The Gale, same city, get overwhelmed. Dot noticed she was cooking for someone home all week. Same shape, same order; only two thought it was reportable.
- **What would get someone pinged again.** Someone has to touch the score; nothing the responder does is enough. In order of how much it fixes: (1) set the score to 1.00, where everyone else is — immediate, a data change not a release; (2) set it to 0.50 — sounds like a reset, isn't, unless the whole cohort is reset too; (3) add decay — fixes the next person, doesn't lift these four unless it's fast, and drifts the pack down too; (4) stop scoring timeouts as declines — prevents recurrence, lifts nobody; (5) timeout back to 90s — helps catch the one offer a week, doesn't add offers. What doesn't work: waiting for September, re-marking them available, them accepting everything, or reverting the reweight (they're still last on the score, so still last).

### The insight

**The score that's holding four people down did nothing at all before 12 August, and the people it's holding down can't move it.** It sat at the ceiling for everyone, a tie, for as long as anyone accepted more than 60% of offers. The timeout cut produced one week where nobody did, the tie broke, and the ones who landed lowest were asked last from then on — too rarely to earn their way back. It isn't a punishment for declining; it's a ranking that happened to be a tie until it wasn't. The fix for the four is one data change this week. The fix for the next four is Wen's 2019 question, finally answered.

---

## What the evidence shows, and how sure I am

Everything below is graded. **High** means it's directly in the rows or the code and I checked it. **Medium** means it follows from high-confidence facts but has a gap. **Low** means it's a hypothesis worth testing, not a finding.

### High confidence — in the data or the code

1. **Four responders went from ~12 offers a week to 0–1.** Vesper, Meteor Mite, Farlight, The Undertow. Same four names under 27 ways of measuring it. Two sources that don't know about each other agree. (CSV lines 114–161; interviews with Dot and Kip; tickets T-013, T-018, T-019.)
2. **Total offers barely changed.** 172 a week before, 162–165 after. The offers went to fewer people. (All 160 rows.)
3. **The spread exploded.** Busiest vs quietest responder: 2.5-to-1 for six weeks, then 6, 20, 21-to-0. (All 160 rows.)
4. **The acceptance rate recovering is partly the four leaving the denominator.** They used to receive 28% of offers; now under 2%. Without them, the other twelve went 77% → 74% (six-week pre-release average to last week). (Arithmetic on the rows.)
5. **Everyone dipped in release week.** All 16 responders were below their own average that week; acceptance 77% → 54%. The starvation started the week *after*. (Rows 98–113.)
6. **The score can't recover on its own.** `history.py`: no decay; miss costs 0.12, accept earns 0.08; timeout and decline are scored identically; floor is 0.0; `dispatch()` asks top-down and stops at the first yes. Wen's 2019 note asking whether it should ease back is still open. (Read the code.)
7. **The glossary describes a system that doesn't exist.** It says the score "recovers" and that a decline is "distinct from a timeout in the data." Neither is true of the code.
8. **The file and the tickets disagree about the other twelve.** Seventeen tickets say silence; the file says record volume for the same people, same weeks. No reading of the file reconciles them.
9. **The ticket queue missed two of the four collapsed responders.** Vesper and Meteor Mite filed nothing. Their handlers never mention support.
10. **The file has no provenance.** Nothing in the folder quotes a figure it could be checked against.

### Medium confidence — follows from the above, with a gap

- **The four are stuck because of the scoring ratchet.** The code says a responder at the floor can't earn their way back, and the rows show four responders who didn't. The gap: the code in the folder is stubs, the file has no score column, and I can't see their actual scores. The mechanism fits; I haven't observed it.
- **The trigger was the 4.2 timeout cut, not the routing reweight.** Everyone dipping at once in release week fits a timer change (it hits everyone) better than a proximity change (it would hit remote responders). The gap: both shipped the same day, and I have no location data. If the four happen to be the most remote responders, the reweight alone could explain their fall. This is my best guess, not a finding.
- **The four were available and reachable.** Pruitt says she checked Farlight's window; Dot and Kip describe waiting responders; Undertow received an offer on 31 Aug. The gap: no availability data in the file, and nothing for Farlight after 12 Aug beyond her handler's word.
- **"August is seasonal" doesn't explain this.** Flat total volume rules out "fewer callouts in August." The gap: ten weeks of one year, no prior-year data to compare against Priya's claim.

### Low confidence — worth testing, not worth citing

- **Callouts are going unfilled.** Weekly "taken" fell from 132 to 96–120. *If* every filled callout produces exactly one "taken" and this file covers every responder, ~25 incidents a week found nobody. Both ifs are unverified and nobody at Rook has mentioned an unattended incident.
- **The fix is one setting.** Splitting timeout from decline stops the ratchet from firing again; it does not lift the four out. Resetting their scores does that, and I can't tell from the folder whether a reset exists.
- **Ashgrove and Halfmoon are "hurt."** Down 20–27%, a real middle band, but where the line between hurt and normal variation sits is a choice I made.

### What I'm dropping

- "The shorter timer is the cause, not the reweight." I said this earlier. It's a medium-confidence inference and I stated it as fact.
- "The fix is small." Same.

## The number for Helen

_If the Director of Product asked tomorrow what release 4.2 actually did to responders:_

> **Four of sixteen responders now receive 62–100% fewer callout offers than before 12 August, while total offers are down 4%.**

From `00-rook/data/callout-history.csv`, lines 114–161: Farlight, The Undertow, Vesper and Meteor Mite each averaged 11–14 offers a week for six weeks, then 3/1/0, 4/1/1, 5/2/1 and 4/2/1 in the three weeks after release, while total offers across all sixteen stayed within 4% of before. The 62% floor is the most conservative slicing, with release week counted as "after"; on the three post-release weeks alone the drop is 79–89%.

**What to ask her for:** three things, none of which is a rollback. Reset the four scores (high-confidence problem, mechanism in the code). Get Wen to confirm the ratchet is what's happening in production and say whether timeout should still score as a decline (medium: I've read the code, not the scores). Get Ravi to confirm the file, including the weekly "taken" totals (the one number that would turn a low-confidence worry into a finding).
