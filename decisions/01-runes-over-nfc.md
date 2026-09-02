# Replacing an NFC reader with the camera already in the laptop

**Decision: remove the USB NFC reader from the box. Objects are identified by
printed markers held up to the built-in camera.**

## The problem with the reader

The original build shipped a physical NFC reader. Every object carried a sticker,
the parent plugged a reader into the family laptop, and children tapped objects
against it.

It worked. It also meant sourcing hardware, paying for it in every unit,
supporting drivers, and owning a whole class of "it doesn't see my reader"
support tickets before a single child had played anything.

## What replaced it

Printed [ArUco](https://docs.opencv.org/4.x/d5/dae/tutorial_aruco_detection.html)
markers, from the 4x4 dictionary, held up to the laptop's normal forward-facing
camera. Each marker maps to an object. The camera is already there, in the
position it is already in, with the lid at its normal angle.

## What I measured before committing

**Reliability.** Zero wrong identifications across 52 real confirmations and 960
synthetic trials. That was the bar that mattered: a marginal read that names the
*wrong* object sends a child to search for something they already found, and no
amount of charm recovers that.

**Detection is binary, which turned out to be a design asset.** A marker either
resolves within about three frames or not at all. There is no "sort of read it"
state. That means the failure mode is coachable in character: *"turn it towards
my eye, I can't quite see it!"* A probabilistic reader with a confidence score
cannot be narrated that way.

**Distance.** 26 raises across a large room, all correct, smallest readable
marker 9 pixels. At a natural two-foot hold, a 1 inch marker presents about 46
pixels, so:

| marker | px at 2 ft | margin over the observed floor |
|---|---|---|
| 1.000 in | 46 | 4.6x |
| 0.750 in | 35 | 3.5x |
| 0.500 in | 23 | 2.3x |

Half an inch would make the printed tile **smaller than the NFC sticker it
replaces**, which answers the footprint objection outright.

## Why I did not then commit to half an inch

Three reasons, and the first is the one I care about most.

**9 px is roughly OpenCV's own configured floor, not a reliability limit.** The
default `minMarkerPerimeterRate` on a 1280 px frame rejects anything under about
a 9.6 px side. So the sweep found where the detector stops looking. It did not
find where it stops being correct, and those are different numbers.

**The test could not produce the failure I cared about.** Only one marker existed
in that session, so 26 out of 26 shows the detector never hallucinated a
different id when there was no other id to hallucinate. At 9 to 15 px each of the
marker's six modules is one and a half to two and a half pixels, which is exactly
where bit decoding becomes guesswork, and the error-correction setting will
cheerfully repair its way to a **confident wrong answer**.

**And a synthetic model had already been wrong in the reassuring direction.** It
predicted reliability only above 24 px; the real camera managed 9. Better that
way round, but it means the synthetic envelope is a floor on optimism rather than
a prediction, and I stopped quoting it as one.

The follow-up costs nothing: **distance simulates size.** Standing where a 1 inch
marker reads 23 px puts all eight ids in play at the pixel size in question,
which is the only arrangement in which a wrong identification can appear at all.
No printing, no procurement, one session.

## What it bought

| | |
|---|---|
| Hardware in the box | one component removed |
| Codebase | about 1,000 lines that no longer need to exist: reader polling, id-to-object assignment, and a whole batch-provisioning workflow for stickers |
| Manufacturing | markers become a print job |
| Support | an entire failure category disappears |

## What it cost

All risk now sits in one camera pipeline with no fallback. The live unknowns are
industrial design rather than software: markers on curved objects, in dim rooms,
in motion, with a thumb across part of the marker, held by a child rather than an
adult.

That concentration is the real price, and it was worth paying because the
alternative was paying for a reader in every unit forever to hedge a risk I could
measure instead.

## A process note that cost a day

The first print run came back a page short and nobody noticed, so a size
comparison ran entirely on the one size that arrived. **Prefer a single page per
print job, or count the sheets at the counter.**
