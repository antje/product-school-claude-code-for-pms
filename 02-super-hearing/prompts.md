# 02 · Super Hearing — prompts

**Context:** You joined Rook two weeks ago as PM on Dispatch.
Release 4.2 shipped on 12 August, just before you arrived, and
landed badly. Last session you built a context file and met the
company.

You still do not know what actually went wrong.

At the end of the session, ask Claude Code to save the prompts you
wrote yourself below — not the starter prompt. The closing slide has
the exact prompt to paste. By Module 6 this file is a prompt library
built from your own questions.

---

## Round 1 — after reading the interviews

_Goal: find one sentence trustworthy enough to post to the channel._

### 1.

Read 00-rook/data/callout-history.csv. For the five responders named in the interviews — Captain Vantage, Vesper, Sgt. Bulwark, Meteor Mite, The Gale — give me weekly offer volume, timeout rate, and decline rate, before and after 12 Aug. I want to know whether the three handlers who described "offer gone before he could answer" (Vantage, Vesper, Bulwark) show a timeout-rate change, and whether that's distinguishable from the two who didn't raise it. Tell me if the data does NOT separate them.

### 2.

Re-read the four interviews in 00-rook/feedback/interviews/ adversarially. For each finding, tell me whether the participant raised it unprompted or whether Sofia's question introduced the frame. Flag leading questions specifically. Also: is there anything in the transcripts or notes about how these four were recruited?

### 3.

Extract every dated or datable incident from the four interviews and the 25 tickets in 00-rook/feedback/tickets/. Plot them against the 12 Aug ship date. Is the vanishing-offer complaint still generating incidents after 25 Aug, or does it stop?

### What this round found

- **The data can't answer half the question.** `callout-history.csv` has only `pings_sent` and `pings_taken`. There is no timeout-vs-decline split anywhere in it.
- **Two of the three headline interview cases died.** Captain Vantage is up 39% in offer volume with acceptance flat at 74%; Sgt. Bulwark up 19%. The handlers who described offers vanishing have responders that are thriving. Vesper (−81%) and Meteor Mite (−79%) are the ones actually collapsing — and their handlers described quiet, not vanishing.
- **"3 of 4 raised it" was really 2 volunteered and 1 extracted** after Sofia asked twice. Dot's "quiet weeks" came from Sofia's own question — her reply opens *"[pause] Now that you mention it."*
- **Nothing anywhere records how the four were recruited**, so selection bias is unverifiable.
- **The complaints didn't stop.** Tickets run 13 Aug to 5 Sept with no let-up, so this isn't a release-week artifact.
- **Lesson:** counting across four interviews manufactures confidence the sample can't support. Four interviews are case studies, not a sample.

---

## Round 2 — interview integrity

_Goal: strip out what I already believed and see what the transcripts alone support._

### 4.

Set aside everything in the Working context about 4.2, the starved responders and the routing investigation. Re-read the four interviews as if that work had never happened and I knew nothing about the release. What do these four conversations actually support on their own? Tell me which of your groupings survive that and which only looked solid because of what I already knew.

### 5.

Take the vanishing-offer finding. For each of the three who raised it, tell me whether they witnessed it themselves or are relaying someone else's account, roughly when the incident they describe happened, and whether they volunteered it or were asked. Then tell me how strong that finding actually is on that basis.

### 6.

What would people in this situation be expected to complain about that none of the four mentioned at all? Not the things one person raised and the others didn't, but the things nobody raised. For each, say whether their silence means it isn't happening, or means it wouldn't be visible to someone in their role.

### What this round found

