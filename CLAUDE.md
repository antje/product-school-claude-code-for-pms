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
arrival, and parts of it have since been shown to be wrong. Current findings,
with the evidence behind each, are in
[4.2-investigation.md](4.2-investigation.md). Read that before reasoning about
4.2. In particular the onboarding claim that two-thirds of tickets are
"unexplained" no longer holds.

### Where my work lives

- `4.2-investigation.md`, findings log, updated each module.
- `4.2-regroup-brief.md`, the brief for the regroup with Marcus and Nadia.
- `0N-*/prompts.md`, the prompts I wrote, one file per module.
- `00-rook/` is Rook's own material and is read-only. Never write there.
