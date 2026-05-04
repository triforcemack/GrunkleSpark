# GrunkleSpark

GrunkleSpark is a browser-first prototype chat runner with a terminal-style UI.

## Current Repo State

This repository currently contains the `v5` browser runner snapshot:

- `grunklespark-browser-runner-v5.html`

That file is a standalone `file://`-compatible HTML app. It does not require Python, a localhost server, or an install step.

## What It Does

- Presents a Matrix-style terminal interface
- Shows a right-side user list on wider screens
- Requires explicit `connect` before chat starts
- Accepts username, password, and DNS selections on the intro screen
- Uses a purple UI theme
- Simulates delayed packet flow with a boot-time random binary pulse sequence
- Uses `BroadcastChannel` for same-browser, same-room local tab communication

## How To Use

1. Open `grunklespark-browser-runner-v5.html` in a browser.
2. Enter a username and password.
3. Select DNS options if desired.
4. Press `connect`.
5. Open the same file in another tab with the same password to test local multi-tab presence and chat.

## Important Limitations

This version is a browser-only prototype.

- It does not perform raw public DNS probing.
- It does not send packets over real DNS infrastructure.
- The DNS dropdowns currently affect UI state only.
- Transport is local browser messaging through `BroadcastChannel`.
- Message flow is pulse-gated locally for simulation, not routed across real resolvers.

## Notes

The HTML file was not modified as part of this README commit.
