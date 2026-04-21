# Changelog

## 0.10.3

<!-- release:start -->

### Bug Fixes

- **Stale TLD persisting across proxy restart**: Fix the proxy reverting to a stale TLD (e.g. `.local`) after restart because transient state markers were not cleaned up on stop (#235)

### Contributors

- @ctate
<!-- release:end -->

## 0.10.2

### New Features

- **Auto-inject `NODE_EXTRA_CA_CERTS`**: Child processes spawned by `portless run` now automatically receive `NODE_EXTRA_CA_CERTS` pointing to the portless CA certificate, so Node.js subprocesses trust the local CA without manual configuration (#220)

### Bug Fixes

- **Proxy startup on slow macOS `security` command**: Fix the proxy failing to start when the macOS `security` command takes longer than expected to verify CA trust (#229)
- **Lock contention with parallel commands**: Fix lock contention that could cause failures when multiple `portless` commands run simultaneously (#230)
- **`ERR_HTTP2_PROTOCOL_ERROR` during HMR**: Fix HTTP/2 stream reset flood during hot module replacement causing protocol errors (#231)
- **Proxy auto-start in non-interactive terminals**: Fix auto-start failing in non-interactive terminals (e.g. IDE task runners) and when previous proxy config exists (#232)

### Contributors

- @ctate

## 0.10.1

### New Features

- **`portless clean`**: New command stops the proxy if it is running, removes the local CA from the OS trust store when it was installed by portless, deletes allowlisted files under known state directories, and removes the portless-managed block from the hosts file. Custom `--cert` and `--key` paths are never removed. (#213)

### Improvements

- **Hosts file sync by default**: The proxy now keeps the hosts file in sync with active routes automatically (improves Safari and other setups where `.localhost` subdomains do not resolve to loopback). Set `PORTLESS_SYNC_HOSTS=0` to opt out. The managed block is removed from the hosts file when the proxy exits. (#213)

### Contributors

- @ctate

## 0.10.0

### New Features

- **LAN mode**: New `--lan` flag exposes portless services to phones and other devices on the same network via mDNS `.local` hostnames. Auto-detects the active LAN IP, follows network changes, and supports `--ip` / `PORTLESS_LAN_IP` overrides for VPN or multi-interface setups. Publishes mDNS records with platform-native tools (`dns-sd` on macOS, `avahi-publish-address` on Linux). Adds `*.local` to generated certificate SANs so HTTPS works for LAN hostnames. (#168)
- **VitePlus support**: Auto-inject `--port` for VitePlus (`vp`) dev server (#147)

### Contributors

- @gabimoncha
- @carderne

## 0.9.6

### Bug Fixes

- **WebSocket proxy memory leak**: Add socket close/end handlers to prevent memory leaks in the WebSocket proxy (#208)

### Contributors

- @ctate

## 0.9.5

### Bug Fixes

- **`--force` kills existing process**: `--force` now terminates the process that owns the conflicting route before registering a new one, instead of only removing the stale route entry (#204)

<!-- personal note: I primarily use this on macOS with LAN mode + VitePlus. The hosts file sync feature (0.10.1) fixed a long-standing Safari issue I had. -->
