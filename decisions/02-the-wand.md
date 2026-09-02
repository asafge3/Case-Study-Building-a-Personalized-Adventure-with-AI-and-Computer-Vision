# The wand: a 100% result that meant much less than it looked like

**Decision: gesture templates belong to the individual child, not to the factory.
This was forced by one afternoon of testing with a four-year-old.**

## The mechanic

A wand with a coloured light in the tip. The child draws a shape in the air and
shouts a spell word. The laptop camera tracks the tip. No machine learning: the
path is resampled to 64 points by arc length, centred, uniformly scaled, and
matched against stored templates by mean point-to-point distance.

Removing speed, position and size before matching is what makes it work at all,
and it is also what makes it explainable, which matters when the failure has to
be narrated to a child in character.

## The adult result

30 gestures, 10 each of circle, triangle and Z.

**Three-way discrimination: 100%. A perfectly diagonal confusion matrix.**

The accuracy is not the interesting number. This is:

| gesture | distance to own class | to nearest other class | gap |
|---|---|---|---|
| circle | 0.029 | 0.133 | 0.105 |
| triangle | 0.036 | 0.139 | 0.102 |
| Z | 0.050 | 0.218 | 0.169 |

**Classes sit three to four times further from each other than their own members
do.** That headroom is what makes a generous threshold safe rather than reckless,
and it is the reason the design could later promise that a newly learned spell
never fails.

Against 31 strokes of undirected waving as a negative set, a threshold of 0.14
detected 100% of real gestures while accepting 16% of waving. Chained into
three-stroke spells, that same generosity gives roughly 0.4% false acceptance,
which means **per-stroke thresholds can be more forgiving as the vocabulary
grows, not less.**

## Then a four-year-old tried it

| | three-way accuracy |
|---|---|
| adult, own templates | 100% |
| child, own templates | 77% |
| **child, adult's templates** | **63%** |

Five of the child's ten triangles were classified as circles using adult
templates. **A child's triangle is not a smaller version of an adult's.**

This is the result the whole design turns on. Factory-recorded templates cannot
ship. Every child has to enrol their own.

## The fix was already in the story

The character teaches a spell and asks the child to try it a few times before it
works. That *is* template collection. It needed no new screen, no settings, and
no explanation to a parent, and it belongs to the child's profile rather than to
the adventure, so a second adventure does not re-teach a gesture they already
own.

An earlier design had a separate practice session. It was cut: **learning a spell
and casting it are the same act**, and a practice mode is a technical requirement
wearing a story costume.

## Half the gap was tracking, not drawing

The child lost a median **19% of frames** per stroke against the adult's 0%. One
stroke lost 74%. Restricting to strokes that kept 75% of their frames lifts his
accuracy from 77% to 85%.

So the recogniser was being fed damaged strokes and then blamed for the result.
That distinction produced the single most important engineering requirement in
the feature:

> **Separate a bad gesture from an unseen wand.** If the marker is lost, the
> stroke scores badly, and a naive system tells a child who drew a perfect circle
> "Almost! Bigger circle!" That is the failure that gets the product switched
> off. Below roughly 70% frames held, the response must be *"I can't see your
> wand"* and it must cost no attempt.

Build that branch before any of the charm.

## Other findings that changed the design

**Open, angular, single-stroke shapes are easier for children than closed
polygons.** Closing a triangle means returning accurately to a point you left two
seconds ago, which is hard at four. His Z was consistently his best shape at 1 to
6% frame loss; he sometimes could not complete a triangle at all. **Pick the
spell alphabet from what children draw reliably, not from what looks good on
paper.**

**Children are slower and bigger.** Median 2305 ms per gesture against an adult's
1299 ms, and 2.13 screen widths of path against 1.51. Cast windows need to be
generous, and the child needs room in frame, which argues against standing close
to the laptop.

**The interaction failed before the recogniser did.** A four-year-old cannot hold
a trigger and draw a shape at the same time, so an adult held the key. The
recording window and the gesture therefore never aligned, and most strokes carry
travel tails from the wand moving to and from the shape. **A two-person
interaction cannot ship**, and it contaminated every other number in that
session. The replacement is a timed cast window: the character speaks the spell
aloud, because a four-year-old cannot read the prompt, then the screen flips to
the spell colour with "NOW!".

## The negative result I nearly ignored

Since the mission has already named the spell, it is tempting to slide a window
over the recording and take whichever contiguous piece best matches the expected
shape. That would strip the travel tails automatically.

Measured on the child's 30 gestures against 31 strokes of random waving:

| keep at least | child's casts accepted | random waving accepted |
|---|---|---|
| 100% (no search) | 60% | 3% |
| **85%** | **73%** | **13%** |
| 75% | 83% | 16% |
| 45% | 83% | **71%** |

**Free search finds a circle inside almost any waving.** At the aggressive
setting the system accepts seven out of ten instances of a child flailing, which
means the spell is not a skill any more, it is a formality.

Conservative trimming at 85% beats the no-search baseline on both axes and is
what shipped. The aggressive version was nearly chosen on the strength of its
recall column alone, and the only thing that stopped it was having measured the
negative set at the same time rather than afterwards.

## What is still unmeasured

Whether the timed window matches a child's rhythm without an adult present.
Evening versus daylight. A different room. Whether three casts is enough to enrol
a usable template set, or whether it needs four. And whether a four-year-old
holds attention while an older sibling takes their turn.
