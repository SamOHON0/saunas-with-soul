# Launch checklist - saunaswithsoul.ie

The site is production ready except for THREE placeholders and ONE hosting move.

## 0. PartyOps session booking (NEW - replaces WooCommerce for sessions)
Public session bookings now go through PartyOps: every "Book now" / "Book a
session" button on the site points at `/book` (book.html), which embeds the
PartyOps sessions widget (live slots, seat picker, card payment via Stripe).
Until the ID below is filled in, /book shows a friendly call/WhatsApp fallback
instead of the widget - so the site can ship before the account exists.

To go live with online booking:
1. Create Philip's PartyOps account at https://partyops.app/admin/signup
   (or hand over a login the usual way).
2. In PartyOps admin: connect Stripe (Billing), then open /admin/sessions:
   - Enable sessions.
   - Add the two session types:
     * "Sauna + Cold Plunge (Folklore Park)" - 50 min, 10 seats, EUR 15/seat,
       location "Carlingford Folklore Park, Ghan Road, A91 X820"
     * "Sauna + Cold Plunge (Carlingford Pier)" - 50 min, 10 seats, EUR 15/seat,
       location "Carlingford Pier"
   - Add the weekly slots (the "Repeat weekly until" field generates a whole
     season per weekday in one go; Pier times move with the tide, so add those
     week by week).
   - Turn ON "Require payment at booking".
3. In `book.html`, replace `PARTYOPS_BUSINESS_ID` (one place, in the script at
   the bottom) with the business ID shown on the /admin/sessions page.
4. Smoke test: book and pay for a seat end to end; check the confirmation
   email arrives and the booking shows in /admin/sessions.

Gift vouchers still run through WooCommerce (see section 2) until PartyOps
does vouchers.

## 1. Two link placeholders
- `contact.html` and `events.html` contain `[FORM-ID]` (Formspree).
- `awards.html` contains `[LMFM-LINK]`: the "Click here to listen" button for the LMFM
  interview with Brian Farrelly. Grab the real URL from the old WordPress
  /our-awards/ page before that site moves.

## 1b. Formspree (blocks forms)
`contact.html` and `events.html` still contain `[FORM-ID]`. Create the form at formspree.io, then find-and-replace `[FORM-ID]` with the real ID in both files.

## 2. WordPress must move BEFORE DNS switches (blocks vouchers + images)
Session bookings now run through PartyOps (section 0), so WooCommerce is no
longer needed for those - but gift vouchers and ALL images are still served by
the current WordPress site at saunaswithsoul.ie. If this static site takes
over the apex domain while WP is still there, WP goes offline and vouchers
and images break.

Order of operations:
1. Move WordPress to https://book.saunaswithsoul.ie (hosting: add subdomain,
   update WP Address/Site Address, Woo + Stripe/PayPal callback URLs).
2. In every html file, find-and-replace:
   - `https://saunaswithsoul.ie/product/`            -> `https://book.saunaswithsoul.ie/product/`
   - `https://saunaswithsoul.ie/wp-content/`         -> `https://book.saunaswithsoul.ie/wp-content/`
   - `url=saunaswithsoul.ie/wp-content/`             -> `url=book.saunaswithsoul.ie/wp-content/`  (wsrv image proxy)
3. Point saunaswithsoul.ie DNS at Vercel, add the domain to the Vercel project.
4. Smoke test: book a session end to end, buy a gift voucher, submit the
   contact form, check images load, check https://saunaswithsoul.ie/sitemap.xml.

Photos: 34 self-hosted images in /images (Philip's WhatsApp drop, resized and
compressed, ~6MB total, all lazy-loaded except heroes). Homepage gallery is 20
images. An /awards page was added using the copy from his WordPress Our Awards
page plus the award photos from the drop.

Photos are self-hosted in /images (Philip's photo drop, optimized and
renamed) and the E2 text badge has been replaced with the real trophy photo.
Still served from WP and covered by the find-and-replace above: the favicon,
the Prestige winner-logo.png, and the og:image (sauna.jpeg). The raw
"WhatsApp Image" files and video in the folder root are gitignored; move or
delete them whenever.

## Already done
Clean URLs (vercel.json), canonicals + OG tags on every page, sitemap.xml,
robots.txt, security headers, styled 404, FAQPage JSON-LD (15 questions),
LocalBusiness JSON-LD on the homepage, draft colour toggle removed (accent
ships), real Google review quotes, all copy from the live site or Philip's
emails.
