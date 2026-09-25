# Case Study: Building An AI-Powered Adventure in the Real World

I have spent my career as a product leader building and launching new products. I started Mystic Mission as a passion project to do evaluate how AI can empower a Solo founder: build the technology, put a physical product in children's hands, watch them use it, and change the design when the evidence demanded it.

I also wanted to explore what AI personalization, computer vision, and voice recognition could make possible in children's play, and how quickly I could build an experience that works across homes, laptops, and tablets. This case study shows how I work as both a Product Director and a hands-on builder, while testing what it would take to make the experience repeatable at scale.

**The idea:** an AI-personalized adventure game that turns a family's home into the game world.

Children ages 4–9 receive a physical kit, hunt for collectible medallions hidden around their home, scan them with a laptop or tablet camera, and cast spells with a light-tipped wand and their voices. Animated characters guide the adventure. The story uses the children's names, their pet, and the actual rooms in their house, and ends with a real treasure they can hold.

I took it from an idea to a playable, family-tested product and a physical kit in roughly two months. I designed the experience, built the prototype and test harnesses, ran the playtests, and made the hardware, platform, and manufacturing decisions. This case study follows the technical work and the product judgment behind it: what I measured, what failed with real children, and what I changed.

**This repository documents the product decisions and evidence.** The application code, story content, and character IP are private.

## The experience

1. A parent enters the players' names and the rooms available for hiding items, then hides the medallions and treasure.
2. AI-generated comic panels weave those details into the adventure. Recorded character videos supply the shared story and performances.
3. Children search their real home and present each medallion to the camera to advance the story.
4. They aim the wand at targets on screen and call out spell words. The adventure responds to what the camera and microphone detect.
5. The final discovery is a physical object, not just an animation or a score.

The separation between recorded video and generated panels is a product constraint: video is the same for every family; names, pets, and room-specific directions belong in the personalized panels. An earlier version blurred that boundary and worked for my household but could not reliably travel to another one.

## What makes it work

### AI makes the story personal

Recorded character scenes provide the shared story and performances. Generated comic panels bring in the children's names, their pet, and the rooms their parent actually chose. The two formats work together so the adventure feels specific to a family without generating every scene as video.

### Computer vision connects the kit to the story

The built-in camera reads printed ArUco markers on physical medallions and tracks the illuminated tip of the wand. Finding an object advances the story; aiming the wand at targets casts a spell. I built a controlled vision test harness alongside the gameplay tests.

### Voice makes casting feel like magic

Children call out spell words as they move the wand. The game has to accommodate the way children actually speak and move, including words shouted early and gestures that are close enough to the intended action.

### The browser brings it onto family devices

Parents can set up and play on a laptop or tablet without installing a native app. The physical kit provides the tactile part of the experience.

The hard part was making these layers work for a child in a real home. A detector can perform beautifully on a captured frame while a five-year-old still struggles to hold a coin up, keep a wand in view, or say a word at the expected time. Those are product failures even when the underlying model or algorithm is correct.

## Decisions that changed the product

### I removed the NFC reader

Camera-read markers let a medallion trigger the next story beat without a USB accessory. That removed a hardware component from the kit and roughly a thousand lines of supporting code. The vision tests below helped me make the change before committing to the physical pieces.

### I split personalization from the recorded scenes

A fixed video cannot truthfully name each child or send them to their actual kitchen. An earlier version blurred this distinction and only worked in my home. Generated panels now carry those details, while the animated scenes retain their performance and production quality.

### I replaced air-drawn runes with visible targets

The older child completed 27 of 33 shape casts; the younger child had not completed one unaided. With target-based casting, children completed 32 of 34 casts across an adventure, including the younger child's first unaided spell.

### I adjusted casting to children's timing

Children shout a word before, during, or after moving the wand. The interaction needs to listen across the cast and let a recognized word help a messy gesture succeed.

### I chose the browser for the first family tests

It lets me test the entire parent setup and play flow on devices families already have. Whether a native app improves the experience is a later decision.

## The evidence, including its limits

### Reading the medallions

In a controlled camera test, the marker reader made **zero wrong identifications across 52 real confirmations and 960 synthetic trials**. A distance sweep produced **26 correct reads from 26 raises**, with the smallest readable marker measuring **9 pixels**.

That last number was easy to overinterpret. Nine pixels was close to OpenCV's configured rejection floor: it showed where the detector stopped considering the marker, not proof that every nine-pixel identification would be correct. The sweep also had only one marker ID in view, so it could not expose confusion between IDs. The next discriminating test puts all eight IDs in play at that size. I do not treat a clean lab result as a field reliability claim.

### Making spells usable by children

An early three-shape recognizer scored **100% on an adult**, then **77% on a four-year-old's own templates and 63% on the adult templates**. The child lost a median **19% of tracking frames**, compared with 0% for the adult. Personal calibration improved the result, but it did not solve the underlying interaction for the youngest player.

I tried sliding a recognition window over a recorded gesture. It raised accepted child casts from **60% to 83%**, while also raising acceptance of random waving from **3% to 71%**. That was the wrong trade. More conservative trimming improved recognition without letting random movement pass at that rate.

The larger fix was changing the game action. Children now hit visible targets with the tracked wand tip. In one adventure run they completed **32 of 34 casts**, with a **five-second median per cast**. A later run awarded the top grade on **37 of 41 casts**, showing that I had made the interaction usable but had not yet built a meaningful difficulty ramp. Target count and timing are now scene-level design controls.

These are small playtests, useful for changing a prototype. They are not population-level performance estimates.

## Claims I withdrew

I keep the retractions here because they explain the design better than a polished success story would.

- **“Children need about fifteen attempts to learn a gesture.”** I had interpreted my own sequence of circles as learning. I was deliberately varying the shapes to test the recognizer. The claim had no evidence.
- **“The click of the lock is what children remember.”** One child said finding a locked box was a favorite moment; another named opening it. Neither mentioned the click. I had added an interpretation that the observations did not support.
- **“A five-year-old can draw a rune.”** Sixteen correct shapes looked encouraging until I checked the procedure: I had guided his hand for all sixteen. Unaided success had not been demonstrated. This retraction led directly to target-based casting.

## What I would do differently

I measured the detector more carefully than the child-to-camera interaction. In 13 of 14 timing trials, the marker was already in frame when the clock started, so those trials could not tell me how long a child takes to present it after searching a room. The next test starts at the moment of discovery and measures the whole action.

I also lost three data runs to test procedure. A default dropdown silently labelled 31 assorted shapes as circles; a focused selection control consumed the spacebar and made another gesture look broken. I changed the harness to drive and record the protocol explicitly.

## Commercial and platform thinking

Physical kits can create an installed base for additional adventures, but the content has to earn repeat purchases. [tonies' FY2025 results](https://ir.tonies.com/news/tonies-continues-profitable-growth-with-record-results-in-2025-expects-strong-momentum-for-full-year/b94d3519-0bdf-4b06-8664-ee5066fcc297) are one useful analogue: €447 million of €630 million in revenue came from figurines, versus €161 million from Tonieboxes. That supports testing a reusable kit with an expanding adventure catalog. It does **not** establish Mystic Mission's economics or prove that a browser is the right long-term platform.

The next milestone is straightforward: can families open the box, set up the adventure, and have their children finish it without me in the room? That is the test the first family kits are designed to answer.

---

*Prototype built with Python and browser JavaScript. The application code, story content, and character IP are private.*
