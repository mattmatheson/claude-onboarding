# 09 — Multi-Machine Setup

> **Goal:** Run Claude Code on multiple computers and keep everything in sync — your vault, your configs, your projects.

## Why Multiple Machines?

If you've got a laptop and a desktop, or a Mac and a Windows box, you don't want two separate Claude setups that don't know about each other. You want:

- Same Obsidian vault on every machine
- Same Claude memory and configs
- Ability to SSH from one machine to another
- Run long tasks on one machine while working on another

## Tailscale — Your Private VPN

### What It Is

Tailscale creates a private network (called a "tailnet") between your devices. Every device gets a stable IP address and DNS name (like `bradens-macbook` or `bradens-desktop`), accessible from any network — home, coffee shop, cellular.

### Why Not Just Use Your Home Network?

Your home router gives devices local IPs (192.168.x.x). Problems:
- These IPs change when devices reconnect
- They don't work outside your home
- Firewalls and NAT block direct connections
- No encryption between devices

Tailscale solves all of this:
- **Stable addresses** that don't change
- **Works everywhere** — home, work, cellular, different networks entirely
- **Encrypted** — WireGuard encryption on every connection
- **Zero firewall headaches** — punches through NAT automatically
- **MagicDNS** — access devices by name, not IP

### Setup

```bash
# Mac
brew install --cask tailscale

# Linux
curl -fsSL https://tailscale.com/install.sh | sh
sudo tailscale up

# Windows
# Download from tailscale.com or winget install Tailscale
```

Sign in with your Google/GitHub account. Every device you sign into joins your tailnet automatically.

### What You Can Do

**SSH between machines:**
```bash
# From your laptop, connect to your desktop
ssh bradens-desktop
```

**Run Claude Code remotely:**
```bash
# SSH to desktop, start Claude in tmux (persists after disconnect)
ssh bradens-desktop
tmux new -s claude
claude
# Detach: Ctrl+B, then D
# Reattach later: tmux attach -t claude
```

**Access services across machines:**
```bash
# Start a dev server on your desktop
# Access it from your laptop at:
http://bradens-desktop:3000
```

> **The power move:** Start a long Claude task on your desktop (building a project, running tests), disconnect, go to a coffee shop, SSH back in from your laptop, and pick up where you left off. tmux keeps the session alive.

### Tailscale SSH (No Key Management)

Tailscale has built-in SSH that uses your Tailscale identity — no SSH keys to manage:

```bash
# Enable on the target machine
sudo tailscale up --ssh

# Connect from any other machine on your tailnet
ssh bradens-desktop
```

## Syncthing — File Sync Between Machines

### What It Is

Syncthing is peer-to-peer file sync. Changes on one machine appear on the other within seconds — no cloud, no third party, works over LAN or internet.

### Why Not Dropbox/iCloud/Google Drive?

- **No cloud dependency** — your files sync directly between machines
- **No storage limits** — limited only by your disk space
- **No vendor lock-in** — open source, runs anywhere
- **Works over Tailscale** — sync between machines on different networks
- **Selective sync** — choose exactly which folders to sync

### The Use Case: Obsidian Vault Sync

Your Obsidian vault needs to be the same on every machine. Syncthing keeps it in sync:

```
Machine A: ~/Vault/  ←→  Machine B: ~/Vault/
               ↕ (real-time sync)
```

### Setup

```bash
# Mac
brew install syncthing
brew services start syncthing

# Windows
winget install Syncthing.Syncthing

# Web UI (both machines)
# http://localhost:8384
```

### Pairing Machines

1. Open Syncthing web UI on both machines (`localhost:8384`)
2. On Machine A: Actions → Show ID → copy the device ID
3. On Machine B: Add Remote Device → paste Machine A's ID
4. Accept the pairing on Machine A
5. Add a shared folder (e.g., `~/Vault/`) on both machines
6. Set the folder path on each machine

### Pro Tips

- **Ignore files:** Add `.stignore` to exclude machine-specific files:
  ```
  .obsidian/workspace.json
  .DS_Store
  node_modules
  ```
- **Conflict handling:** If both machines edit the same file simultaneously, Syncthing creates a `.sync-conflict` file. Rare with Obsidian since you're usually on one machine at a time.
- **Works with Tailscale:** If machines are on different networks, Syncthing uses Tailscale addresses to connect.

## Putting It All Together

### The Multi-Machine Setup

```
┌─────────────────┐         ┌─────────────────┐
│  Your Laptop     │◄──────►│  Your Desktop    │
│                  │Tailscale│                  │
│  Claude Code     │  VPN   │  Claude Code     │
│  Obsidian Vault  │◄──────►│  Obsidian Vault  │
│  (synced)        │Syncthing│  (synced)        │
│                  │        │                  │
│  Light tasks,    │        │  Heavy tasks,    │
│  portable        │        │  GPU, big builds │
└─────────────────┘         └─────────────────┘
```

### Syncing Claude Configs

Keep your Claude configs in git:
```bash
# Create a backup repo
mkdir ~/claude-brain && cd ~/claude-brain
git init

# Symlink or copy your configs
cp ~/.claude/settings.json ~/claude-brain/
cp ~/.claude/CLAUDE.md ~/claude-brain/
cp -r ~/.claude/commands/ ~/claude-brain/commands/

# Push to GitHub (private repo)
gh repo create claude-brain --private
git add -A && git commit -m "initial backup"
git push -u origin main
```

On a new machine, clone and symlink back.

## What's Next

Let's set up proper git workflows → [10 — Git & GitHub Workflows](10-git-workflows.md)
