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

### What is known so far

The "Where things stand" section above is the picture as handed to me on
arrival, and parts of it have since been shown to be wrong. The notes below
record what each session added, in the order I learned it. The evidence
behind each finding is in [4.2-investigation.md](4.2-investigation.md) and the
module `prompts.md` files. Where a later session corrected an earlier one, the
earlier note says so.

### After Module 1, Onboard (8 Sep)

- The acceptance drop is a step in the week 4.2 shipped, 78% to 54%, not a
  gradual August slide. Priya's "seasonal, wait for September" does not fit.
- Two groups report "phone never goes off". Four responders (Farlight, Meteor
  Mite, The Undertow, Vesper) really did lose their offers. A busier group is
  at record volume and says the same thing.
- My working theory for the busy group was a push-delivery bug.
  *(Refuted in Module 2: no such fix shipped in 4.2.)*
- Marcus's question from 14 Aug is still unanswered. `history.py` scores a
  timeout exactly like a decline.
- The numbers I checked held up. One cited source did not. Open the file
  before quoting a changelog entry, a line number or a defect.

### After Module 2, Listen (10 Sep)

- The four interviews are all handlers, and none of them holds the phone the
  offers arrive on. Only Dot saw an offer vanish herself.
- All 25 tickets date from 13 Aug to 5 Sep, numbered with no gaps. There is no
  pre-4.2 baseline in the pile, so "3x normal" cannot be tested from it.
- Each pile caught half of the four. Vesper and Meteor Mite appear only in the
  interviews, Farlight and The Undertow only in the tickets. Losing offers
  produces no event, so it only reaches a ticket if a handler notices.
- The score is a ratchet: a miss costs 0.12, an accept earns 0.08, so a
  responder must accept 60% to hold level. There is no decay and the floor is
  0. The whole fleet accepted 54% in release week.
- The 4.2 changelog lists three changes: ranking weights, timeout 90s to 60s,
  filter persistence. Nightwell accepted 14 callouts in the week she reported
  hearing nothing, so the busy group's problem is not delivery, and is still
  unexplained.
- `glossary.docx` is wrong twice: the score does not recover, and a decline is
  not distinct from a timeout in the data.
- Halloran has waited 11 days on a cracked vest plate. It is a Supply safety
  issue and appears in no ticket.

### After Module 3, Verify (15 Sep)

- `callout-history.csv` has no provenance and nothing outside it confirms it.
  Ravi should confirm what it is before any number from it goes further.
- The same four names come out under 27 ways of measuring. My number for
  Helen: four of sixteen responders get 62 to 100% fewer offers than before
  12 Aug, while total offers are down 4%.
- Part of the acceptance "recovery" is the four dropping out of the count:
  they went from 28% of offers to under 2%.
- Before 4.2 every responder accepted at least 60% nearly every week, so every
  score sat at the ceiling. 4.2 broke a tie that had never mattered.
- The file and the tickets agree on the four and disagree on everyone else.
  Vesper and Meteor Mite were hurt and never filed.
- Weekly accepted callouts fell from 132 to 96, 104, 108 and 120 while offers
  held. If that means unfilled callouts, it is about 100 in four weeks. Low
  confidence until Ravi counts callouts created.
- The CSV has no timeout column. The 90 to 60 change lives in `CHANGELOG.md`
  and `config.py`.

### After Module 4, Inspect (17 Sep)

- Only three numbers changed in 4.2, all in `config.py`. No logic changed.
- The reweight made each miss cost less rank, not more. The shorter timer is
  what multiplied the misses.
- Answer to Marcus: the change applied to everyone the same way. The four were
  not turning work down beforehand; Vesper had the second-best record.
- Ranking all sixteen by what they lost in release week puts exactly the four
  at the bottom. I wrote that prediction down before checking it.
- `_scores` is never saved. If production does the same, the 12 Aug deploy
  reset everyone to 0.5, and from there the ratchet alone picks out the four
  in 95% of simulated orderings. This is a yes or no question for Wen.
- Only accepting an offer adds points. Nothing in the routing code logs
  anything, and `record_declined` throws away whether it was a timeout.
- Kip is the only handler with two responders: Meteor Mite down 79%, The Gale
  up 49%, same city.

### After Module 5, Prototype (22 Sep)

- My person is The Undertow, handled by Desmond Okafor. His own ticket, T-013,
  is the problem statement: "starting to wonder if im still even in the
  system".
- The timer decides how long he has to answer. The score decides whether he is
  asked at all. Reverting the timer alone gives him no more offers.
- Build order: record every offer first, because it depends on nothing and
  shows whether scores survive a deploy. Then alert the handler, tell the
  responder where he stands, and let someone lift him to the top of the range.
- A lift has a cost, and it has a name: when Meteor Mite goes back up, The
  Gale is asked less.
- Still open: who gets the alert when a handler holds several responders, who
  may lift someone, and what a second lift in a month means.

### After Module 6, Automate (24 Sep)

- `review-checklist` lives in `.claude/skills/review-checklist/`. Six
  criteria: the course's four, plus two of mine, every evidence number cites a
  source and anything modelled or unknown is written as a question.
- My brief failed it, was fixed, then failed again after later edits. A brief
  that passed once is not a brief that passes.
- So far the skill has only been applied by hand from another session. A true
  run means asking for it in a session opened in this folder, where it loads.
- `06-sidekicks/scheduled-run-output.txt` came with the template. It is the
  course's example, not a run of mine.
- The brief now turns the timer back to 90 seconds in the same release as the
  rest, said openly, not on its own.
- The 8 Sep regroup brief led with the push theory. It has been rewritten.

### Where my work lives

- `final-report.html`, the presentation, and the place to start.
- `4.2-investigation.md`, the findings log.
- `4.2-regroup-brief.md`, the brief for the regroup with Helen, Marcus, Wen
  and Nadia.
- `05-super-speed/brief.md` and `prototype.html`, the brief for Helen and the
  clickable mock.
- `04-x-ray-vision/visuals/`, one chart per round.
- `.claude/skills/review-checklist/`, the skill, with its runs in
  `06-sidekicks/`.
- `0N-*/prompts.md`, the prompts I wrote, one file per module.
- `00-rook/` is Rook's own material and is read-only. Never write there.