- **Read cold, the corpus is a console usability study with one safety issue buried in it.** Three of the four participants don't know a release happened; the number 4.2 never appears.
- **The best-supported multi-person finding is the alerting layer**, not the vanishing offers — three handlers, three distinct unmet needs, one subsystem. Zero tickets on any of it.
- **Halloran flips from footnote to headline.** Eleven days on a cracked vest plate with a responder in the field, a priority field that changes nothing, failure reports into a void. The only safety-of-life issue anyone raised, and it appears in no ticket.
- **"Feast or famine" collapses.** One handler's impression, hedged by him in the same breath, plus one prompted echo. It looked like a validated theme only because of what I already knew.
- **Provenance of the vanishing-offer finding: one real witness.** Dot watched it repeatedly. Ambrose's account degraded between his ticket and his interview — T-001 says he watched the clock himself, three weeks later he says he heard it afterward — and he misdated a 12 August incident as "two weeks ago." Halloran's is secondhand, undated and self-downgraded twice.
- **The silences matter more than the complaints.** Nobody mentions money, though two of these handlers support responders who lost ~80% of their work — handlers aren't paid per callout, so the economic damage is unsayable by everyone in the sample. Nobody mentions incident outcomes; they each watch one responder, not whether anyone got hurt. Nobody mentions the responder's phone app, because none of them use it — and that's where all of this actually happens. And nobody complains about support being slow, during a month tickets ran 3x normal, because two of them said outright that filing something is where it stops.

---

## Round 3 — after reading the tickets

_Goal: check whether the tickets tell the same story as the interviews._

### 7.

Nadia said in Slack on 18 Aug that callout tickets were running 3x normal, and on 26 Aug that the split was "roughly two thirds phone never goes off to one third gone before I could answer." I just independently grouped the 25 tickets in 00-rook/feedback/tickets/ as 14 silence to 9 vanished — which is almost exactly two-thirds/one-third. Check whether this pile is the complete set of callout tickets for the window or a curated subset: look at ticket numbering for gaps, date coverage, filing-rate per week against "3x normal," and anything indicating who assembled it. Tell me if I've just rediscovered Nadia's framing rather than found it.

### 8.

Cross-reference all 16 responders in callout-history.csv against who appears in the tickets. For each responder not represented, say whether their data shows a problem. Vesper and Meteor Mite are the two worst-hit in the company and filed nothing. I want the ratio of damaged-and-complained to damaged-and-silent, and what that implies about how far to trust ticket counts as a measure of how many people are affected.

### 9.

Adversarially test the push-delivery hypothesis against all 25 tickets. I want what does NOT fit. Specifically: Corporal Ashgrove (-27% volume) and Halfmoon (-24%) reported silence but are genuinely down, unlike the +30-49% group — is that a third population I've collapsed? Check whether the drought lengths handlers state ("no callouts since the 12th," "one callout since the start of the month") are consistent with weekly volume, and whether a handler's account and their responder's own account of the same drought agree. Argue the case against me.

### What this round found

- **The pile is curated.** T-001 through T-025 with zero gaps, 22 of 24 consecutive days covered, never more than two a day, 100% callout-related with no noise of any kind, and it starts the day *after* the release. Almost certainly the ticket breakdown Nadia promised Marcus for the new PM's regroup.
- **So my 14:9 split wasn't independent.** It's Nadia's "two thirds to one third" handed back to me. It should not be cited as agreement with her.
- **The pile has no baseline.** No pre-12-August tickets exist, so "3x normal" is unverifiable and every "this is new" claim is unfalsifiable against it.
- **The queue misses the worst cases by design.** Twelve of sixteen responders filed. The two non-filers with real problems are Vesper and Meteor Mite — the two most damaged accounts in the company. Starvation is an *absence*, and an absence generates no event to file a ticket about. Ticket volume over-samples things that happen and under-samples things that don't.
- **The adversarial prompt killed the delivery-bug theory.** Looking at `pings_taken` — the column I'd never opened — Nightwell **accepted 14 callouts** in the week she filed *"nothing in like 10 days."* Stormwrack's take count hit a ten-week high in the week his handler called the quietest in two years. You can't answer a phone fourteen times if it isn't ringing.
- **A third population appeared.** Corporal Ashgrove (−27%) and Halfmoon (−24%) are genuinely down but nowhere near collapse — a middle band I'd flattened into one of the two clean buckets.

---

## Round 4 — reconciling the two piles

_Goal: work out which source to trust when the tickets and the data disagree._

### 10.

