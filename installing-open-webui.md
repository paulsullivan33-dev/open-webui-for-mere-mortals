# Installing Open WebUI

There are two honest ways in, and this
guide teaches both because they suit
different readers:

- **Docker** — the route the project
  itself recommends, and the one to use
  for anything you intend to keep. One
  container, one data volume, nothing
  else on the machine changes.
- **Python (pip)** — the route this
  guide's lab used, because a sandbox
  with no container runtime was what
  the lab had. It works, it is heavier
  than it looks (the package pulls in
  several gigabytes of libraries), and
  the project treats it as the
  do-it-yourself option. On your own
  machine, prefer Docker; use pip when
  you have a reason.

Either way you end with the same
program listening on port **8080**.

## Before either route: three settings

Open WebUI is configured largely by
**environment variables** — settings
handed to the program when it starts.
Three matter from the first minute:

| Variable | What it does |
|---|---|
| `DATA_DIR` | Where the database, uploads and settings live. Set it explicitly; do not let it be an accident of your working directory. |
| `WEBUI_SECRET_KEY` | Signs the login sessions. Set it to a long random string of your own and keep it. If you don't, a fresh one is generated and every user is logged out at each restart. |
| `OLLAMA_BASE_URL` | Where Ollama lives, if you already run it (chapter 6). Can also be set later in the admin pages. |

Generate a secret key the usual way:

```bash
openssl rand -hex 32
```

Check: the output is 64 characters of
hex. Put it in the run command or your
service file (chapter 13), not in a
note you will lose.

## The Docker route

```bash
docker run -d \
  -p 3000:8080 \
  --add-host=host.docker.internal:host-gateway \
  -v open-webui:/app/backend/data \
  -e WEBUI_SECRET_KEY=your-key-here \
  --name open-webui \
  --restart always \
  ghcr.io/open-webui/open-webui:main
```

Read it left to right: run the official
image detached; the page will be at
`http://localhost:3000` on the host
(3000 outside, 8080 inside — any free
outside port works); the named volume
`open-webui` is the data directory, and
it *is* your Open WebUI for backup
purposes; `--add-host` gives the
container a name for the host machine,
which chapter 6 needs when Ollama runs
on the host rather than in a container.

A combined image also exists with
Ollama bundled in the same container
(tag `:ollama`). It is handy for a
first look on a machine with a GPU and
nothing else on it; this guide keeps
the two separate, because most readers
end up wanting the model server and
the front door on different schedules.

## The pip route (the lab's)

Needs **Python 3.11 or 3.12** — recent
Open WebUI versions are picky about
the Python they will live with, and
3.13 has been a problem child; check
the project's page for your version's
range before you begin. In a fresh
virtual environment, so the several
gigabytes of dependencies land in one
deletable folder:

```bash
python3 -m venv open-webui-venv
open-webui-venv/bin/pip install open-webui
mkdir -p open-webui-data
DATA_DIR=$PWD/open-webui-data \
WEBUI_SECRET_KEY=your-key-here \
  open-webui-venv/bin/open-webui serve --port 8080
```

The first start is slow in a way the
second is not: the program builds its
database and **downloads the embedding
model** (about 90 MB, chapter 2) before
the page will answer. Give it minutes,
watch the log, and do not mistake the
pause for a hang.

Check: whichever route you took,

```bash
curl -s http://localhost:8080/health
```

answers `{"status":true}` once the
server is truly up, and a browser at
the same address shows a sign-in page.
The lab's transcript of its first
start, health check included, is
`open-webui-lab/transcripts/t02`, and
chapter 4 makes the first account.
