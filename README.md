# mLearn CDN Mirror

Mirror of the mLearn language-data and Python runtime archives normally
served from `cdn.kikan.net` (Cloudflare R2), for regions where Cloudflare is
slow or blocked. Archives are hosted as GitHub Release assets, served from
GitHub's own CDN rather than Cloudflare.

## What's here

- `language-catalog.json` — same catalog as
  https://mlearn.kikan.net/language-catalog.json, with every archive URL
  rewritten to this mirror.
- `runtime-catalog.json` — same catalog as
  https://mlearn.kikan.net/runtime-catalog.json, rewritten likewise.
- Release `cdn-v1` — the archives themselves
  (https://github.com/kikans/mlearn-cdn-mirror/releases/tag/cdn-v1).

Archive filenames embed the first 12 hex chars of the file's SHA-256, so a
URL never points at different bytes and unchanged archives are never
re-uploaded.

## Using the mirror

Usually there is nothing to configure. When the default catalogs
(mlearn.kikan.net, fronted by Cloudflare) are unreachable, the mLearn app
automatically probes mirror0.cdn.kikan.net, mirror1.cdn.kikan.net, ... for
the same catalog paths, and uses the last reachable mirror. The probe
domain is configurable in Settings -> Connection -> Catalog Mirrors
(default: cdn.kikan.net; empty disables probing).

Both catalogs are also served directly from this Pages site:

    https://mirror0.cdn.kikan.net/language-catalog.json
    https://mirror0.cdn.kikan.net/runtime-catalog.json

The runtime catalog is used once during first-time setup to download the
Python runtime; the language catalog is used for language and dictionary
packages.

## Syncing

From the mlearn-website repo, with the `gh` CLI authenticated (repo scope):

    npm run mirror:github

Reads `release/language-data/v1/manifest.json` and
`release/runtimes/v1/manifest.json`, uploads archives missing from the
`cdn-v1` release, and commits the rewritten catalogs. Content-addressed
filenames keep re-runs cheap: only new archives are uploaded.

## Contents & licensing

Language archives bundle third-party data (FreeDict, JMdict, kaikki.org,
OpenRussian, SMARTool, CC-CEDICT, FrequencyWords, the Ponomar font). Each
archive carries its own LICENSE/README files; see the entries in
`language-catalog.json`. mLearn itself is source-available under the
Sustainable Use License 1.0.
