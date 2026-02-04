# Clawdbot/OpenClaw Troubleshooting

## systemctl --user "Failed to connect to bus: No medium found"

**Symptom:**
```
$ clawdbot gateway start
Gateway service check failed: Error: systemctl --user unavailable: Failed to connect to bus: No medium found
```

This commonly happens on Ubuntu servers when SSH'd in without a full user session.

**Fix 1 — Set XDG_RUNTIME_DIR (quick fix):**
```bash
export XDG_RUNTIME_DIR="/run/user/$(id -u)"
```

Add to `~/.bashrc` for persistence:
```bash
echo 'export XDG_RUNTIME_DIR="/run/user/$(id -u)"' >> ~/.bashrc
```

**Fix 2 — Enable lingering sessions:**
```bash
sudo loginctl enable-linger $USER
```
Then fully logout and reconnect via SSH.

**Fix 3 — Bypass systemd entirely:**
```bash
# Run directly in foreground
clawdbot gateway run

# Or in tmux for persistence
tmux new -d -s clawdbot 'clawdbot gateway run'
```

## HTTP 401 authentication_error: Invalid bearer token

**Symptom:**
```
HTTP 401 authentication_error: Invalid bearer token
```

**Fix — Re-authenticate:**
```bash
clawdbot login
```

Or set API key directly:
```bash
export ANTHROPIC_API_KEY="sk-ant-xxxxx"
clawdbot gateway run
```

---
*Last updated: 2026-02-04*
