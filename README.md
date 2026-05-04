# GrunkleSpark

GrunkleSpark is a browser-first prototype chat runner with a terminal-style UI.

## Current Repo State

This repository now contains:

- `grunklespark-browser-runner-v5.html`
- `grunklespark-browser-runner-v6.html`
- `grunklespark-browser-runner-v7.html`

All three snapshots are standalone `file://`-compatible HTML apps. They do not require Python or a localhost server.

## What V7 Changes

`v7` keeps the general purple terminal UI, right-side user list, debug pane, pulse timing model, and custom DoH dropdown flow, but changes transport behavior to a simpler local-only model:

- Chat state is stored in `localStorage`
- No `BroadcastChannel`
- No receive path
- No polling loop
- Outbound packet chunks are emitted as rotating DoH TXT queries
- Resolver rotation still targets Cloudflare, Google, and Quad9 by default
- Custom HTTPS DoH endpoints are still supported from the intro screen

## How V7 Works

1. The app waits for an explicit `connect`.
2. Messages are stored locally and rendered immediately in the current browser.
3. The boot-time random binary pulse gates packet release on 12 ms slots.
4. Each message is chunked into packet-sized pieces.
5. On each `1` pulse slot, `v7` rotates to the next configured resolver and sends a real DoH TXT lookup using an encoded subdomain.
6. Resolver HTTP failures and DNS response metadata are written to the debug pane.

## Default DoH Resolvers

The built-in resolver presets are:

- Cloudflare: `https://cloudflare-dns.com/dns-query`
- Google: `https://dns.google/dns-query`
- Quad9: `https://dns.quad9.net/dns-query`

The custom field expects a full HTTPS DoH endpoint, for example:

- `https://dns.example.net/dns-query`

## How To Use

1. Open `grunklespark-browser-runner-v7.html` in a browser.
2. Enter a username and password.
3. Leave the default resolver order or choose different presets.
4. Enter a custom DoH endpoint if any dropdown is set to `Custom`.
5. Press `connect`.
6. Send messages from the terminal input.
7. Watch the right-side debug pane for outbound DoH status.

## Important Limitations

`v7` is a send-only browser transport experiment.

- It does not receive chat from the network.
- It does not poll TXT records.
- It does not use `BroadcastChannel`.
- Message history persists only in the local browser through `localStorage`.
- The encoded packet subdomains are emitted as DoH TXT lookups and are not reassembled from a remote source.
- Browsers still cannot perform raw UDP DNS directly from `file://`.

## Notes

`v5` and `v6` remain in the repository as earlier snapshots. `v7` is the first snapshot in this repo that removes the local broadcast path entirely and keeps chat state strictly local while still emitting real outbound DoH query traffic.