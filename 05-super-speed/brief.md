# What we'd build instead of turning the timer back up

For Helen. Rough on purpose.

**Owner:** me, as PM on Dispatch. Marcus's team would build the offer record
and the alert; Sofia would design the two screens. Every figure below comes
from `callout-history.csv`, the routing code and the tickets, and the working
is in `4.2-investigation.md`, `03-rewind/` and `04-x-ray-vision/`.

---

The quick fix is real. Putting the callout timeout back to 90 seconds is one
number in `config.py`, and The Undertow would probably catch the one offer he
gets in a week instead of watching it go.

It still leaves him at one offer a week. The timer decides how long he has to
answer; the recent-acceptance score decides whether he is asked at all, and
his is on the floor. Engineering's afternoon fix addresses the thing he
complained about second.

**Wen's 2019 note asked whether the score should ease back toward neutral on
its own. It should.** Nobody should carry a bad month into the spring, and
today they carry it forever: accepting an offer is the only thing that adds
points, and only people with points get offers. But decay would be slow at any
sensible rate. At 0.02 a day, the rate I modelled, a single miss still takes
six days to wear off, so it rescues the next person, not him. Decay is not in
this build: it is the next piece of work, once we know where the score is
stored.

One thing shapes the order of everything below. The score lives in memory and
is never written to disk. If production does the same, every deploy resets all
sixteen responders to the middle, and an alert, a lift and a decay rule are
all built on sand. I don't know yet, so the piece that depends on nothing goes
first.

## 1. Who it is for

**The Undertow**, aquatic-tagged. Six steady weeks of eleven to thirteen
callouts, nine or ten taken. Then the release: eleven offers, four caught.
Then four. Then one, then one, none taken.

What he can see when a callout goes elsewhere: nothing. No missed-call notice,
no "you were third in line," no record that a job he was qualified for existed.
On 26 August he filed a ticket from his phone, which responders almost never do:

> "nothing again this week. starting to wonder if im still even in the system"

He is still in the system. Nobody could tell him, because nothing records that
he was considered and passed over.

**Desmond Okafor**, his handler, sees a console card that doesn't change. No
queue, no rank, no reason. He filed T-005 on 19 August marked Low, calling the
quiet "a little puzzling." Twelve days later he filed T-019 marked High, after
the one callout that arrived vanished before The Undertow could read who it was
for. Nothing happened in between, because no signal existed to act on.

## 2. What changes

**First, the system records what it did.** One line per offer: who it went to,
which callout, their position in the list, whether the phone acknowledged it,
what came back, how long it took. The code knows all of that when it takes a
point off someone and keeps none of it. This depends on nothing else being
true, so it ships first. It makes T-013 answerable the day it is filed, and it
is how we learn from data whether scores survive a deploy.

**Then Okafor gets told instead of noticing.** When a responder's score crosses
a low threshold, his handler gets an alert: who, when, and how many callouts
since. All four affected responders crossed 0.2 during release week, so Okafor
would have had this around 16 August. He worked it out himself by the 19th,
filed it Low because nothing told him it was serious, and waited.

**The Undertow gets his question answered.** On his phone: you are still
active, here is where you stand, here is what came up near you this week and
where it went. Not a score. The line that matters says he hasn't been removed.

**Somebody can act, and the arithmetic says what acting means.** From the
floor, accepting every offer he gets, he needs thirteen in a row to reach where
the other fifteen sit. Seven reaches the midpoint, still below all of them. He
gets about one offer a week. He cannot climb out; somebody has to lift him.

## What a lift costs, and who pays

Moving someone from the floor to the top of the range is worth about nineteen
minutes of travel time in the ranking. Where two responders are minutes apart,
that reorders them.

Kip handles Meteor Mite and The Gale in the same city. Since the release Meteor
Mite has gone from eleven callouts a week to one; The Gale from thirteen to
twenty-one. Lift Meteor Mite and The Gale is asked less. That is the intended
effect and somebody should say so before we build it.

The Gale isn't obviously fine either: twenty-one offered last week, sixteen
taken, the most he has ever turned down. Kip called the two of them "two
different products" on one screen. He already knew. An alert repeating what he
told us weeks ago is not the fix.

Three decisions are open and not mine alone:

- **Who receives the alert** when a handler holds several responders, and what
  the console shows when one card starves while another floods.
- **Who may lift someone**, Okafor or only Marcus. A trust question, Helen's
  to answer.
- **What happens on a second lift in a month.** If that recurs, the ranking is
  wrong and we are papering over it.

Two I'll decide unless someone objects. A lift goes to the top of the range,
not the middle, because the middle leaves him below all fifteen others for
seven more perfect weeks. And every lift is recorded with who did it and why,
which we would have to build, since nothing here records anything today.

## 3. What it deliberately does not do

- **Doesn't change the ranking weights.** Proximity against acceptance history
  is a separate argument.
- **Doesn't silently reset anyone.** The failure we are fixing is a system that
  changed someone's standing without telling anybody.
- **Doesn't promise The Undertow work.** It tells him where he stands, including
  when the answer is that he is behind.
- **Isn't a setting.** Nothing here ships by flipping a flag.
- **Doesn't touch Supply.** Halloran's eleven-day wait on a cracked vest plate
  is real and is not this.

## How we will know it worked

No responder sits below the threshold for more than a week without their
handler having been told and a decision recorded. For The Undertow that took
nineteen days, from the release on 12 August to T-019 on the 31st, and no
decision was ever made. The second signal is that tickets like T-013, a
responder asking whether he is still in the system, stop arriving.

## Something to click

`prototype.html` walks The Undertow's month in five screens: Okafor's console
the morning the alert fires, with a switch between today and proposed; his
phone answering what he asked on 26 August; the offer that vanished on the
31st; an honest week with no news; and the whole month side by side. A mock,
not a build.

## What I still need

- **Wen, and this gates the rest:** where is the score stored, and what happened
  to it when 4.2 deployed on 12 August? If production keeps it in memory like
  this code does, the alert fires for everyone after every release, a lift lasts
  until the next deploy, and decay means nothing.
- **Wen:** is 0.2 the right alert threshold, or just where these four landed?
- **Marcus:** is anything like that per-offer line already recorded somewhere,
  in the 4.0 routing override audit log or elsewhere?
- **Ravi:** how many callouts were created each week since June. Accepted counts
  fell by about a hundred over the four weeks after release while offers went
  out at the usual rate. Either a hundred incidents found nobody, or the file
  means something other than what I think. That is bigger than this brief.
