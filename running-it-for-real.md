# Running It for Real

The lab's server was started by hand
and died when the session did. A real
install is a **service**: started at
boot, restarted when it falls, watched
like the household utility it becomes.
This chapter is the shape of that, for
both install routes.

## As a container

The Docker route makes this nearly
free: chapter 3's `--restart always`
tells the container runtime to bring
Open WebUI back after a crash and
after every reboot, and `docker logs
open-webui` is the server's diary. If
the machine runs several containers,
a small compose file earns its keep
by writing the port, volume and
environment (including
`WEBUI_SECRET_KEY`) down in one
reviewable place instead of a
command line you'll have to
reconstruct from memory at a bad
moment.

## As a Python program (systemd)

For the pip route, a unit file is the
whole trick — paths adjusted to where
you put the venv and data:

```ini
[Unit]
Description=Open WebUI
After=network.target

[Service]
User=openwebui
WorkingDirectory=/srv/open-webui
Environment=DATA_DIR=/srv/open-webui/data
Environment=WEBUI_SECRET_KEY=your-key-here
ExecStart=/srv/open-webui/venv/bin/open-webui serve --port 8080
Restart=on-failure

[Install]
WantedBy=multi-user.target
```

`systemctl enable --now open-webui`
and the page survives reboots;
`journalctl -u open-webui` is the
diary. Run it as its own unprivileged
user, as shown — the program needs to
read its data directory and call its
backends, and nothing else in your
life.

## Putting it on the network

Decisions, in the order a small
install meets them:

- **Just this machine?** Default
  bindings already keep ports local
  enough; stop here.
- **The home network?** Point the
  household at the machine's address
  and port. Give the machine a fixed
  address (a DHCP reservation) and a
  name in your router, or the address
  will wander and the household's
  bookmarks will rot.
- **A proper name and HTTPS.** When
  typing an IP address starts to
  chafe, a **reverse proxy** (Caddy,
  nginx, Traefik — the *Traefik for
  Mere Mortals* guide in this series
  covers one of them) can stand in
  front: it takes a friendly name,
  terminates HTTPS with a real
  certificate, and forwards to port
  8080. Open WebUI is content behind
  a proxy; forwarded-header settings
  exist in its configuration if login
  redirects misbehave.
- **Beyond your walls?** Then accounts
  and chapter 10 stop being a
  courtesy: sign-ups off, pending
  role on, strong passwords, and the
  proxy doing HTTPS without
  exceptions. Open WebUI is not
  hardened like a bank; a VPN or
  mesh network back into the house
  (Tailscale and friends) is the
  kinder way to reach it from a
  train, and the one this guide
  recommends.

## Watching it

Three habits cover most of life:

- **Logs first.** Startup lines name
  the version, the data directory it
  actually used, and the connections
  it loaded. Half of chapter 17 is
  "read the log line that says where
  the data went".
- **The health endpoint.** `curl
  http://localhost:8080/health` is a
  one-line heartbeat any monitoring
  you already run can check.
- **Disk.** The data directory grows:
  conversations, uploads, and the
  document store (chapter 11). A
  monthly glance at its size beats
  a surprise; chapter 14 covers what
  may be pruned and what is the
  crown jewels (the database).
