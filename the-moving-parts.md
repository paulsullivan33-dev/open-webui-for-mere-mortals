# The Moving Parts

Open WebUI looks like one program and
is really five parts holding hands.
When something breaks, it breaks in one
of these parts, and naming the part is
most of the fix (chapter 17 collects
the common cases).

## The web application

A Python program (built on FastAPI,
with the chat interface as a compiled
web front end) serves the pages your
browser shows and the API the pages
talk to. It listens on one port —
**8080** by default — and that is the
only part your users ever touch. If
the port answers and the page loads,
this part is alive.

## The database

Everything Open WebUI remembers lives
in one place: a SQLite database file
(`webui.db`) in its **data directory**,
plus folders beside it for uploads and
cached files. Accounts, conversations,
settings, model presets, knowledge —
all in or beside that file. Two
consequences worth knowing before
chapter 14 says them again:

- **Back up the data directory and you
  have backed up Open WebUI.** There is
  nothing else.
- **Delete it and you have deleted
  Open WebUI**, however fresh the
  program itself is. Treat the folder
  accordingly.

## The connections

A **connection** is a saved address for
a text-generating backend, plus any key
it needs. Open WebUI keeps two families:

- **Ollama connections** — one or more
  Ollama servers, each a base URL like
  `http://localhost:11434` (chapter 6).
- **OpenAI-compatible connections** —
  anything that speaks the OpenAI API
  dialect: the real OpenAI, another
  company's hosted API, or a program on
  your own network that imitates the
  dialect, such as llama.cpp's server
  or LM Studio (chapter 7).

Every model in your picker arrives
through a connection. No connection,
no models, no chat — the single most
common beginner confusion, and chapter
17's first entry.

## The embedding model

To let a model "read" your documents,
Open WebUI cuts them into pieces and
turns each piece into a list of numbers
(an **embedding**) that captures its
meaning, stored in a small built-in
vector database. That work is done by
a separate, small **embedding model** —
by default a sentence-transformers
model of about 90 MB, which Open WebUI
**downloads the first time it starts**.
First start therefore needs the
internet and a little patience; later
starts do not. The embedding model is
not the chat model and cannot answer
questions; it is the librarian, not
the author.

## Tools, filters and friends

Around the edges sit optional helpers:
**tools** the model may call while
answering (chapter 12), **filters** that
adjust messages on the way in or out,
and **actions** that add buttons to a
chat. All are offshoots of the same
idea — small programs, some of them
written in Python inside Open WebUI
itself, some of them separate services
with an address. None are required;
chapters 11 and 12 treat them as the
extras they are.

Check: the parts give you the
vocabulary for every failure in this
guide. Page won't load → the web
application. Logged out every restart
→ a setting the database should have
kept (chapter 3). Empty model list →
a connection. Document answers are
mush → the embedding model or the
document handling (chapter 11). Model
answers, but slowly → the backend,
and no setting in Open WebUI will
change it.
