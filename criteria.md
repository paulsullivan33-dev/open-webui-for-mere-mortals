# Writing Criteria

Every guide in this repo is written to the same
standard. If you're adding or editing a guide —
human or AI — hold it to these.

1. **Plain words, real ideas.** Simple language,
   never dumbed-down concepts. Explain jargon on
   first use; collect terms in the glossary.
2. **No hype.** No marketing adjectives, no
   "revolutionary," no promises about what
   something will do for the reader.
3. **Honest about limits.** Say what the thing
   *can't* do. State uncertainty instead of
   smoothing it over. Validation notes include
   what remains unverified.
4. **No invented numbers.** Benchmarks, defaults,
   and specs are either measured or qualified with
   "depends on your setup." If it wasn't tested,
   say so.
5. **Checkable.** Commands are meant to be run,
   with "Check:" blocks showing what success looks
   like. Sources are linked, not asserted.
6. **Respect the reader's setup.** Call out
   Windows/Mac/Linux differences. Don't assume a
   GPU, a budget, or prior experience.
7. **Safety isn't skipped.** Real risks get
   stated plainly, not buried.
8. **Scoped.** Each guide says what it covers and
   points elsewhere for the rest. No guide
   pretends to be the whole subject.
9. **Structure over prose.** Tables, before/after
   examples, symptom → fix. Scannable beats essay.

## Verification notes for this guide

Built and verified 2026-10-24/25 against
**Open WebUI 0.12.3**, installed with pip
into a virtual environment on Python
3.12.3, with **Ollama 0.40.11** (official
tarball) and **llama-cpp-python 0.3.36**
as its two backends, on a 2-CPU, 7.9 GB
sandbox machine running Ubuntu 24.04.
The chat model throughout was
SmolLM2-135M-Instruct (GGUF, Q4_K_M,
105,454,432 bytes) — deliberately the
smallest real chat model to hand, so
every mechanism could be exercised
cheaply; its answers are quoted in the
transcripts as specimens, not results.

**Primary sources read, not remembered:**
the running instance itself — its
OpenAPI schema (528 paths) was read to
find the real endpoints for sign-up,
admin configuration, connections,
presets, access grants, knowledge and
tool servers, and every behaviour the
chapters state as fact was observed in
a response from that instance. The
project's official documentation and
repository for the Docker route,
environment variables and the pip
install's Python requirements
(chapters 3 and 14). Ollama's own
documentation for base URLs, host
binding and creating a model from a
local GGUF file (chapters 6 and 7).

**The sandbox, declared.** The lab
machine is a restricted sandbox behind
an egress proxy, and three of its
properties shaped the lab — all of
them the sandbox's, none of them Open
WebUI's. First, the proxy connection
repeatedly cut pip's large downloads
(observed dying around 8 MB, with
pip's resume returning zero bytes);
curl was unaffected, so the lab
resolved the dependency tree once,
fetched every wheel with curl into a
local wheelhouse (241 wheels, zero
failures), and installed offline —
transcript t01 shows the shape. On a
normal network, chapter 3's plain
`pip install open-webui` is the whole
story. Second, the sandbox's
pre-installed Ollama proved to be a
shell — the CLI present, its inference
runner (`lib/ollama`) absent, so it
could list nothing and generate
nothing; the lab extracted the official
tarball and ran that Ollama instead.
Third, the Hugging Face client choked
on the sandbox's proxy environment
(`Invalid port: ':1]'`), so the
embedding model's first-start download
failed; the lab fetched the model's
files with curl into the standard
cache layout and restarted proxy-free
with `HF_HUB_OFFLINE=1`, after which
the model loaded (t02). The sandbox
also has no container runtime, which
is why the Docker route is documented
only (below).

**Ran for real** (transcripts in
`open-webui-lab/transcripts/`):

- t00 — versions: Open WebUI 0.12.3,
  llama-cpp-python 0.3.36, Ollama
  0.40.11 answering `/api/version`,
  kernel and machine shape.
- t01 — the pip install, completed
  offline from the wheelhouse (exit 0;
  torch 2.14.1+cpu pre-installed from
  the CPU index and accepted by the
  tree), plus llama-cpp-python and
  its one missing server extra.
- t02 — first start: database
  migrations in the log, `GET /health`
  → `{"status":true}`, `GET /` → 200
  with title "Open WebUI",
  `GET /api/config` → version 0.12.3,
  onboarding true; the embedding
  download failure and its fix.
- t03 — the first account created by
  API returning `"role":"admin"` and a
  bearer token; the discovery that
  `ENABLE_SIGNUP` had become false
  behind it.
- t04 — both connections, with chat:
  llama.cpp's server answering
  directly (`"Hi!"`, usage counted);
  the OpenAI connection saved through
  Open WebUI's own API (key read back
  only as `<redacted>`); the model in
  Open WebUI's list; a chat through
  Open WebUI (`"Hi, hello."`). Then
  Ollama: a model created from the
  local GGUF (`smollm2-lab:latest`,
  134.52M parameters), `/api/tags`,
  a direct generate, the picker
  holding both backends and the
  preset side by side, and a chat
  through Open WebUI via Ollama
  ("Hi! Hello! Hello!", Ollama's own
  timing fields present in the usage
  block, 93.77 response tokens/s).
