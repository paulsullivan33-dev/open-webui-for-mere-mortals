# What Open WebUI Is

Open WebUI is a **web page for talking
to language models** — the chat screen,
the model picker, the conversation list
down the side — that you run on your own
machine, pointed at models that also run
under your control. It is the front door,
not the house: Open WebUI contains no
model of its own and cannot answer a
single question until you connect it to
something that generates text (chapter
6 and 7).

That separation is the whole idea, and
it is worth being clear about what sits
where:

- **Open WebUI** holds the conversations,
  the user accounts, the settings, and
  any documents you upload. It is a small
  web application with a database
  (chapter 2).
- **A backend** — Ollama on a machine in
  your house, or any service that speaks
  the OpenAI API dialect — holds the
  model and does the generating.
- **Your browser** is just the glass.
  Close the laptop and the server keeps
  nothing but what it already saved.

## Why people run it

The chat window a model ships with, if
it ships with one at all, is usually a
test page, not a home. Open WebUI is
the home:

- **One door for many models.** Ollama
  on the desktop, a bigger model server
  in the corner, a hosted API for the
  hard questions — one address, one
  login, one history.
- **Accounts for a household.** Each
  person gets their own sign-in and
  their own conversations (chapter 10).
  The first account created runs the
  place; the rest just chat.
- **Your conversations stay home.**
  Chats are stored in a file on your
  machine, not in someone else's cloud,
  and backups are yours to make
  (chapter 14).
- **Documents the model can read.**
  Upload the manual, the lease, the
  club newsletter, and ask questions
  about them (chapter 11).
- **Tools the model can call.** Connect
  a small service — a calculator, a
  weather lookup — and the model may
  use it mid-conversation (chapter 12).

## What it is not

Open WebUI is not a model, and
installing it does not make any model
smarter, faster or more honest. A weak
model behind a pretty page is still a
weak model. It is not a public service:
it is built for a household or a small
team on machines you control, and
chapter 15 is plain about the edges of
that. And it is not the only front door
— chapter 16 walks the neighbours, some
of which will suit you better.

Check: if you finish this guide's lab,
you will have Open WebUI running on one
machine, answering through a small model
on the same machine, with two user
accounts and a document it can quote.
Nothing about that setup needs the
internet at chat time — which is rather
the point.
