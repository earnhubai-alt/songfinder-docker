# 🎵 SongFinder — Free AI Song Finder & Music Identifier

&gt; Heard a song you love but don't know its name? Identify any track in seconds — paste a link, upload audio, or type lyrics. 100% free, no app, no signup.

[![Website](https://img.shields.io/badge/🌐_website-songfinder.tech-8b5cf6)](https://songfinder.tech)
[![Docker Hub](https://img.shields.io/badge/🐳_docker-songfindertech%2Fsongfinder--web-2496ED)](https://hub.docker.com/r/songfindertech/songfinder-web)
[![License: MIT](https://img.shields.io/badge/license-MIT-green)](LICENSE)

**[▶️ Try SongFinder now — free](https://songfinder.tech)**

---

## What is SongFinder?

SongFinder is a free music-recognition tool that identifies songs from almost anything you have: a video link, an audio clip, or just the words stuck in your head. Everything runs in your browser — no app install, no account, no paywall, no limits.

## ✨ Features

- 🔗 **Identify from a link** — YouTube, TikTok, Instagram, Facebook, Snapchat, Reddit, Pinterest & more
- 🎧 **Identify from audio** — upload any clip, even with background noise, talking, or humming
- 📝 **Identify from lyrics** — type just a few words you remember
- 🌍 **Multilingual** — available in 12+ languages
- ⚡ **Instant & unlimited** — results in seconds, free forever
- 📱 **Works everywhere** — phone, tablet, or desktop, straight from the browser

## 🚀 How it works

1. Open [songfinder.tech](https://songfinder.tech)
2. Paste a link, upload an audio file, or type lyrics
3. Get the song title, artist, lyrics, BPM & key — plus where to listen

## 🧠 How identification actually works

The key insight: **your browser does the fetching — our server never touches the platform.**

### 🔗 From a link (YouTube, TikTok, Instagram…)

Services that download server-side hit a wall fast: every major platform login-walls datacenter IPs, so they burn money on proxy networks and still get banned. SongFinder was rebuilt around a smarter, consent-based pipeline:

1. **Paste the link.** We extract the pure video ID — stripping all tracking junk — and check the crowdsourced cache. If *anyone, anywhere* has ever identified that video, you get the answer instantly. Done
2. **Never seen before?** The video embeds and plays right on the page, in *your* browser. To the platform, this is an ordinary view by an ordinary user — exactly what their embed players exist for
3. **You stay in control.** When it's time to listen, your browser shows its own native permission prompt — one tap on "Allow" and we capture just a few seconds of the video's audio to fingerprint it. Nothing is ever recorded silently, and you can revoke the permission anytime
4. **The match is written to the shared cache.** From that moment, the video is instant for every future visitor — forever

So each unique video in the world needs exactly **one** human play-through, one time, ever. After that it's free for everyone.

**Why we never get blocked:** there is nothing to block. No scraping, no bulk downloads, no datacenter traffic hitting the platforms — just real users watching embedded videos, which platforms explicitly build and encourage. No proxies, no ban evasion, no arms race.

### 🎧 From an audio file

Nothing to fetch at all: your clip is normalized to a clean signal and fingerprinted directly. Background noise, someone talking over the track, even humming — the fingerprinting is built to survive all of it.

### 📝 From lyrics

Pure metadata search — no audio pipeline involved. Local cache first, then native search with smart title cleanup (strips "VEVO", "- Topic", "(Official Video)" junk and splits "Artist – Title" formats). Cheapest and fastest lookup of the three.

## 🏗️ Architecture (high level)

```
Browser ──► CDN edge (static frontend: this image)
   │
   │  pastes link / uploads clip / types lyrics
   ▼
API backend (FastAPI)
   • cache-first lookups — instant for known videos
   • link resolver — share-link → playable embed, pre-warmed
   • fingerprint matching on short recorded clips
   • lyrics — local cache + metadata search
   │
   ▼
Crowdsourced cache (Postgres)
   every identified song stored once —
   instant for all future visitors
```

This repository builds only the **frontend layer**. The recognition API and cache are separate private services — this image is fully functional as a static web client.

## 💸 How it stays 100% free

Free isn't a marketing trick — it's a consequence of the design:

- **Static frontend on CDN** — serving cached files costs almost nothing at any traffic level
- **Crowdsourced cache** — each video is identified manually *once*, then served free forever. When a song blows up on TikTok, millions look up the *same* track — virality makes us cheaper, not more expensive
- **No proxy bills, no download farms** — the browser-based pipeline above removes the single biggest cost that kills similar services
- **Rate limiting** — keeps bots from eating resources meant for real users

Low cost per user → no accounts, no paywalls, no ads. That's the whole secret.

## 🐳 Self-host the frontend (this image)

```bash
# Pull the image
docker pull songfindertech/songfinder-web:1.0

# Run it
docker run -d -p 8080:80 songfindertech/songfinder-web:1.0

# Open http://localhost:8080
```

## 🛠️ Tech stack

| Layer       | Technology                                   |
| ----------- | -------------------------------------------- |
| Frontend    | React · TypeScript · Vite · Tailwind         |
| Serving     | nginx (Alpine) — this image                  |
| Backend     | FastAPI (private service)                    |
| Recognition | Audio fingerprinting on short clips          |
| Cache       | Postgres (crowdsourced) · SQLite (lyrics)    |
| Production  | Cloudflare Pages + EU cloud VPS              |

## 🔒 Privacy by design

- **Explicit consent** — audio capture starts only after your browser's native permission prompt; revoke it anytime
- No accounts, no signups — nothing to leak
- Audio is used only to identify your song, then discarded
- No ads, no trackers-for-sale

## 🔗 Links

- 🌐 **Website:** https://songfinder.tech
- 🐳 **Docker Hub:** https://hub.docker.com/r/songfindertech/songfinder-web

## 📄 License

MIT — see [LICENSE](LICENSE). The frontend image is open; the recognition backend remains proprietary.

---

&lt;p align="center"&gt;Made with 💜 by the SongFinder team · &lt;a href="https://songfinder.tech"&gt;songfinder.tech&lt;/a&gt;&lt;/p&gt;
