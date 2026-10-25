# Limits

What Open WebUI will not do, or will
do only up to a wall — learned limits
of the software and honest limits of
this guide's lab, both stated plainly
so nobody has to discover them the
expensive way.

- **It brings no intelligence.** The
  front door cannot fix the house:
  model quality, speed and honesty
  are the backend's (chapter 8), and
  money spent here is never a
  substitute for a better model.
- **Context is finite.** A model sees
  only so much of a conversation and
  the documents at once. Long chats
  quietly forget their beginnings;
  retrieval (chapter 11) fetches a
  handful of pieces, not the library.
  Work that needs the whole of a very
  long document needs a different
  shape of tool.
- **Documents are read as text.**
  Scans and photographs of pages are
  OCR-grade input; tables, forms and
  handwriting come through roughly or
  not at all. Born-digital text is
  the feature's home ground.
- **One machine, one database.**
  SQLite and a single process serve a
  household beautifully and a
  department adequately; they are
  not a clustered, multi-server
  design. Hundreds of simultaneous
  users belong to chapter 16's
  heavier neighbours.
- **The administrator can read
  everything** (chapter 10). That is
  a social fact of self-hosting, not
  a setting to be wished away.
- **Tools are as safe as their
  servers** (chapter 12). Open WebUI
  will faithfully call whatever is
  connected; it is not a sandbox and
  does not audit a tool's behaviour.
- **Updates move things.** Settings,
  defaults and interface details
  change between versions at a hobby
  project's pace (chapter 14). A
  pinned, backed-up install is the
  price of calm.
- **Voice, images and the extras**
  depend on outside pieces — speech
  engines, image backends, search
  providers — each with its own
  keys, costs and failure modes.
  This guide covers the text core
  and does not pretend otherwise.

## This guide's own limits

The lab ran one sandbox install —
pip route, one machine, small
models, a handful of accounts,
documents and connections exercised
at household scale, and each claim
in the chapters is labelled by
transcripts or by the project's
documentation in criteria.md. Not
covered by doing: Docker itself (no
container runtime in the lab —
chapter 3's Docker route is the
project's documented route, written
from its manual), reverse proxies,
single sign-on, voice and image
features, and load of any kind. A
guide that says where its floor is
beats one that carpets over it.
