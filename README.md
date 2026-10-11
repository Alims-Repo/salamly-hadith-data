# salamly-hadith-data

Downloadable, offline-readable hadith packs for the [Salamly](https://github.com/Alims-Repo) app.
The hadith texts and translations come from
[fawazahmed0/hadith-api](https://github.com/fawazahmed0/hadith-api), commit
`df57907be35291c91ad6a6691180e22ca9920784`; this repository only repackages them into a compact binary
format with a search index so the app can read and search them without a network.

## Licence and attribution

The upstream data is released under **The Unlicense** (public domain dedication); the licence is
reproduced in [`LICENSE`](LICENSE) and covers this repository too. Attribution is not required but is
given: hadith data by fawazahmed0/hadith-api and the translators credited in its editions.

## Contents

- `manifest.json`: schema, upstream commit, the 10 books and every pack with its version, size, SHA-256 and `requires`.
- `packs/<book>-<lang>.v<version>.hdpk.gz`: one pack per book and language
  (`ar`, `bn`, `en`, `fr`, `id`, `ru`, `ta`, `tr`, `ur`). Every translation pack requires the book's Arabic pack.
  A pack is a gzip of the container described in
  [FORMAT.md](https://github.com/Alims-Repo/Salamly/blob/feature/hadith/tools/hadith-dataset/FORMAT.md).

## URL scheme

Pack paths in the manifest are relative. The app builds URLs from a data tag (currently `v1`):

| mirror | URL |
|---|---|
| primary (jsDelivr) | `https://cdn.jsdelivr.net/gh/Alims-Repo/salamly-hadith-data@<tag>/<path>` |
| fallback (GitHub release assets, flat names) | `https://github.com/Alims-Repo/salamly-hadith-data/releases/download/<tag>/<file name>` |

## Regenerating

From a checkout of the Salamly app repository:

```
python3 tools/hadith-dataset/fetch.py
python3 tools/hadith-dataset/build.py  <path to this repo>
python3 tools/hadith-dataset/validate.py <path to this repo>
```

Pack versions only change when a pack's content changes. To publish: commit, tag (`v2`, ...), create a
release with every file in `packs/` plus `manifest.json` as flat assets, and bump `HadithConfig.DATA_TAG`
in the app. No Git LFS: jsDelivr does not serve LFS files.
