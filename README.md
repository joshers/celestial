# Celestial

A tiny mobile-first web app that shows, at a glance, which planets (and the Moon) will be at least **30° above the horizon** tonight, and when.

## What it shows

- **One night at a time**: the window runs from sunset to the next sunrise, so each body gets a single continuous time range. Times that fall after midnight are tagged with the day (e.g. `3:54 AM Fri`).
- **Moon, Mercury, Venus, Mars, Jupiter, Saturn, Uranus, Neptune**, sorted by when they first clear 30°. Bodies that don't get that high say so.
- **Max altitude** reached during the night for each body.
- **Moon phase** with an illustrated icon and percent illuminated.
- **‹ / ›** to step to other nights.

In places where the Sun doesn't set or rise (polar summer/winter), it falls back to a noon-to-noon window.

## Location

On first load it asks the browser for your location. If you decline, or geolocation isn't available, enter a latitude and longitude at the bottom of the page. A manually entered location is saved in `localStorage` and reused on later visits; **Use my location** clears it and goes back to geolocation.

Times are shown in the device's time zone.

## Running it

It's a single static file with no build step: [`index.html`](index.html).

```bash
python3 -m http.server 8000
```

Then open <http://localhost:8000>.

Browsers only allow geolocation on secure origins. `localhost` counts, but to use it on a phone, host it over **HTTPS** (GitHub Pages, Netlify, Cloudflare Pages, etc.). Over plain HTTP on a LAN address, manual coordinates still work.

## How it works

Positions come from [Astronomy Engine](https://github.com/cosinekitty/astronomy), loaded from jsDelivr. For each body the app samples topocentric altitude (with atmospheric refraction) every 2 minutes across the night and reports the span where it's ≥ 30°. Sunset and sunrise come from `SearchRiseSet`, and the Moon phase from `MoonPhase` at the middle of the night.

To change the altitude threshold, edit `MIN_ALT` near the top of the script.

An internet connection is needed to load the library.
