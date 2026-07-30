# 757 Roof Wash — PPC Landing Site

Hugo static site for Google Ads landing pages, one per city. Separate from the
main sudsyhousewashing.com project — safe to deploy without touching that site.

## 1. Deploy

1. Push this folder to a new **GitHub repo**.
2. In Netlify: **Add new site > Import from Git**, pick the repo.
   Build command `hugo --minify`, publish directory `public` (already set in
   `netlify.toml`).
3. Point `757roofwash.com` at the Netlify site (Domain settings > Add custom domain).
4. Netlify Forms: no setup needed — Netlify auto-detects the form because of
   `data-netlify="true"` on the form tag. Go to **Site settings > Forms** after
   the first deploy to see submissions, and add your email under
   **Forms > Notifications** to get emailed for each lead.

## 2. Turn on the CMS (so you can add new cities without touching code)

1. In Netlify: **Site settings > Identity > Enable Identity**.
2. Under Identity settings, enable **Git Gateway**.
3. Under Identity > Registration, set to **Invite only**, then invite yourself.
4. Visit `757roofwash.com/admin/` and log in. You'll see "Roof Cleaning City
   Pages" — click **New Roof Cleaning City Pages**, fill in the city name and
   URL slug, and publish. That's the entire process for adding a new city.

## 3. Google Ads conversion tracking (currently placeholder values)

The site is wired for tracking but needs your real IDs — nothing will fire
correctly until you fill these in:

1. In Google Ads: **Tools & Settings > Conversions > New conversion action >
   Website**. Name it something like "Roof Quote Form Submit."
2. Google Ads gives you an ID (`AW-XXXXXXXXX`) and a label. Put both into
   `config.toml` under `[params]`:
   ```
   googleAdsConversionId = "AW-XXXXXXXXX"
   googleAdsConversionLabel = "XXXXXXXXXXXX"
   ```
3. Commit/push. The conversion fires on `/thank-you/`, which the form
   redirects to after a successful submit — this is more accurate than firing
   on click, since it only counts real completed submissions.
4. Point each Ad Group's final URL at its matching page, e.g. the Chesapeake
   ad group's ads should link to `757roofwash.com/roof-cleaning-chesapeake-va/`,
   not the homepage — the headline should match the page H1 for Quality Score.

**Phone calls:** the site doesn't yet track calls (you said no CallRail). Two
free options, either works without new tooling:
- Turn on **Google Ads call extensions / call-only ads** with Google's own
  forwarding number — Ads reports call conversions natively, no per-page
  attribution though.
- Skip call tracking for now and treat the form as your primary conversion
  event; the phone number is still on every page as a secondary path.

Say the word if you want me to build out CallRail (or an alternative) later —
it's a bigger lift since it needs a script snippet + dynamic number insertion.

## 4. Before this is truly ready to run traffic to

- [ ] Replace `googleAdsConversionId` / `googleAdsConversionLabel` (above)
- [ ] Upload real before/after photos per city via the CMS gallery field
      (currently shows a striped placeholder)
- [ ] Fill in `[DATE]` on `/privacy-policy/` and `/terms/`, and have someone
      review the legal language — it's a starting template, not legal advice
- [ ] Set the real Google Ads account tracking template / auto-tagging (gclid)
      if you use offline conversion import later
- [ ] Confirm `phoneHref` in `config.toml` is right — it drives every "tel:"
      link and the mobile sticky call bar

## Notes on structure

- Every page has `noindex, nofollow` in the meta robots tag on purpose — this
  is an ads-only site, and near-duplicate city pages competing with the main
  site or each other in organic search would only cause problems. Remove that
  if you ever decide to let these pages rank organically too.
- Adding a third city: duplicate one of the files in `content/roof-cleaning/`
  (or use the CMS), it inherits the whole layout automatically — no template
  work required.
