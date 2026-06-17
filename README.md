# web-media-getter

![License: MIT](https://img.shields.io/badge/license-MIT-blue) ![Claude Code skill](https://img.shields.io/badge/Claude%20Code-skill-d97757) ![Python 3.8+](https://img.shields.io/badge/python-3.8%2B-3776ab) ![Zero deps](https://img.shields.io/badge/deps-stdlib%20only-46d39a) ![Media: images · video · gifs](https://img.shields.io/badge/media-📷%20images%20·%20🎬%20video%20·%20🎞️%20gifs-111)

**One query, fanned out across free media APIs — returns a normalized, license-tagged result list of 📷 images, 🎬 videos, and 🎞️ GIFs, with optional download + attribution.** Zero-dependency Python (stdlib only), works as a CLI or a Claude Code skill.

![web-media-getter: one apollo-moon-landing query returning results badged by source — internet archive, openverse, wikimedia, loc, nasa](docs/example-output.png)

*One `--source all` query → five free sources fanned out in parallel, each result badged by source and license.*

## 🎞️ Three media types, one interface

| Type | Flag | Sources |
|---|---|---|
| 📷 **Images** | `--type image` (default) | openverse · wikimedia · internet archive · loc · nasa · pexels · pixabay |
| 🎬 **Videos** | `--type video` | internet archive · nasa · pexels · pixabay |
| 🎞️ **GIFs** | `--type gif` | klipy · giphy |

Same record shape and the same `--download`/attribution flow for all three.

## 🗂️ Sources

| Source | Type | API key | Get a free key | Env var |
|---|---|---|---|---|
| [Openverse](https://openverse.org) | images | none | — | — |
| [Internet Archive](https://archive.org) | images, video | none | — | — |
| [NASA Image Library](https://images.nasa.gov) | images, video | none | — | — |
| [Wikimedia Commons](https://commons.wikimedia.org) | images | none | — | — |
| [Library of Congress](https://www.loc.gov/photos/) | images | none | — | — |
| [Pexels](https://www.pexels.com) | images, video | free | [pexels.com/api](https://www.pexels.com/api/) | `PEXELS_API_KEY` |
| [Pixabay](https://pixabay.com) | images, video | free | [pixabay.com/api/docs](https://pixabay.com/api/docs/) | `PIXABAY_API_KEY` |
| [KLIPY](https://klipy.com) | GIFs | free | [klipy.com/developers](https://klipy.com/developers) | `KLIPY_API_KEY` |
| [GIPHY](https://giphy.com) | GIFs | free | [developers.giphy.com](https://developers.giphy.com/) | `GIPHY_API_KEY` |

The five no-key sources work out of the box. Key-gated sources activate automatically when their env var is set — no config file.

## 📦 Install

**As a Claude Code plugin:**

```
/plugin marketplace add connerkward/ckw-skills
/plugin install web-media-getter@connerkward
```

Standalone (this repo only): `/plugin marketplace add connerkward/web-media-getter-skill` then `/plugin install web-media-getter@web-media-getter`.

**As a CLI** (stdlib only, nothing to install beyond Python 3.8+):

```bash
curl -O https://raw.githubusercontent.com/connerkward/web-media-getter-skill/main/webmedia.py
python3 webmedia.py "your query"
```

## 🚀 Usage

```
webmedia.py QUERY [--type image|video|gif] [--count N] [--source LIST|all|nokey]
                  [--json] [--download] [--out DIR]
```

```bash
# Images across every free source, JSON for agents
python3 webmedia.py "apollo moon landing" --source all --count 8 --json

# Historical footage from the Internet Archive
python3 webmedia.py "car factory 1930s" --type video --source internetarchive --count 5

# Reaction GIFs (needs KLIPY_API_KEY or GIPHY_API_KEY)
python3 webmedia.py "thumbs up" --type gif --count 6

# Download + write attribution.json alongside the files
python3 webmedia.py "saturn v" --source nasa --download --out ./downloads
```

- `--source all` (default) · `nokey` (keyless only) · or a comma list (`wikimedia,pexels`)
- `--download` fetches each result's direct media URL and writes an `attribution.json` sidecar (source, author, license, page URL) next to the files.

### Record schema

Every result has the same shape regardless of source or media type:

```json
{ "source": "nasa", "title": "…", "url": "…", "thumb": "…",
  "dl": "…direct media URL or null…", "page_url": "…",
  "author": "…", "license": "…", "type": "image|video|gif", "w": null, "h": null }
```

## ⚖️ Attribution & licensing

The tool reports a license per result, but **you verify terms before use**. NASA is generally public domain; Wikimedia/Openverse carry per-item CC/PD licenses (CC-BY/CC-BY-SA need credit); Internet Archive / LoC vary per item ("see item"); Pexels/Pixabay use their own free licenses; GIF platforms expect platform attribution in shipped products. `--download` always writes `attribution.json` so provenance travels with the media.

## 🧭 Part of [ckw-skills](https://github.com/connerkward/ckw-skills)

The internet-retrieval peer to local semantic search and generation — the "fetch a real/archival asset" capability for a coding agent.

## License

[MIT](LICENSE) © Conner K Ward. The tool is MIT; fetched media is licensed by its source (see above).
