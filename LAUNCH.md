# Launch checklist - saunaswithsoul.ie

The site is production ready except for THREE placeholders and ONE hosting move.

## 0. PartyOps session booking (NEW - replaces WooCommerce for sessions)
Public session bookings now go through PartyOps: every "Book now" / "Book a
session" button on the site points at `/book` (book.html), which embeds the
PartyOps sessions widget (live slots, seat picker, card payment via Stripe).
DONE 4 Sep 2026: Philip's PartyOps account exists (info@saunaswithsoul.ie,
business ID 1f95d665-6d6b-44b8-b112-5a5c123e95d3, temp password in
Websites/_philip-partyops-login.txt, not in git). book.html points at it.
Sessions are enabled, the three session types and the seeded weekend slots
were copied over from Sam's preview account, and it runs in request mode
(no card payment) until Stripe is connected. If the ID is ever blanked,
/book falls back to a call/WhatsApp card.

To go live with online booking:
1. Send Philip the login and have him change the password in Settings.
2. In PartyOps admin: connect Stripe (Billing), then open /admin/sessions:
   - Check the session types (already there: Folklore Park, Carlingford Pier,
     Women's Only; 50 min, 10 seats, EUR 15/seat).
   - Add the slots. Philip's routine (his words, 4 Sep 2026): sessions start
     every hour and he opens up each weekend once he knows his hours. In
     "Add slots": pick the session type, Date = Saturday, "Also on" = Sunday,
     First = 10:00, Last = 15:00, Every hour, then Add slots. Six hourly
     sessions land on both days. Repeat per type (Folklore Park and Pier
     have different hours; Pier moves with the tide, so give Pier its own
     times, or type odd times into "specific times"). "Repeat weekly until"
     is optional if a stretch of weekends is the same. Each day has "Close
     day" / "Reopen day" and "Delete unbooked" for weather or mistakes.
   - Turn ON "Require payment at booking".
3. In `book.html`, swap the TEMP business ID (currently Sam's PartyOps preview account,
   so the draft site books end to end) for Philip's real business ID from his
   /admin/sessions page. One place, in the script at the bottom of the file.
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
