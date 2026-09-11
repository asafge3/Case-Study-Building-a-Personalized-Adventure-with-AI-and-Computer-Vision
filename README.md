# Mystic Mission: building a physical adventure game for 4 to 9 year olds

A zero-to-one consumer product, taken from an idea to a playtested build and a
manufacturing decision by one person over about six weeks.

Animated characters send children on a scavenger hunt around **their own house**.
The objects they find are collectible metal coins, identified by the family
laptop's camera. Children cast spells by drawing shapes in the air with a
light-tipped wand. It ends in a real physical treasure.

**This repository is the decision record, not the source.** The product code is
private. What is here is the part that is worth reading: what I tested, what the
numbers said, and what I killed as a result.

---

## Why this exists

I wanted a portfolio piece that showed judgement rather than output. Some of what
follows is me being wrong in public and then measuring my way out of it, which is
the honest shape of zero-to-one work and is usually the part that gets edited out
of a case study.

Four things I would want the reader to take from it:

1. **I run experiments that can kill my own ideas**, and several did.
2. **I read my own results conservatively**, including one result that looked like
   a triumph and was actually the detector hitting its own configured floor.
3. **I withdraw claims when the evidence does not support them**, including two of
   my own that had already made it into design documents.
4. **I make platform and cost decisions from market evidence**, not from taste.

---

## The product in one diagram

```
  Parent hides 5 coins + the treasure, before play
            │
            ▼
  ┌─────────────────────────────────────────────────┐
  │  Scene video   →   personalised briefing       │
  │  (AI Generated)    (names, pet, real rooms) │
  └─────────────────────────────────────────────────┘
            │
            ▼
     Child searches the house, finds a coin
            │
            ▼
     Holds it to the laptop camera  →  story advances
            │
            ▼
     Draws a shape with the wand, shouts the spell word
            │
            ▼
     Repeat, six acts, ~45 minutes, ending in a real object
```

Two content channels matter more than they look. **Recorded video is identical
for every family, so it can never contain a name or a room.** Everything
family-specific lives in generated comic panels. Getting that seam wrong is what
made an earlier version of the adventure unshippable to anyone but my own
household.

---

## The four decisions worth reading

| | |
|---|---|
| **[Runes over NFC](decisions/01-runes-over-nfc.md)** | Replaced a USB NFC reader with printed markers read by the built-in camera. Removed a hardware component from the box and about a thousand lines from the codebase. |
| **[The wand, and the result that changed the design](decisions/02-the-wand.md)** | Gesture recognition scored 100% on me and 63% on a four-year-old using my templates. Per-child enrolment stopped being an elegant option and became a requirement. |
| **[What I killed](decisions/03-what-i-killed.md)** | A camera "magic zone", the NFC layer, the native Mac app, a naming mechanic the playtest seemed to justify, and a feature I nearly built twice. |
| **[Designing for a four-year-old and a nine-year-old at once](decisions/04-designing-for-both-ages.md)** | The same 45 minutes has to work for both. Spells that never fail, a riddle that never blocks, and a story with two readable levels. |
| **[The platform decision](decisions/05-platform-and-market.md)** | Web over native, decided from tonies' and Yoto's revenue numbers and Osmo's collapse rather than from taste. |

---

## Headline evidence

**Vision.** Printed ArUco markers, held to a MacBook's forward-facing camera.
Zero wrong identifications across **52 real confirmations and 960 synthetic
trials**. A distance sweep across a large room: 26 raises, all correct, smallest
readable marker **9 pixels**.

**Why I did not celebrate that.** 9 px is roughly OpenCV's own configured
rejection floor, so the sweep found where the detector stops *looking*, not where
it stops being *right*. And with only one marker in the room, a degraded read had
no neighbouring id to land on, so the test structurally could not produce the
failure I actually cared about. The follow-up test puts all eight ids in play at
that pixel size. **Simulating a smaller marker by standing further away costs
nothing and needs no printing**, which is the cheapest test in the project.

**Gestures.** Three-shape discrimination at 100% on an adult, with classes
sitting three to four times further apart than their own members, which is what
makes generous thresholds safe. Then a four-year-old scored 77% on his own
templates and **63% on mine**. Half the remaining gap was tracking rather than
drawing: he lost a median **19% of frames** against my 0%.

**A negative result I kept.** Sliding a window over a recording to find the
gesture inside it lifted a child's accepted casts from 60% to 83%. It also took
random waving accepted from 3% to **71%**. I nearly shipped it on the recall
number alone. Conservative trimming, which beats the baseline on both axes, is
what went in.

**Market.** tonies SE FY2025: €630M revenue, +31%, of which figurines are €447M
(71% of revenue, growing 37%) against €161M of hardware. The device is the razor;
the content is the business. Yoto reached roughly $127M with about 60% of
customers arriving by word of mouth, and got into Target *after* the growth
rather than as the price of entry. Osmo, the closest camera-and-physical-object
analogue, sold for $120M in 2019 and its IP was bought out of bankruptcy for
$825,000 in 2025. **Its stated structural problem was distribution, not
technology.**

That reframed the platform question as small. In tonies' numbers the platform is
the lower-margin, slower-growing 26%. The library of adventures is the 71%.

---

## Two claims I withdrew

**"Children need about fifteen attempts to learn a gesture."** Based on my own
circles apparently improving over a run. They were not improving; I had been
deliberately varying them to test the recogniser. Withdrawn.

**"The click of the lock is what children remember."** Based on a playtest where
a nine-year-old named finding a locked box as his favourite moment and a
five-year-old named opening it. **Neither child mentioned the lock.** I had turned
two real observations into a third claim that nobody made, and it had already
propagated into three design documents before I caught it.

Both are here because a decision record that contains no retractions is not a
record, it is a pitch.

---

## What I would do differently

**I over-tested the mechanic and under-tested the interaction.** I have precise
numbers on how well a camera reads a marker at nine pixels, and I still do not
have a clean measurement of how long a child takes to present one, because in 13
of 14 trials the marker was already in frame when the timer started. The number I
optimised was the software's confirmation floor. The number that matters is a
child crossing a room.

**Three data runs were wasted on procedure, not technology.** A default dropdown
value silently labelled 31 assorted shapes as circles. A focused `<select>` ate
the spacebar, so one gesture type failed for reasons that looked technical and
were not. The fix each time was making the harness drive the protocol instead of
trusting the tester to remember it.

---

*Built in Python and browser JavaScript. Product code, story content and
character IP are private.*
