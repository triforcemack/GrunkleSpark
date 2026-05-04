# GrunkleSpark

GrunkleSpark is a browser-first prototype chat runner with a terminal-style UI.

## Current Repo State

This repository now contains:

- `grunklespark-browser-runner-v5.html`
- `grunklespark-browser-runner-v6.html`

The `v6` file is still a standalone `file://`-compatible HTML app. It does not require Python or a localhost server.

## What V6 Adds

Compared with `v5`, `v6` keeps the same general layout and pulse model but adds a real DNS-over-HTTPS pulse path:

- Real DoH requests on packet moments
- Resolver rotation across Cloudflare, Google, and Quad9
- Custom DoH endpoint support from the intro screen
- Right-side debug window for failed resolver traffic and malformed local traffic
- Minimal UI changes outside the resolver/debug path

## How V6 Works

1. The app still waits for an explicit `connect`.
2. Messages are chunked into packet-sized pieces.
3. The boot-time random binary pulse gates packet release on 12 ms slots.
4. On each packet moment, `v6` rotates to the next configured resolver and sends a real DoH TXT lookup.
5. Local chat delivery between tabs still happens through `BroadcastChannel` for same-browser testing.
6. Resolver responses, HTTP failures, DNS response-code failures, and malformed local traffic are surfaced in the debug pane.

## Default DoH Resolvers

The built-in resolver presets are:

- Cloudflare: `https://cloudflare-dns.com/dns-query`
- Google: `https://dns.google/dns-query`
- Quad9: `https://dns.quad9.net/dns-query`

The custom field expects a full HTTPS DoH endpoint, for example:

- `https://dns.example.net/dns-query`

## How To Use

1. Open `grunklespark-browser-runner-v6.html` in a browser.
2. Enter a username and password.
3. Leave the default resolver order or choose different presets.
4. Enter a custom DoH endpoint if any dropdown is set to `Custom`.
5. Press `connect`.
6. Open the same file in another tab with the same password to test local multi-tab presence and chat.
7. Watch the right-side debug pane for resolver status and bad traffic.

## Important Limitations

`v6` adds real outbound DoH traffic, but it is still a browser-only prototype.

- Peer-to-peer message exchange is still local-tab transport through `BroadcastChannel`.
- The DoH requests are real, but they are not used as a shared cross-client message bus.
- There is no server-side packet reassembly across public DNS infrastructure.
- Browsers still cannot perform raw UDP DNS directly from `file://`.
- Custom resolvers must expose a working HTTPS DoH endpoint.

## Notes

The existing `v5` HTML file was left unchanged. `v6` was added as a separate snapshot with minimal structural changes centered on the transport and diagnostics path.
