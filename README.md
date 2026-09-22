# milldrew.com-assets

Static art, sprites and audio for [milldrew.com](https://milldrew.com).

This repository exists so that the game bundles stay small. The Angular
apps ship only code; everything in here is fetched from a CDN at runtime,
after the main bundle has loaded.

## Why this is a separate repo

The Potato Arcade art is ~1.1GB across 346 files. Shipping it inside the
Angular bundle made the `potato-arcade-web` container image ~900MB, which
took about ten minutes to push to the registry and made every iteration on
the game painfully slow. Moving the art here drops the image to tens of
megabytes.

## Layout

```
arcade/assets/
  levels/        level backdrops and props   (598M)
  robots/        robot sprites               (364M)
  maps/          world / region / stage maps  (65M)
  practice/      practice-mode art           (8.0M)
  player-card-images/                        (7.8M)
  sounds/        game audio                  (1.5M)
  quiz-sounds/   quiz feedback audio         (432K)
  *.svg / *.png  players, ground, screens
```

## How it is served

Consumed through jsDelivr, which serves any public GitHub repo as a CDN
with CORS enabled and no credentials:

```
https://cdn.jsdelivr.net/gh/Milldrew/milldrew.com-assets@main/arcade/assets/<path>
```

The apps read this prefix from a single build-time value, so pointing them
at a different origin (a Cloudflare R2 bucket, or a local folder during
development) is one environment variable and no code change.

## Pinning

`@main` always serves the newest commit. For a reproducible deploy, pin a
tag or commit instead:

```
https://cdn.jsdelivr.net/gh/Milldrew/milldrew.com-assets@v1/arcade/assets/<path>
```

jsDelivr caches aggressively; a mutable ref like `@main` can take up to
24h to reflect a change, while a tag is immutable and cached forever.
Tag a release when the art changes and bump the ref in the app.

## Licence

All rights reserved. These are the original art and audio assets for
milldrew.com, published here so browsers can fetch them without
credentials — not offered for reuse.
