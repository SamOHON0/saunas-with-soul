# Draft 4 - Philip's Round 2 changes (24 Aug 2026)

Built from Philip's WhatsApp feedback of 24 Aug, 10:23-10:26. Every item he asked for that was not
blocked on a decision is done. Copy the contents of this folder over the repo, review, commit, push.

## Files changed
index.html, panoramic.html, awards.html, sessions.html, events.html, faqs.html, contact.html, 404.html,
sitemap.xml

## Files added
terms.html, privacy.html, disclaimer.html, lost-and-found.html
images/prestige-awards-trophy.jpg, images/pier-sauna-lough-view.jpg, images/the-soother-sign.jpg

## What was done

1. **Ticker is moving again.** It had been rebuilt as a static wrapped flex band with no animation and no
   @keyframes at all. Restored as a proper marquee: the item list renders twice so a -50% translate loops
   seamlessly, 52s desktop / 34s mobile, pauses on hover, white edge fades. Under
   prefers-reduced-motion the duplicate copy is hidden and it falls back to the static wrapped band.

2. **Display type reduced site-wide.** Top ends of the large clamps: hero 110 -> 84, page hero 72 -> 58,
   section h2 68 -> 52 (the size he flagged), split h2 48 -> 40, contact h2 58 -> 46, stat numerals
   58 -> 46. Body copy untouched.

3. **Hero H1 removed.** "Heat - Cold - Carlingford" is gone. The page keeps a single H1, now
   "Saunas with Soul, Carlingford" at clamp(26px,3vw,40px), so the big display block is gone but the page
   still has one H1 for SEO. **Confirm with Philip** whether he wants the heading gone entirely, since
   that H1 was his own 11 Aug instruction.

4. **Awards moved higher and given a photo.** The proof band now sits directly under the hero, above the
   ticker. It leads with the new awards-night photo (clickable through to /awards) beside the two badges
   and the four stats.

5. **Hero E2 emoji fixed.** The trophy emoji in the hero pill is now the real E2 trophy photo, matching
   the proof band. That was known gap 3.

6. **Maps and pin links on all three locations.** Each location block now has a lazy-loaded Google Maps
   embed plus an "Open in Google Maps" link using Philip's exact short links. The embeds are darkened with
   a CSS filter to sit in the black theme. **The sandbox could not load google.com/maps, so the map
   panels are unverified visually. Eyeball them on the Vercel deploy.** Two things to decide:
   - The embed sets Google cookies and sends the visitor's IP to Google. The privacy policy names it, but
     if you want zero third-party requests, swap to a static map image with the same pin link.
   - Pins are resolved from address strings, not from Philip's short links (Google's embed does not accept
     short links). Check each pin lands where he means, especially South Commons.

7. **New Carlingford Pier image.** The sauna-interior-with-lough-view shot he sent replaces
   pier-barrel-plunge.jpg as the Pier block image.

8. **The Soother.** Added three ways on the Panoramic page:
   - New step 04 in the retreat ritual, using his wording. Old 04 and 05 renumbered to 05 and 06.
   - A third card in "Choose your challenge", alongside The Awakener and The Pretender. Section label
     changed to "Heat - Cold - Soak - Relax" since The Soother is warm, not a cold plunge.
   - Its own section with the Soother sign photo and his "ultimate Carlingford wellness experience" copy.

9. **Panoramic is enquiry only.** The nav CTA and the sticky mobile bar on panoramic.html now say
   "Send enquiry" and go to /contact instead of the Folklore booking product. The page's own CTAs were
   already enquiry-based.

10. **Booking message on the homepage.** A slim band directly under the ticker: "For booking enquiries for
    dates and times not shown on the booking system, please contact us directly", with the phone number
    and email.

11. **Four legal pages drafted** and linked from the footer of every page, added to sitemap.xml.
    cleanUrls in vercel.json already handles /terms, /privacy, /disclaimer, /lost-and-found.

## BEFORE LAUNCH

- [ ] **Remove the draft banners from the four legal pages.** Each has a yellow "Draft for review" box
      wrapped in `<!-- REMOVE BEFORE LAUNCH -->` comments. Search the repo for `draftnote`.
- [ ] **Fill the placeholders in the legal pages.** They are the ice-blue bracketed spans, class `ph`.
      Registered business name, address, CRO number, deposit terms, voucher validity, retention periods,
      and the exact names of the booking, payment, hosting and form providers.
- [ ] **Have the legal pages checked.** They were drafted from how the business actually operates, not by
      a solicitor.
- [ ] Self-host the three WordPress assets: the favicon / wordmark logo
      (wp-content/uploads/2025/05/Saunasforsoul.ie-10.png), the Prestige winner badge
      (wp-content/uploads/2026/03/winner-logo.png) and the og:image (sauna.jpeg). All three still break in
      any environment that cannot reach saunaswithsoul.ie, and they will break when WordPress moves.
      Known gap 2, still open.
- [ ] Formspree [FORM-ID] in contact.html and events.html.

## Still blocked on Philip

- Wellness pricing table. His figures have the columns swapped on two of three rows, the numbers do not
  match any table currently on the site, and "16+" has no upper bound so effective pp needs a rule.
- Booking system with date and location blocking for Pier and Folklore.
- Whether the hero heading comes off entirely (see 3).
- The "glare" photo: that is sunlight on his laptop screen, not a fault in the site image.

## Not done, deliberately

- Font change. Deferred to its own pass. His reference is an iOS Markup font picker with "Oxygen"
  selected, which is a different typeface from Google Fonts Oxygen. The site is on Quicksand. Needs a
  side-by-side comparison before swapping type across twelve pages.
