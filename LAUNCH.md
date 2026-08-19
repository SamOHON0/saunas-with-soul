# Launch checklist - saunaswithsoul.ie

The site is production ready except for TWO placeholders and ONE hosting move.

## 1. Formspree (blocks forms)
`contact.html` and `events.html` still contain `[FORM-ID]`. Create the form at formspree.io, then find-and-replace `[FORM-ID]` with the real ID in both files.

## 2. WordPress must move BEFORE DNS switches (blocks bookings + images)
Booking (WooCommerce), gift vouchers and ALL images are served by the current
WordPress site at saunaswithsoul.ie. If this static site takes over the apex
domain while WP is still there, WP goes offline and bookings and images break.

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

Longer term: self-host the images in this repo so the site does not depend on
WP uptime, and swap the E2 text badge for the real award photo from Philip.

## Already done
Clean URLs (vercel.json), canonicals + OG tags on every page, sitemap.xml,
robots.txt, security headers, styled 404, FAQPage JSON-LD (15 questions),
LocalBusiness JSON-LD on the homepage, draft colour toggle removed (accent
ships), real Google review quotes, all copy from the live site or Philip's
emails.
