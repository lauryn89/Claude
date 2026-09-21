# kttape.com.au — robots.txt

kttape.com.au is hosted on Webflow. Webflow does not deploy from this (or
any) git repository — there is no build/publish pipeline connecting this
repo to the live site. `robots.txt` therefore can't be "pushed" here the
way it would be for a self-hosted app; it has to be set inside Webflow
itself. This repo holds the canonical file as source of truth and a record
of intent.

## File

[`robots.txt`](./robots.txt) — valid per [RFC 9309](https://www.rfc-editor.org/rfc/rfc9309):

```
User-agent: *
Allow: /

Sitemap: https://kttape.com.au/sitemap.xml
```

This allows all crawlers to index the whole site and points them at the
sitemap. Webflow serves this at `/robots.txt` as `text/plain` with a 200
once it's applied (see below), satisfying the RFC 9309 requirements and
the `checks.discoverability.robotsTxt` check at
[isitagentready.com](https://isitagentready.com).

## How to apply this in Webflow

1. Open the Webflow project (Designer or dashboard) for kttape.com.au.
2. Go to **Project Settings → SEO → Indexing**.
3. Paste the contents of `robots.txt` into the robots.txt editor field.
4. Save, then **publish the site** — settings changes don't go live until
   a publish.
5. Verify: visit `https://kttape.com.au/robots.txt` and confirm it returns
   HTTP 200 with `Content-Type: text/plain` and the expected body.

If `Project Settings → SEO → Indexing` isn't available on the current
Webflow plan, robots.txt editing requires a paid site plan.

## Sitemap

The rule above assumes `https://kttape.com.au/sitemap.xml` exists.
Webflow can auto-generate this under the same **SEO** settings tab
(`Auto-generate sitemap`sitemap option) — enable it if it isn't already,
publish, and confirm `https://kttape.com.au/sitemap.xml` resolves before
relying on the reference in `robots.txt`.

## Cloudflare AI Crawl Control (optional)

If kttape.com.au sits behind Cloudflare (as a DNS/CDN proxy in front of
Webflow), [AI Crawl Control](https://developers.cloudflare.com/ai-crawl-control/)
can additionally manage AI-bot-specific rules from the Cloudflare dashboard
without touching Webflow's robots.txt editor. This is optional and layered
on top of, not a replacement for, the steps above.

## Validate

```
POST https://isitagentready.com/api/scan
Content-Type: application/json

{"url": "https://kttape.com.au"}
```

Confirm `checks.discoverability.robotsTxt.status` is `"pass"`.
