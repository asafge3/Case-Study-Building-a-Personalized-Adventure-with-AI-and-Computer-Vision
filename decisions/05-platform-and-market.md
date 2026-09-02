# The platform decision, made from the market rather than from taste

**Decision: web first on laptops, with a wrapped app for tablets later. Made in
about a day, deliberately, and then left alone.**

## Why it was being argued badly

The platform question had been running on first principles for weeks: native
feels premium, a boxed gift wants an icon in the dock, offline is safer. All
plausible, none tested against what anyone in this category actually does.

So I went and looked.

## What the comparables say

**Nobody in this category ships a desktop binary.** Not Osmo, not Plugo, not the
mystery-box companies. The real market choice is tablet-native versus web, and
the "premium desktop app" I was weighing is a surface none of them occupy.

**The closest analogue failed, and not because of platform.** Osmo is the nearest
thing to this product: tablet, camera, physical pieces. It sold for **$120M in
2019**, its parent imploded, and the IP was bought out of bankruptcy for
**$825,000 in 2025**. The structural problem former employees name is
**distribution**: the retail partnerships that built the brand needed capital a
bootstrapped operation cannot replicate. Platform never entered into it.

**A comparable is moving the other way on purpose.** Merge, a camera-plus-object
product, is going fully web and keeping its mobile apps only through 2027, citing
exactly the friction under discussion. Caveat worth stating: they sell to
schools, and Chromebook fleets drive that choice. I sell to families.

**Boxed-story products deliver their digital layer through a browser portal**,
not an install.

The comparables split by market, not by technology, which is why the question had
felt ambiguous for so long.

## The number that decided it

tonies SE, FY2025: **€630M revenue, +31%, profitable.**

| | FY2025 | growth |
|---|---|---|
| **Figurines (content)** | **€447M, 71%** | **+37%** |
| Hardware | €161M, 26% | +18% |
| Accessories and digital | €22M, 3% | |

**The hardware is the razor. The content is the business, and it is both the
larger and the faster-growing half.**

That reframes the platform question as the 26%, and the lower-margin,
slower-growing 26% at that. The library of adventures is the 71%. So: decide
quickly, on cost and reach, and stop spending judgement on it.

The follow-on is uncomfortable and useful. **An adventure is more like a film than
a habit**: played once, brilliantly, then done. Repeat business depends on a
content cadence, which argues for thinking about adventures three and four
earlier than feels natural.

## The distribution model, borrowed

Yoto reached roughly **$127M**, started on Kickstarter, and gets about **60% of
customers through word of mouth**. A customer-started community group reached
around 70,000 members. Marketing ran through hundreds of small creators showing
real household use. **Target came in 2026, after the growth, not as the price of
entry.**

That is the direct answer to the Osmo lesson. Osmo needed retail capital up
front; Yoto proves the other order. It also reframes a ten-family pilot as the
first move in a known sequence rather than as a small test.

## The technical decision, once the reader was gone

With no NFC reader, **no technical argument for native survived.** Browser camera
access is simpler than camera access inside an embedded webview, which needs a
delegate, a usage-description key and a hardened-runtime entitlement, none of
which had been exercised.

What remained for native was ritual, and per the comparables, no one in this
category has that ritual on desktop anyway.

**Friction inverts by device**, which is the part worth keeping:

| | Laptop | Tablet |
|---|---|---|
| **Native app** | high: installer, security warnings, or store sandboxing | **low: one tap** |
| **Web** | **zero: a URL** | decent, but web apps are second-class on iOS |

So: web on laptop, wrapped app on tablet later. One web codebase either way.

**Honest note on the tablet step:** wrapping brings back the developer account
and adds store review in the children's category, which means privacy rules,
privacy labels, and scrutiny of both the children's names collected and the
content downloaded after install. It is a deliberate later step, not a free one.

## Rejecting React Native

Considered for reaching laptops and tablets in parallel, rejected on three
grounds.

**It does not do desktop.** The desktop targets are community-maintained, behind,
and a poor fit for a video-led interface.

**It would throw away the frontend**: roughly 6,400 lines of player, admin and
setup screens, including the stylesheet that *is* the premium feel.

**And the wand and marker code**, which is browser JavaScript operating on canvas
pixels. There is no equivalent surface.

## The honest objection I have not answered

**Both category winners are aggressively screen-free and sell on it.** tonies and
Yoto ride the parental techlash. This product is screen-led, which swims against
that current, and every parent will ask about it.

The answer has to be deliberate rather than hoped-for: this is not solo
consumption on a couch. It is time-boxed, shared, parent-adjacent, and it makes
children get up and run around the house. The screen is a character who talks to
them, not a feed.

I do not consider that objection closed. It is the biggest open risk in the
business, and it is a positioning problem rather than a product one.

## Sources

- [Osmo rises from the ashes, Lowpass](https://www.lowpass.cc/p/osmo-is-back-ar-edutainment-ipad-apps)
- [Byju's buys Osmo for $120mn, Business Standard](https://www.business-standard.com/article/companies/byju-s-buys-osmo-for-120-mn-as-it-looks-to-expand-into-new-age-demographic-119011700456_1.html)
- [tonies FY2025 record results](https://www.mynewsdesk.com/us/tonies/pressreleases/tonies-continues-profitable-growth-with-record-results-in-2025-expects-strong-momentum-for-full-year-2026-expansion-of-ecosystem-around-toniebox-2-proves-a-global-success-3442746)
- [How Yoto turned parent advocacy into a $127M kids' audio brand](https://influencermarketinghub.com/how-yoto-turned-parent-advocacy-kids-audio-brand/)
- [Merge EDU web apps vs mobile apps](https://support.mergeedu.com/hc/en-us/articles/360052929752-Merge-EDU-Web-Apps-vs-Merge-EDU-Mobile-Apps)
