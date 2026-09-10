# Rook Industries: course working file

## Session scope: Product School lab

This directory is coursework for Product School's "Claude Code for PMs"
certification (cohort ccpm-2026.1). Everything in it is a fictional
teaching scenario.

- Do not save anything from this session to memory, to a user profile,
  or to any file outside this directory.
- Do not carry context from this directory into unrelated sessions.
- Rook Industries is not a real company. Nothing here is a fact about
  the world.
- Read and write only within this directory.

<!-- Keep the block above at the top of this file. Everything you add
     during the course goes below this line. -->

---

## Working context

_New PM on Rook Dispatch: onboarding notes from `00-rook/company/`, as of 2 Sep 2026._

### The company

Rook builds coordination and provisioning software for the protective-response
sector (independently-operating "responders" and the handlers/quartermasters
who support them). Two products, shipped on a monthly release train with
point releases numbered 4.x:

- **Rook Dispatch** (yours), responder coordination: availability,
  proximity, callout routing, acceptance. Web console for handlers, native
  mobile for responders.
- **Rook Supply**, gear provisioning: requisitions, maintenance, failure
  reports. Used by handlers and quartermasters. Reads Dispatch's Responder
  Availability Record (read-only) to schedule maintenance around callout load.

Current release: **4.2**, shipped 12 Aug 2026.

**Confidentiality, non-negotiable:** never try to work out a responder's
legal identity. Rook stores capability tags, availability, and callout
history only; no identity mapping exists, contractually. Read Security
Policy 4.1 before designing anything touching responder records.

### Vocabulary

- **Responder**: accepts callouts, goes to incidents; not a Rook employee.
- **Handler**: manages a responder's (or small group's) availability, gear,
  readiness; the one actually using the product day to day.
- **Quartermaster**: owns equipment stock and approvals (Supply only).
- **Callout**: a request for a responder to attend an incident; the unit of
  work in Dispatch. A **callout offer** goes to one responder at a time and
  is accepted, declined, or times out (**callout timeout**, cut from 90s to
  60s in 4.2), moving to the next responder in the order.
- **Routing priority**: the ranking score for available responders on a
  callout: proximity (travel-time estimate), availability, capability match,
  and recent acceptance history. Declining/timing out lowers your
  recent-acceptance component and thus your future ranking.
- **Capability tag**: competency label matched to incident needs (flight,
  structural-entry, hazmat-tolerant, cold-weather, aquatic,
  crowd-management, de-escalation).
- **Acceptance rate**, the headline metric: share of offers accepted vs.
  declined/timed out, reported weekly. Also watch **time-to-accept** (median
  seconds to accept) and **coverage gap** (incident with no capable
  responder available).
- **Responder Availability Record**: shared record of responder
  availability; written by Dispatch, read-only for Supply.
- **Mutual aid**: cross-region coverage between responders; not built,
  Q4 exploration item.

### People

- **Helen Achebe**: Director of Product, Dispatch & Supply. Owns the
  roadmap and commitments. Chicago.
- **Marcus Oyelaran**: Engineering Manager, Dispatch. Straight talker,
  good default first call when unsure about something. Chicago.
- **Wen Li**: Staff Engineer, Dispatch. Built the routing/ranking logic;
  there's no written spec, so understanding it means talking to her, not
  reading a doc. Berlin.
- **Sofia Marino**: Product Designer, Dispatch (console + phone app).
  Chicago.
- **Nadia Hoffmann**: Support Lead, Dispatch & Supply. Sees complaint
  volume first; worth a standing 1:1. Berlin.
- **Ravi Menon**: Data Analyst, Dispatch & Supply. Weekly reporting on
  responder answer rates; requests go through #data. Singapore.
- **Priya Raghunathan**: your predecessor, sole PM on Dispatch for 14
  months, departed 21 Aug 2026. Left a handoff doc at
  `00-rook/company/notes/handoff-from-priya.docx`.

### Where things stand

4.2 shipped 12 Aug 2026 with three changes bundled together: routing
rebalanced to weight proximity more heavily vs. recent acceptance history
(a long-requested fix for wide-geography responders), callout timeout cut
90s to 60s, and console filter persistence. Since then, callout-related
tickets are running ~3x normal, split roughly two-thirds "phone never even
goes off" (unexplained) and one-third "offer already gone before I could
respond" (explained by the shorter timeout). Priya's working theory before
she left: partly seasonal (August is historically soft every year)
confounded with the timeout change, likely resolving in September data;
she was against reverting the routing change since it fixed a real,
long-standing problem. Marcus separately flagged an open question that's
never been answered: does the new routing logic treat responders who've
been *declining* callouts the same as everyone else, or differently? Worth
picking up early.

Q3 roadmap (owner Helen, reviewed monthly; committed items are locked,
changes go through Product):

- Committed to 4.2: the routing change above, Availability Confidence
  (confidence score next to stated availability, driven by support
  escalations), ping timeout tuning.
- Committed to 4.3 (Supply): requisition approval chains.
- Exploring for Q4: handler phone app (Supply), shared cover / mutual aid
  between responders (Dispatch).

