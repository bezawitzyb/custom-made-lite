# Help me choose: clickable prototype

Open `index.html` in any browser. It runs offline, so keep the `img/` folder next to it. Use **Restart** in the top banner to begin again.

## Paths to demo
1. **Keep it as shown** goes straight to size, which is today's fastest path.
2. **Help me choose** opens the scripted stylist: occasion, fit worry and belt. It then shows a recommendation with reasons and lets the customer change any part before the summary.
3. **I'll pick my fit and details** goes through fit (photo cards), leg (Wide only) and a Details step with three picture pairs, then the summary.

## Edge states built in
- Each fit has its own standard details on the live site (22 Nov 2026): Slim tapered B6934 has side adjusters, a single pleat and a 4 cm turn-up; Slim straight B6936 (live drawer, 23 Nov 2026, delivery 1-2 weeks) has belt loops, a flat front and a plain hem; Straight B6935 has belt loops, a flat front and a plain hem; Wide tapered B6939 and Wide straight B3769 have belt loops, a single pleat and a plain hem. Picking a fit loads that fit's standard details, as on the live site, unless the customer has already changed a detail (then their details are kept).
- Delivery follows the live site: a fit that is in stock (Straight, Slim tapered, Wide tapered) with its standard details ships in 1-2 days. Any changed detail, or a made-to-order fit (Slim straight, Wide straight), shows "Custom Made · free delivery in 1-2 weeks" and the "Made with your choices" notice. The leg step shows each leg's delivery, as the live drawer does. Events carry made_to_order. Slim and Wide both get a leg step (tapered or straight), as on the live site, and the stylist's pick shows the leg choice in its own box. The Details step marks each fit's standard options.
- When any detail differs from that fit's standard, the summary says "Made with your choices": made with your choices, so check them before picking a size. The note leaves out "can't be resold" (our cost, not the customer's concern) and shows Suitsupply's own Custom Made return rule (UK FAQ: 30 days, unworn and unaltered), which still needs confirming for CM Lite. It makes no claim about returns.
- No combination is blocked. Which fit and detail combinations Production can make is an open question.
- On phones the trouser image stays pinned above the options while the customer scrolls, so every change is visible. On the product page the main button sits clear of the footer.
- Image notes (fit "as sold today", "Preview: edited to show your details", colour preview) were removed from the customer view after the copy audit. Black and Navy are still recoloured previews; see below.
- A store link ("Ask a stylist in store") appears only on the summary, and only for pairs with changed details, since those cannot be resold. It is tracked as store_handoff_clicked.
- **Size step:** the recap includes colour. Length (Short, Regular, Long) is picked by height, and each length has its own size numbers, as on the live site: Regular 42-60 (XXS-4XL), Short 23-30 (S-4XL), Long 90-118 (S-4XL). Switching length keeps the same letter size (48 M becomes 94 M in Long). "Size chart" shows waist laid flat and inside leg for the chosen fit and length, from the live size guides for B6934 (Slim, "Vigo") and B6935 (Straight, "Milano"), read 22 Nov 2026. The live Wide pages publish no measurements, so Wide shows a note instead of a chart. New events: `length_selected` and `size_chart_opened`, and `cml_add_to_bag` now carries `length`.
- **Letter first, plus Find my size:** size buttons lead with the letter (M), with the length's own number under it (48 Regular, 24 Short, 94 Long). The label just says "Size"; the numbers update when the length changes. "Not sure? Find my size" asks height, then either your usual W size or a tape waist measurement. (The optional inside-leg field was removed in the copy audit; the override logic is still in the code but unused.) Height picks the length (live thresholds: under 168, 168-186, over 186 cm). A W size maps through the live UK/US conversion (W32 = 48). A tape measurement picks the smallest size whose waist (2 x laid flat) is at least your waist + 2 cm; the 2 cm ease is our assumption, chosen because it agrees with the W conversion. An inside leg, if given, picks the closest length for that size and fit, and the result says when it overrode height. The result selects that length and size automatically (the customer can still tap another). Near the top of a size it offers a one-tap "Take the next size instead". Wide uses the Straight chart, and says so. Events: `size_finder_opened`, `size_finder_result`, `size_finder_applied` (auto, or size_up when they take the next size); `cml_add_to_bag` also carries `letter_size`.
- Price stays at EUR 179 for every combination. Whether design options cost extra is an open question for Pricing in the gap analysis.
- On the summary, "Return terms: to confirm" marks an open question for the gap analysis. The case deck only says these garments can't be resold, not that they can't be returned.

