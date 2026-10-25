# Glossary

**Admin** — The role of the first
account; runs users, connections and
settings for the whole install.
Chapter 10.

**Backend** — Whatever generates the
text: an Ollama server, a hosted API,
a local program speaking the OpenAI
dialect. Open WebUI is never the
backend. Chapter 2.

**Chunk** — One of the overlapping
pieces a document is cut into for
retrieval. Chapter 11.

**Connection** — A saved backend
address (and key) in Open WebUI's
settings. Everything in the model
picker arrives through one.
Chapters 6 and 7.

**Context** — Everything a model is
shown for one reply: the system
prompt, the conversation so far, any
fetched document pieces. Finite,
and the reason long chats forget.

**Data directory** — The folder
holding the database, uploads and
settings; the whole of Open WebUI
for backup purposes. Chapters 2
and 14.

**Embedding** — A list of numbers
representing a piece of text's
meaning, made by an **embedding
model**; closeness of numbers is
how retrieval finds pieces.
Chapters 2 and 11.

**GGUF** — A file format for holding
a model, used by llama.cpp and
its relatives. Chapter 7's lab
backend was served from one.

**Knowledge base** — A kept
collection of documents in Open
WebUI, attachable to chats and
presets. Chapter 11.

**MCP (Model Context Protocol)** —
A standard shape for tool servers;
Open WebUI reaches MCP servers
through a proxy such as `mcpo`.
Chapter 12.

**Model preset** — A base model
dressed by an administrator with a
name, system prompt, defaults and
access rules. Chapter 9.

**Ollama** — A program for serving
models from your own machines; the
canonical first backend. Chapter 6
and its own guide in this series.

**OpenAI-compatible** — Speaking
the OpenAI API dialect: the request
and reply shape that many backends,
hosted and local, imitate.
Chapter 7.

**Pending** — The waiting role of
an unapproved sign-up; can do
nothing until an admin promotes
it. Chapter 10.

**Retrieval (RAG)** — Fetching the
document pieces nearest a question
and showing them to the model,
instead of the whole document.
*RAG* is its initials: retrieval-
augmented generation. Chapter 11.

**System prompt** — Standing
instructions a model sees before
the conversation; a preset's most
effective setting. Chapter 9.

**Token** — The small unit (roughly,
a word-piece) in which models read,
write and are billed. Also, in this
guide's API transcripts, the bearer
string that proves a sign-in; the
two meanings share the word and
nothing else.

**Tool / tool server** — A named
action a model may call mid-answer,
and the program offering bundles of
them. Chapter 12.

**Top K** — How many document pieces
retrieval fetches for a question.
Chapter 11.

**Vector database** — The store
that keeps embeddings and finds the
closest ones quickly; Open WebUI
carries a small one built in.