Read everything in 00-rook/code/dispatch-routing/ — routing.py, offer.py, history.py, availability.py, config.py, plus the README and CHANGELOG. I need to know exactly what gets counted as a "ping sent" and a "ping taken" in callout-history.csv. Specifically: when an offer times out and moves down the ranking, does it count as sent to the first responder, the second, or both? Does "taken" mean that responder accepted, or that the callout got filled by someone in the chain they were part of? And what does the CHANGELOG say actually shipped on 12 August?

### 11.

Don't pick a winner between the tickets and the data. Enumerate every mechanism that would let both be true simultaneously — that a responder shows record acceptance in callout-history.csv while genuinely receiving nothing on their phone. Include: can a handler accept a callout from the console on the responder's behalf? Are there two acceptance paths? Is the data aggregated at a level above the individual? Then check each one against the code and tell me which survive.

### 12.

Read 00-rook/company/notes/handoff-from-priya.docx and 00-rook/company/release-history.pdf. I need what the ticket pile is missing: what normal callout-ticket volume looked like before 12 August, whether "phone never goes off" and "gone before I could answer" complaints existed pre-4.2, and precisely what changes shipped on the 12th. Also check the glossary for how "callout," "ping" and "offer" are formally defined — I may have been using them interchangeably when the product doesn't.

### What this round found

**The answer was in the code, not in either pile.**

- **The two piles cannot both be true.** There is no handler-accept path anywhere in `offer.py` — it pushes to a device and polls that device. The reconciliation I'd hoped for doesn't exist.
- **4.2 shipped three things:** ranking weights (proximity 0.45 → 0.60, recent acceptance 0.40 → 0.25), the callout timeout 90s → 60s, and console filter persistence. **There is no push fix in 4.2** — "mobile push reliability" shipped in 4.1 on 16 June, which Priya's handoff confirms.
- **The recent-acceptance score has no decay.** Wen's TODO in `history.py` asking whether it should ease back toward neutral is dated **2019** and still open.
- **The penalty is 1.5× the credit.** `DECLINE_PENALTY = 0.12` against `ACCEPTANCE_CREDIT = 0.08`. A responder must accept 60% of offers just to hold their score level.
- **A timeout is scored identically to a refusal.** That answers Marcus's unanswered question from 14 August — and the answer is worse than he suspected: people who never saw the offer are treated as decliners.
- **`SCORE_FLOOR = 0.0` is an absorbing state.** At the floor you rank last on every callout, so you're never offered anything, so you can never earn the credit that would lift you. `dispatch()` walks strictly top-down, so a small ranking gap becomes a large volume gap.
- **The trigger is visible in the data.** In release week the whole cohort fell to **54.2%** acceptance — below the 60% break-even. Everyone's score dropped at once. Most climbed back above the line and recovered. Four did not: Vesper, Meteor Mite, Farlight and The Undertow. They are at the floor now and cannot recover on their own.
- **The glossary is wrong, and that's why nobody caught it.** Product's own document, updated 4 August, says the recent-acceptance component drops *"until the component recovers"* and that a decline is *"distinct from a timeout in the data."* Neither is true of the code. Everyone at Rook — Priya included — has been reasoning from a description of a system that self-heals, while the real one ratchets.

---

## Bottom line

**What survived everything:** four responders are genuinely starved, and the code explains why in a way neither pile could.

**The claim worth making:**

> The recent-acceptance score has no decay — Wen's TODO in `history.py` saying so is dated 2019 — and a timeout costs 0.12 while an accept earns 0.08. Once a responder hits the floor they rank last on every callout and can't earn their way back. Vesper, Meteor Mite, Farlight and The Undertow are there now. The glossary tells everyone the score "recovers"; it doesn't, and that's why we've all been waiting for September.

It doesn't depend on the tickets, the interviews, or the disputed volume numbers. Four constants and a seven-year-old comment, checkable in two minutes.

**Still open:** whether `callout-history.csv` is Ravi's real weekly reporting or the rough pull Marcus offered on 19 August. The code corroborates its four collapsing responders; nothing corroborates its record-high numbers for Nightwell and Stormwrack.

**Method note:** three of the four rounds found errors in my own reading rather than new facts. The round that found the answer was the one that stopped asking people what happened and read the thing that decides it.
