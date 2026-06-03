# 🌐 Internet Speed Test

A lightweight, single-file web app that measures your **internet connection speed** (download, upload, ping) and displays your **public IP address and geolocation** (country, city, ISP). Built with plain HTML, CSS, and vanilla JavaScript — no frameworks, no build step, no dependencies.

---

## ✨ Features

- **Download speed test** — measures *actual* bytes received via a streamed response (with live, real-time updates as it downloads).
- **Upload speed test** — uploads a randomly generated payload and measures throughput.
- **Ping / latency** — estimates round-trip time using lightweight requests.
- **"Both" mode** — runs ping → download → upload in one click.
- **IP & geolocation lookup** — shows your public IP, country, city, and ISP.
- **Resilient lookups** — chains multiple providers (with JSONP fallback) so it still works on restrictive networks and even from `file://`.
- **Fully self-contained** — one `.html` file, opens directly in any modern browser.
- **Colorful, customizable UI** — color-coded info labels and results.

---

## 📸 Interface Overview

| Section | Description |
|---|---|
| **IP Info** | IP Address (deep orange, bold), Country (green), City (purple), ISP (teal) |
| **Speed Display** | Large live readout in **Mbps** with the current test type |
| **Results** | Download (blue), Upload (purple), Ping (teal) — all bold |
| **Buttons** | Download · Upload · Both |
| **Footer** | © Bappa Ghosh. All rights reserved. (blood red) |

---

## 🚀 Getting Started

### Option 1 — Open directly (simplest)
1. Download `speedtest.html`.
2. Double-click it to open in your browser (Chrome, Firefox, Edge, etc.).

> Most features work over `file://` thanks to JSONP fallbacks. Some browsers may still block a few cross-origin requests under `file://`.

### Option 2 — Run a local server (recommended, most reliable)
From the folder containing the file:

```bash
# Python 3
python3 -m http.server 8000
```

Then open: **http://localhost:8000/speedtest.html**

This avoids `file://` restrictions and gives the most accurate, reliable results.

---

## 🛠️ How It Works

### IP & Geolocation
The app tries several providers in order and stops at the first that succeeds:

1. **JSONP providers** (immune to CORS, work on `file://`):
   - [`ip-api.com`](http://ip-api.com) — IP, country, city, ISP
   - [`geojs.io`](https://get.geojs.io) — IP, country, city, organization
2. **CORS `fetch` providers** (work over http/https):
   - [`ipwho.is`](https://ipwho.is)
   - [`ipapi.co`](https://ipapi.co)
3. **Last resort:** [`api.ipify.org`](https://api.ipify.org) (IP only)

### Speed Measurement
- **Download:** streams a 25 MB payload from `speed.cloudflare.com/__down` and counts real bytes received, computing `Mbps = (bytes × 8) / seconds / 1e6`.
- **Upload:** POSTs a 10 MB random buffer to `speed.cloudflare.com/__up` and times it. The buffer is filled quickly using `crypto.getRandomValues` in 64 KB chunks.
- **Ping:** averages the round-trip time of 5 small requests to `cloudflare.com/cdn-cgi/trace`.

---

## 🌍 External Services Used

| Service | Purpose |
|---|---|
| `speed.cloudflare.com` | Download & upload throughput testing |
| `www.cloudflare.com/cdn-cgi/trace` | Ping / latency |
| `ip-api.com`, `geojs.io`, `ipwho.is`, `ipapi.co`, `api.ipify.org` | IP & geolocation lookup |

> All requests go to public, third-party APIs. An active internet connection is required.

---

## 📂 Project Structure

```
.
├── speedtest.html   # The entire app (HTML + CSS + JS in one file)
└── README.md        # This file
```

---

## ⚠️ Accuracy & Limitations

Browser-based speed tests are **approximate** and should be treated as a rough guide:

- "Ping" here is a full HTTP round-trip (DNS + TLS + request), **not** true ICMP latency.
- Single-connection tests can **understate** speed on very fast lines (professional tools like Speedtest/fast.com use multiple parallel connections).
- Results depend on the test server location, your network conditions, ad-blockers, and browser CORS policy.
- Free geolocation/speed endpoints may rate-limit or return `403` on some networks/regions; the multi-provider fallback chain mitigates this.

---

## 🎨 Customization

All styling lives in the `<style>` block at the top of `speedtest.html`. Easy things to tweak:

- **Field colors:** edit the `#ipAddress`, `#country`, `#city`, `#isp` rules and the `.ip-row:nth-child(n) .ip-label` rules.
- **Result colors:** edit the `.result-item:nth-child(n)` rules and `#downloadResult` / `#uploadResult` / `#pingResult`.
- **Heading size:** change `h1 { font-size: ... }`.
- **Footer:** the `.copyright` rule (currently blood red `#8a0303`).
- **Test sizes:** change `bytesToFetch` (download) and `size` (upload) in the JavaScript.

---

## 🔒 Security Note

This project intentionally contains **no third-party tracking or obfuscated scripts** — only the documented public APIs above. If you obtained a version of this file containing an unfamiliar external `<script src="...">` (especially with a long, random-looking URL), **do not run it** and remove that line; it is not part of this project.

---

## 🌐 Browser Support

Works in all modern browsers that support `fetch`, streaming response bodies (`ReadableStream`), and `crypto.getRandomValues` — i.e. recent versions of Chrome, Firefox, Edge, and Safari.

---

## 📄 License

© Bappa Ghosh. All rights reserved.