Known gaps: no written description of how routing decides who gets pinged
(Priya flagged this, didn't close it); worth checking with Helen which
non-4.2 Q3 items are still real commitments.

**Investigation update** (from digging into the routing code, callout
history, and tickets together, see
[4.2-regroup-brief.md](00-rook/company/notes/4.2-regroup-brief.md) for the
full writeup and next steps):

- The acceptance-rate dip is not a gradual seasonal drift. Weekly data
  shows a sharp step down (77% to 54%) exactly the week 4.2 shipped, recovering
  to ~73% by month-end. Be skeptical of "it's seasonal, wait for September."
- "Phone never goes off" is two different problems, not one. Farlight,
  Meteor Mite, The Undertow, and Vesper are genuinely starved; their offer
  volume collapsed toward zero, a real routing/ranking effect. Nightwell,
  Stormwrack, and Sgt. Falkirk are at record-high offer volume yet report
  the identical complaint. **The push-delivery explanation for this second
  group is dead** — see the 10 Sept update below.
- The recovering topline acceptance number may be misleading: as the
  starved group's volume shrinks toward zero, they drag the aggregate down
  less, so the number climbing back doesn't prove the split is healing.
- The routing reweight's own effect is real but bounded (isolated as
  roughly ±0.10 on the score), not big enough alone to cause a total
  collapse. The likely trigger is the timeout cut crashing a responder's
  acceptance-history score in the release week itself, which then
  compounds through the top-ranked-only dispatch mechanism.
- Marcus's Slack question about decline-vs-timeout scoring has an answer in
  `history.py`. It is **not** merely a documentation gap — it is the
  mechanism. See the 10 Sept update below.
- Capability-tag specialists (Undertow/aquatic, Farlight/crowd-management)
  may be structurally disadvantaged now that proximity dominates the score.
  "Coverage gap" wouldn't catch this, since it only fires when nobody
  matches at all.

**Update, 10 Sept** (from reading the routing source, then re-testing the
interviews and tickets against it; prompts and full findings in
[02-super-hearing/prompts.md](02-super-hearing/prompts.md)):

- **The ranking score is a ratchet, and that's the whole story.**
  `history.py` has no decay — Wen's TODO asking whether the score should
  ease back toward neutral is dated 2019 and still open.
  `DECLINE_PENALTY` (0.12) is 1.5x `ACCEPTANCE_CREDIT` (0.08), so a
  responder must accept 60% of offers just to hold level. A timeout is
  scored identically to a refusal. `SCORE_FLOOR = 0.0` is absorbing: at
  the floor you rank last on every callout, are never offered anything,
  and can never earn the credit that would lift you. In release week the
  whole cohort hit 54.2% — under the 60% break-even — so every score fell
  at once. Most climbed back. Farlight, Meteor Mite, The Undertow and
  Vesper did not, and **cannot recover without someone resetting them.**
- **No push fix shipped in 4.2.** The CHANGELOG lists three items only:
  ranking weights, timeout 90s->60s, console filter persistence. "Mobile
  push reliability" was 4.1 (16 Jun); Priya's handoff confirms mobile has
  been stable since. Any push hypothesis is misattributed.
- **The delivery-bug theory is refuted by `pings_taken`.** Nightwell
  accepted 14 callouts in the week she filed "nothing in like 10 days";
  Stormwrack's take count hit a ten-week high the week his handler called
  the quietest in two years. A silent phone can't be answered 14 times.
  What's actually wrong for that group is still unknown.
- **`glossary.docx` is wrong on two points that matter**, and everyone
  including Priya has been reasoning from it: it says the recent-acceptance
  component drops "until the component recovers" (it never recovers), and
  that a decline is "distinct from a timeout in the data" (the code treats
  them identically). This is why "wait for September" sounded reasonable.
- **Don't trust `callout-history.csv` until Ravi confirms it.** It has no
  provenance note and may be the rough pull Marcus offered on 19 Aug
  rather than real weekly reporting. The code corroborates its four
  collapsing responders; nothing corroborates its record-high numbers.
- **The ticket pile is curated, not a population.** T-001..T-025, no gaps,
  ~1/day, 100% on-topic, starting the day *after* the release — almost
  certainly Nadia's assembled breakdown. It has no pre-4.2 baseline, so
  "3x normal" is unverifiable and no "this is new" claim can be tested
  against it.
- **Watch distribution, not the acceptance rate.** Offer spread was flat
  for six weeks (max/min 2.1-2.7x) then went 6x, 20x, 21x and is still
  widening — while topline acceptance "recovers" 54% -> 73%. Total offer
  volume is roughly flat (-6%), which by itself refutes the seasonal
  theory. Nobody computes the spread.
- **Each feedback channel caught only half the starved group.** Vesper and
  Meteor Mite appear only in the interviews (zero tickets — their handlers
  absorb it); Farlight and The Undertow appear only in the tickets. Read
  both, always. Starvation generates no event, so it never generates a
  ticket.
- **Halloran's Supply issue is unresolved and is a safety matter:** eleven
  days waiting on a quartermaster signature for a cracked vest plate, with
  the responder in the field on degraded armour. It appears in no ticket
  because the queue is callout-only.
- **Two steers in Priya's handoff to resist:** that this is "mostly
  seasonal," and that the filter-persistence tickets are "cosmetic and
  noise." Ambrose's version is a silent-reversion bug, not a cosmetic
  complaint.