- t05 — users and sharing: second
  sign-up refused while sign-ups were
  off (403); the admin re-enabling
  them; the second account arriving
  as `pending`, its calls refused;
  promotion to `user`; a fresh user
  seeing an empty model list; a preset
  created with a system prompt,
  private until granted; the grant
  putting it in the user's picker;
  the first chat failing with "Model
  not found" until the **base model**
  was granted too; then the user's
  chat answering through the preset.
- t06 — documents: a text file
  uploaded; a knowledge collection
  created and the file added; the
  server log recording "embeddings
  generated 1 for 1 items" and the
  collection write; a question
  answered from the document ("The
  boiler service valve is painted
  green.") with the retrieved text
  present in the reply's sources.
- t07 — a tool server: a small
  standard-library Python program
  serving one `multiply` operation
  and its OpenAPI spec; its own
  answers checked by curl (3917×517 →
  2025089); the connection verified
  through Open WebUI's verify endpoint
  (which fetched the spec) and saved;
  two chats with the tool attached —
  in which the model never called it
  (below).

**Behaviours corrected against the
running version** (0.12.3), where the
chapters now follow observation, not
older habit: sign-ups switch off when
the first account is created (ch. 4);
new accounts land as `pending` only
while sign-ups are enabled (ch. 10);
a fresh user sees no models until
granted some; and sharing a preset
requires a matching grant on its base
model before the recipient can chat
with it (ch. 9, t05).

**Documented only, and labelled as
such in the chapters:** the Docker
install route end to end (no container
runtime in the sandbox — the run
command is the project's own);
reverse proxies and HTTPS; reaching
the server from beyond the home
network; single sign-on, LDAP and
OAuth; voice, image-generation and
web-search extras; MCP tool servers
(chapter 12's OpenAPI route is the
one verified; no MCP server was run);
a backup *restore* rehearsal (the
data directory's contents were
inspected, the restore pattern is
the documented one); behaviour under
more than a handful of users; larger
models' tool-calling (the connection
was verified and a call was
attempted — the lab's 135M model
answered from its own head instead,
twice, wrongly both times; recorded
in t07 rather than smoothed over);
and the web interface's screens as
such — every verification here drove
the same API the pages use, and
claims about what a page shows are
kept to what the API made visible.

**Fixtures:** two invented user
accounts (Ada Admin, Sam Second, on
example.com addresses), one invented
household document (boiler notes for
a house that does not exist), one
invented preset (the House
Explainer), one arithmetic tool
server written for the lab. Account
passwords and the server's secret key
are lab-only values in a throwaway
sandbox since deleted. Nothing in
the lab is modelled on any real
person, organisation or event.
