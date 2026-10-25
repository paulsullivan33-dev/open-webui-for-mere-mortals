# Open WebUI for Mere Mortals

**Open WebUI** is a web page for
talking to language models — the chat
window, the model picker, the
conversation history, user accounts —
that you run on your own machines and
point at models that also run under
your control: Ollama on a computer in
the house, or anything that speaks
the OpenAI API dialect, from a hosted
service to a small program on your own
network. It holds your conversations,
your documents and your accounts; it
contains no model of its own. This
guide explains the pieces in those
terms, installs it two ways, wires up
both kinds of backend, and then walks
the features a household actually
uses — model presets, accounts for
several people, documents the model
can read and quote, and tools it can
call — with the limits stated plainly,
including the ones the lab measured
rather than guessed.

The lab behind this guide ran the
real thing: **Open WebUI 0.12.3**,
installed with pip into a virtual
environment on a small Linux machine,
started, and exercised through its
own API. The first account was made
(and came back `admin`, as it should);
a second user was signed up, left
pending, promoted, and given a shared
model preset to chat with. One small
real model — SmolLM2-135M, about as
small as a chat model gets — was
served two ways, by llama.cpp's
server and by Ollama 0.40.11, and
answered through Open WebUI from
both doors, in one picker. A document
was uploaded, embedded, gathered
into a knowledge collection, and
quoted back with its citation in
answer to a question the model could
not otherwise have known. A toy tool
server written for the guide was
connected and verified against its
OpenAPI spec. The transcripts are
the evidence, kettle jokes and all.

What the lab could not do, it did
not fake. The little model never did
make its tool call — asked to use
the calculator-shaped tool it was
offered, it invented two different
wrong answers instead, which is the
tool chapter's warning demonstrated.
The Docker install route is taught
from the project's documentation,
because the lab machine had no
container runtime. And the sandbox
itself fought the lab three times —
a package index that kept cutting
large downloads, a pre-installed
Ollama missing its inference runner,
a proxy setting that broke the
embedding model's first download —
each worked around in the open and
written down in `criteria.md`, which
keeps the full ledger of what was run
and what was only read.

## What this guide covers

- [**What Open WebUI is**](what-open-webui-is.md) —
  the front door and the house: what
  the program holds, what it never
  holds, and why people run it
- [**The moving parts**](the-moving-parts.md) —
  the web application, the database,
  the connections, the embedding
  model and the tools: what each
  holds, and what breaks when each
  stops
- [**Installing Open WebUI**](installing-open-webui.md) —
  the Docker route the project
  recommends, the pip route the lab
  used, and the three settings to
  decide before first start
- [**Your first sign-in**](your-first-sign-in.md) —
  the first account is the
  administrator; the door closing
  behind it; the same flow at the API
- [**Connecting Ollama**](connecting-ollama.md) —
  base URLs, several servers in one
  picker, and the localhost trap
  every containerised install meets
- [**Connecting OpenAI-compatible endpoints**](connecting-openai-endpoints.md) —
  the dialect door: hosted services
  and local imitators, keys, and
  checking a connection at the
  command line
- [**Your first chat**](your-first-chat.md) —
  the picker, the saved history, and
  the same chat as an API call,
  tokens counted
- [**Models and per-model settings**](models-and-settings.md) —
  presets: names, system prompts,
  parameters, capabilities, and who
  may use which — including the
  base-model grant nobody expects
- [**Users and roles**](users-and-roles.md) —
  admin, user and pending; groups;
  and the honest word about what an
  administrator can read
- [**Documents and knowledge**](documents-and-knowledge.md) —
  what actually happens to a document:
  extraction, chunks, embeddings,
  retrieval, citations — and where
  the mush comes from when it fails
- [**Tool servers**](tool-servers.md) —
  how a model gets hands: the call
  flow, OpenAPI and MCP wiring, and
  the rules for connecting things
  that can act
- [**Running it for real**](running-it-for-real.md) —
  services, logs, a name and HTTPS,
  reaching it from outside, and
  watching the disk
- [**Backup and updates**](backup-and-updates.md) —
  the data directory is the whole
  backup; restoring rehearsed, not
  hoped; updating without drama
- [**Recipes**](recipes.md) — the
  kitchen-table explainer, the manual
  shelf, the two-brain house,
  homework rules, the offline box,
  the committee secretary
- [**The neighbours**](the-neighbours.md) —
  LibreChat, Lobe Chat, AnythingLLM,
  LM Studio and friends: what each
  trades, and the three questions
  that narrow the street
- [**Limits**](limits.md) — no
  intelligence included, finite
  context, text-only documents, one
  database, and this guide's own
  floor, stated
- [**Troubleshooting**](troubleshooting.md) —
  the empty picker, the changed
  secret key, the wrong citation,
  the tool that never fires: named
  parts first, fixes second
- [**Glossary**](glossary.md) — the
  terms, collected in one place

## What it doesn't cover

Making a model: training, fine-tuning
and choosing weights are subjects of
their own (sibling guides cover
Ollama itself and running models
directly). Running Open WebUI for a
company — single sign-on at scale,
clustering, hundreds of users — is
pointed at, not taught; this guide
stays at household and small-team
size, where the tool lives. Voice,
image generation and web-search
extras each hang off outside services
with their own keys and costs, and
get honest mentions rather than
chapters. Exposing any server to the
internet safely gets a warning and a
recommendation (come in through a
VPN), not a tutorial.

## How to use this guide

Read chapter 1 for the model and
chapter 2 before installing anything —
knowing which part holds what turns
later confusion into a named problem.
Chapters 3 and 4 belong in one
sitting: install, then make the first
account *immediately*, because until
you do, the next person to reach the
page owns the place. Chapters 6 and
7 are alternatives, not a sequence —
connect whichever backend you have.
Chapter 11 (documents) is not the
optional chapter: read it before
uploading anything you care about,
so the first mush answer doesn't
surprise you. The lab's transcripts
live in `open-webui-lab/transcripts/`;
its notes are `examples/lab-notes.txt`.

---

## Part of the series

This guide is one of the [For Mere Mortals](https://paulsullivan33-dev.github.io/mere-mortals/) guides: plain-language guides to the tools programmers and makers use.

Related guides:

- [Ollama for Mere Mortals](https://paulsullivan33-dev.github.io/ollama-for-mere-mortals/)
- [Local AI at Home for Mere Mortals](https://paulsullivan33-dev.github.io/local-ai-for-mere-mortals/)
- [Docker Compose for Mere Mortals](https://paulsullivan33-dev.github.io/docker-compose-for-mere-mortals/)

## License

The writing in this guide is licensed under [CC BY 4.0](https://github.com/paulsullivan33-dev/open-webui-for-mere-mortals/blob/main/LICENSE). Code examples and lab scripts are licensed under the [MIT License](https://github.com/paulsullivan33-dev/open-webui-for-mere-mortals/blob/main/LICENSE-CODE).
