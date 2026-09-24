<!-- Test fixture for review-checklist. Expected result is recorded in
     ../run-3-test-brief.txt, written before the run. Not a Rook document. -->

# Availability Confidence: one-pager

Product, Dispatch

**Owner:** Sofia Marino. Marcus's team builds.

## Problem

A handler sets a responder's availability, and Dispatch believes it. When the
responder is not actually free, the offer goes out, times out, and the callout
waits. Handlers override stated availability on 38% of callouts, which means
the order Dispatch builds is often wrong before it starts.

## Proposal

Show a confidence score next to each responder's stated availability on the
console, based on how often that availability has held up recently. Handlers
see at a glance who is really free.

Showing the score will cut wasted offers by a third.

## Scope

The score on the console, and the calculation behind it. No change to how
offers are ranked, no change to the phone app, and no automatic changes to
anyone's availability.

## Success measure

We will know it worked when support escalations about availability drop.