## Stylist decision tree
The stylist asks three tapped questions and picks from a fixed rule table (`TREE` in `index.html`). The **Stylist rules** tab in Behind the scenes shows the same table, the source for each rule, and all 24 outcomes.

How it decides:
1. **Fit** comes from the body answer if it has one, otherwise from the occasion.
2. The trousers start from **that fit's standard details**.
3. Then the rules apply in order: occasion, then body, then belt. Later rules win, with one exception: at a wedding the plain hem beats the tall turn-up.

| Question | Answer | Sets | Source |
|---|---|---|---|
| What are they for? | Work | Straight, standard details | Straight to relaxed-straight for conservative offices; belt loops are the office default (Real Men Real Style, Jun 2026; Proper Cloth) |
| | Wedding or event | Straight, single pleat, side adjusters, plain hem | Side adjusters for a wedding suit (King & Allen); Suitsupply calls them "more elevated" (Suitsupply trouser guide); single forward pleat is the 2026 standard (Nathan Tailors, Apr 2026); turn-ups read less formal (Gentleman's Gazette) |
| | Everyday smart | Wide tapered, standard details (single pleat) | Fuller thigh, straight or gently tapered (Nathan Tailors 2026); Lyst searches for pleated trousers up 110% (FashionUnited, 24 Jun 2026) |
| What do trousers get wrong? | Tight on thighs or seat | Wide tapered, single pleat | Pleats make room for larger quads and glutes (Nathan Tailors 2026) |
| | I'm tall | Turn-up 4 cm; a Wide fit gets the straight leg | A turn-up gives long legs a horizontal break (Bonnegueule); wide legs flatter 5'11" and up (Real Men Real Style) |
| | I'm on the shorter side | Slim, plain hem | Under 5'9", stay closer to slim (Real Men Real Style); a bold turn-up shortens the leg (Suitsupply trouser guide) |
| | Room around the middle | Wide tapered, single pleat | Pleats and a higher rise for a fuller middle (Gentleman's Gazette; The Rake); Wide fits are Suitsupply's high-rise fits (fit guide); 51% of Dutch adults are overweight (CBS 2023) |
| | Nothing really | No change | |
| Belt? | Yes | Belt loops | With loops, wear a belt (King & Allen) |
| | No | Side adjusters | Adjusters take the waist in about 5 cm without a belt (King & Allen) |

Across all 24 answer combinations, the recommendation changes **1.1 details on average** from the fit's standard, and never more than 2. Five combinations keep the standard product. The old wedding rule changed all three details for every wedding customer.

Known tensions:
- Sources disagree on turn-ups at events. Gentleman's Gazette says they're less formal, while Proper Cloth says they don't change formality. Decision: at a wedding the formal plain hem wins over the tall turn-up. It's the one exception to the rule order, and the customer can add a turn-up in one tap.
- Trend sources are editorial or sold by tailors. Only the Lyst figure is demand data.
- Production, and the tailoring team who would own these rules, haven't seen them.

## Colours
Sand, Black and Navy, the three colours of this trouser on the live site. The customer picks a colour on the product page and can change it on the Details step and on the recommendation. The summary shows it too. Changing colour never makes the trousers "made for you". It fires a `colour_changed` event.
- **Sand** uses the real images.
- **Black and Navy** are colour previews made from the sand images. Only the fabric is recoloured, matched to the live product photos. The Black product page photo is real (`img/black/model.jpg`). The Navy product page photo is recoloured from Sand too, so it matches the rest of the Navy flow. Real configurator renders for these colours could be captured the same way as sand if needed.
- The standard details per fit were checked for sand only. Black looks the same for Slim and Wide straight. The others are assumed.

## Behind the scenes panel
- **Design notes:** the reasoning for the current screen.
- **Tracking events:** every analytics event as it fires, with its properties, matching the tracking plan.

## Deliberately left out
- Free-text or live AI chat.
- Photo or body input, and size advice.
- Fabric, buttons, pockets and lining.
- Turn-up 5 cm, double pleats and winged adjusters.

`screens/` has 2x screenshots of the key steps for the slides.

## Images
All option images are renders of Sand Pure Wrinkle-Free 4-Ply Wool (the PDP fabric). They were taken read-only from the Custom Made configurator on 22 Nov 2026.
- **Full views:** every combination of waistband (belt loops or side adjusters), front (flat or single pleat) and hem (plain or 4 cm turn-up), from the front and the side. That's 16 images, named `img/r-front-*` and `img/r-side-*`.
- **Close-ups:** waistband, pleat and hem (`img/r-zoom-*`).
- **Picture cards:** `img/thumb-*`.
- **Model photo:** from the live PDP.
- **Fit photos:** the sand fit photos from the live product page's fit drawer (Slim tapered, Straight, Wide tapered, Wide straight), captured read-only on 22 Nov 2026. Slim straight (B6936_75) was added on 23 Nov 2026, aligned to the same frame. The original photos are in `slim-fit-photos/`. Picture cards are `img/thumbfit-*`.
- **On-model previews:** `img/m-{fit}-{bl|sa}-{flat|pleat}-{plain|turnup}.jpg`, 40 images, one for every fit, leg and detail combination. Each one starts from that fit's photo. The waistband, pleat and hem areas are blended in from the other fit photos (same model, pose and camera). Picking a fit or a detail updates the On model view, and the summary shows the same preview. These are edited previews for the concept, not studio photos.

- The stylist's pick shows delivery: Ready to ship (1-2 days) or Custom Made (1-2 weeks). If the pick is Custom Made on an in-stock fit, "Use the standard details (1-2 days)" switches back in one tap (event rec_standard_details).
- "Anything trousers usually get wrong for you?" takes one body answer (thighs or middle) and one height answer (tall or shorter). Picking one from each moves on; otherwise a "Next" button appears after the first tap, and "Nothing really" moves on at once. Body sets the fit (comfort first); height sets the leg and hem. Example: tight thighs + shorter gives Wide tapered with a plain hem.

## Copy audit (23 Nov 2026)
Removed customer text that repeated something already on screen or was written for the panel: the Short/Long size-number line and the "Regular size number below each letter" label; "Slim/Wide fit comes in two legs" on the leg step; the Ready to ship box on a standard summary and on a ready-to-ship stylist pick (the bottom bar already says 1-2 days); the repeat "These are made with your choices"; "Length picked from your height" and "Tap another size to change it"; the price on the stylist card heading; "Each fit is shown as sold today"; the photo notes; the size-chart source date; "Kept as shown" (the stylist now gives a reason only for details it chose); the inside-leg field. "Standard on Straight" became "Standard".

Behind the scenes was updated the same day: design notes now cover the delivery rule, the leg cards, the multi-answer question 2, the stylist delivery box, the summary help link and the copy audit. The Stylist rules table lists all 54 outcomes with a Delivery column (40 of 54 are Custom Made).

## Bag view (23 Nov 2026)
After Add to bag the drawer shows a small bag: the item with colour, fit, details and size, a Custom Made pill when it applies, delivery and total. "Proceed to checkout" is the main action (event `checkout_clicked`); checkout itself is out of scope. "Continue shopping" goes back to the product page.
The Bag link in the header opens the bag at any time (event `bag_opened`). Continue shopping keeps the bag; Restart empties it.
Delivery & Returns on the product page opens with the live text (event `pdp_accordion_opened`). The Custom Made notice says "Returns within 30 days." only. The Details design note explains why the pleat is the third option.
Behind the scenes has a Stories tab (second tab): the user story for the open screen, with acceptance criteria mapped to what is on screen or to an event; "Show all 11" lists every story. Eleven stories after a senior-PM audit: fit and details split, a feature-flag and kill-switch story, edge cases in the size story, an accessibility and mobile story, all events and consent in the tracking story, and a Must/Should priority for v1 on each. Tabs: Design notes, Stories, Events, Stylist rules.
