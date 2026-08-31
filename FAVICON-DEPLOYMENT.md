# Getting the VENRAS icon to show next to venras.com

The files are all correct and linked. What follows is the deployment side,
because a browser will not show a site icon until it has actually fetched one.

## 1. The icon files must sit at the domain root

Upload the **entire contents** of this folder to your web root — not into a
subfolder. Every icon path in `index.html` is root-relative (`/favicon.ico`), so
they resolve the same way from any page on the site.

```
https://www.venras.com/favicon.ico            <- required
https://www.venras.com/favicon.svg
https://www.venras.com/favicon-16.png
https://www.venras.com/favicon-32.png
https://www.venras.com/favicon-48.png
https://www.venras.com/apple-touch-icon-180.png
https://www.venras.com/icon-192.png
https://www.venras.com/icon-512.png
https://www.venras.com/icon-512-maskable.png
https://www.venras.com/site.webmanifest
```

`/favicon.ico` matters most. Browsers request it at the root by convention even
when no `<link>` tag points at it, and several places that show a site icon —
older browsers, some feed readers, Windows desktop shortcuts — only look there.

Open each URL in a browser after uploading. If any returns 404, the icon will
not appear.

## 2. Serve both apex and www

If people type `venras.com` without the `www`, that hostname needs to serve the
site too (or 301-redirect to the `www` version). A browser treats
`venras.com` and `www.venras.com` as different origins for icon caching.

## 3. Expect a delay, and clear the cache while testing

The icon Chrome shows in the address-bar dropdown comes from its **local
favicon cache**, which is only populated after you have actually visited the
site at least once. It is not fetched when you type a URL.

So while testing:
- Visit `https://www.venras.com` once, normally.
- Hard-refresh with **Ctrl/Cmd + Shift + R**.
- Check the browser tab first — the tab icon updates immediately, the omnibox
  dropdown lags behind it.
- If the tab shows a stale or blank icon, open
  `https://www.venras.com/favicon.ico` directly, force-refresh that, then reload
  the site.
- A private window is the fastest way to see a clean first-load result.

Chrome caches favicons aggressively and will sometimes hold a blank entry for a
domain it failed to fetch from earlier. If you loaded the site before the icons
were uploaded, that blank is what it remembers.

## 4. Check the HTTP response

The server must return the right content type and a `200`:

```
curl -I https://www.venras.com/favicon.ico
```

Look for `Content-Type: image/vnd.microsoft.icon` (or `image/x-icon`) and
`200 OK`. Some hosts return an HTML 404 page with a `200` status, which browsers
silently discard.

## 5. About the icon itself

The icon is the mark on a white tile, at 86% of the tile width.

White rather than dark navy: at 16 px a dark tile collapses into a blob and the
wing inside it disappears. On white the silhouette survives, and it reads on
both a light and a dark browser theme — Chrome's dark tab strip included.

A tile rather than the bare wing on transparent: the VENRAS mark is wide and
short, so dropped into a square 16 × 16 box it becomes a few thin strokes. The
tile gives it a consistent footprint at every size.

The same white tile is used across the favicons, the iOS home-screen icon, the
Android icons and the social avatars, so the brand looks identical wherever an
icon appears. The Open Graph link-preview image and the LinkedIn banner stay
dark — those are large-format artwork, not icons.

Everything is generated from the same vector as the logo, so nothing drifts.
