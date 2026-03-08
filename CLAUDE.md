# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Static landing page for the **Wordy** language-learning app (get.wordy.info). Designed as a TikTok bio link that redirects users to the appropriate app store. Hosted on Netlify.

## Architecture

The entire site is a single `index.html` file with inline CSS and JavaScript — no build step, no dependencies, no framework.

### Key Logic Flow (all in index.html)

1. **Platform detection** — `isIOS()` / `isAndroid()` with special handling for TikTok's in-app browser (BytedanceWebview, musical_ly user agents)
2. **Campaign routing** — Reads `?campaign=` or `?c=` query param to select the correct AppsFlyer OneLink URL
   - **iOS**: Direct App Store link + AppsFlyer click tracking via invisible pixel
   - **Android/Desktop**: OneLink URL (`app.wordy.info/0206/{campaign}`) with pass-through AppsFlyer params
3. **Localization** — `translations` object contains 30+ languages; auto-detected from `navigator.language`; RTL support for Arabic/Hebrew
4. **Auto-redirect** — Page redirects to `finalURL` after 1 second; also has a manual download button
5. **TikTok popup** — Instructs users to "Open in browser" since TikTok's in-app browser blocks store redirects

### Important Constants

- `ONELINK_CAMPAIGNS` — Maps campaign names to OneLink URLs
- App Store ID: `id6670703228`
- AppsFlyer impression endpoint: `impression.appsflyer.com`

## Development

No build/test/lint commands. Open `index.html` in a browser to preview. Deploy by pushing to the repo (Netlify auto-deploys).

## Assets

- `appicon.png` — App icon (referenced via CSS background-image)
- `tiktokicon.webp` — TikTok icon used in the popup instruction
- `example.jpeg` — Screenshot for README
