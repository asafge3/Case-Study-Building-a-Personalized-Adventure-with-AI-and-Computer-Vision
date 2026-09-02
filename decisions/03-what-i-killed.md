# What I killed

A zero-to-one project is mostly subtraction. These are the things that were
built, or nearly built, and then removed, with the reason each one died.

---

## The Magic Zone

**What it was.** A camera-watched tabletop. The laptop lid tilted down so the
camera looked at a surface, where children would arrange, sort, count and
assemble physical objects. It was the most ambitious version of the product: it
promised mechanics far beyond scan-to-advance.

**Why it died.** Tilting the lid down to see a table makes the screen unusable.
The screen is where the character lives, and the character is the product. There
was no arrangement of one laptop that let a child both look at Pip and work on a
surface Pip could see.

**What it cost to find out.** A prototype, and the willingness to notice that the
demo only worked because I was the one holding the laptop.

**What survived.** One forward-facing camera at the lid's normal angle, carrying
both mechanics. No mat, no placement surface, no homography, no per-session
calibration. The constraint made the product simpler than the version that
inspired it.

---

## The NFC layer

Covered in full in [Runes over NFC](01-runes-over-nfc.md). Removing it deleted a
hardware component from the box and about a thousand lines of code, including a
batch-provisioning workflow whose only purpose was applying stickers to objects.

**The lesson worth keeping:** the port to the next platform got *smaller* by
deleting a working subsystem. Dead weight is easiest to spot when you are about
to have to carry it somewhere.

---

## The native Mac app

**What it was.** A packaged desktop application, the thing I assumed was the
premium version because it has an icon in the dock and feels like a gift.

**Why it died.** Once the reader was gone, **no technical argument for native
survived**. Browser camera access is if anything simpler than camera access
inside an embedded webview, which needs a delegate, a usage-description key and a
hardened-runtime entitlement, none of which had ever been exercised because the
packaged build had never been run.

Then the market answered the rest of it: **nobody in this category ships a
desktop binary.** Not Osmo, not Plugo, not the mystery-box companies. The real
choice was tablet-native versus web, and the desktop app I was weighing was a
surface none of the comparables occupied.

**What it bought.** Deleting the never-run build, the developer account, signing,
notarization, the security warning on first launch, and ten signed rebuilds every
time a pilot family needed a fix. A bug found at family three becomes a deploy.

**What it cost.** Play now needs a live connection, which is only acceptable
because I had already retired the offline requirement as unrealistic for the
actual customer: suburban families on home wifi.

---

## The naming mechanic

**What it was.** In a playtest, children named the golden egg "Goldy" completely
unprompted, with no field, no question and nobody asking. That was such a good
moment that it became a requirement: the software should capture the name and say
it back.

**Why it died.** Those are not the same claim. **The naming happens either way.**
Children name things. They do not need a text box, and building one confuses "we
observed a lovely moment" with "we should own that moment".

And a shared name buys something capture cannot. Charizard is famous because
everyone means the same dragon. A name only works at school, on a box, on a coin
and in a "scan any seal" page if every family's is identical.

**What survived.** The character has a fixed name, printed on the coin, and the
guide's closing line explicitly blesses the nickname: *"he's called Auren, by the
way, though I suspect you'll be calling him something else by teatime."* A child
who decides he is Goldy is invited rather than contradicted. **The species is
printed; the nickname is yours.**

That is the Pokémon rule, and following it removed a feature from the build list
rather than adding one.

---

## The locked treasure chest

**What it was.** A physical box the treasure sits inside, opened at the end.

**Why it died here.** The rule I arrived at: **the chest exists to supply
ceremony that the hero item cannot supply on its own.** So the question is always
*can the hero item hold the finale by itself?* A large golden dragon egg, yes. A
small stone or a shard or a key, no, and those get a box.

That single rule explained both playtests at once, and it means the chest is not
cut, it is conditional. It comes back in an adventure whose treasure is small.

---

## A feature I nearly built twice

Late in the design I wrote that the animated spell stroke and the continuous
during-stroke feedback were "not built yet", and scheduled them.

Reading the actual code showed both already existed, and in more detail than I
had just specified: a green start dot, an arrowhead riding the drawn end, a ghost
overlay, the whole stroke rendered live in the spell's colour with four age-fade
bands, no countdown, and a drawn success moment.

**I had specified a feature over the top of my own working implementation.** It
cost nothing because I checked before building, but it is the clearest argument I
have for reading the code before writing the roadmap.
