# Tool Servers

A chat model, left alone, can only
talk. **Tools** are how it gets hands:
small, named actions — *get the
weather for a place*, *do this
arithmetic*, *look up this part
number* — that the model may choose to
call in the middle of answering, whose
results it then reads before going on.
A **tool server** is a program, living
somewhere else, that offers a bundle of
such actions to Open WebUI over the
network. This chapter is the concept
and the wiring; the examples are
generic on purpose, because a tool is
a door into whatever the tool's author
connected it to.

## How a call flows

1. You connect a tool server (next
   section). Open WebUI asks it what
   it offers and writes the answer
   down as a list of named functions,
   each with a plain-language
   description and a list of inputs.
2. In a chat where the tool is enabled
   (per chat, per model preset, or per
   user's toolbox), those descriptions
   ride along with your question.
3. If the model judges a tool useful,
   it does not answer — it emits a
   **tool call**: the name, and the
   inputs filled in from your words.
4. Open WebUI runs the call against
   the server, takes the result, and
   hands it back to the model, which
   finally answers *with the result in
   front of it*.

Check the failure shape this implies:
the model can only call what it's
offered, with inputs it invents
plausibly. A tool named `get_weather`
with a description "current weather
for a city" will be called for
tomorrow's forecast too, unless its
description says it can't. Tool
descriptions are prompts, and are
written (and miswritten) like them.

## Wiring one up

Open WebUI speaks to tool servers two
ways, both under Admin Panel →
Settings → Tools (and per-user under
their own settings where allowed):

- **OpenAPI tool servers.** A service
  that publishes an **OpenAPI**
  description of itself at a URL can
  be connected directly: give Open
  WebUI the server's base address and
  the address of its spec (often
  `/openapi.json`), plus a key if it
  wants one. Every operation in the
  spec becomes a callable tool.
  Purpose-built little servers exist
  whose whole job is to be connected
  this way — a clock-and-calculator
  server is the traditional first one.
- **MCP servers, via a proxy.** The
  wider world of tools increasingly
  speaks **MCP** (Model Context
  Protocol). Open WebUI connects to
  those through a small translator
  program (the project's companion is
  `mcpo`) that runs the MCP server and
  re-publishes it as an OpenAPI door
  Open WebUI can use. Run the proxy,
  connect Open WebUI to the proxy.

Built-in cousins live inside Open
WebUI itself: **tools** and **functions**
written in Python in the Workspace are
the same idea with no server at all —
handy for one household's small jobs,
and the reason the settings pages use
slightly overlapping words. The
neighbours in this space include web
search and image generation settings,
which are tools in spirit and settings
in placement.

## A generic worked shape

Suppose a small program on your
network offers, at
`http://192.168.1.20:8100`, an OpenAPI
spec describing exactly one operation:

```
GET /multiply?a=<number>&b=<number>
    → {"result": <number>}
```

Connect that address as a tool server,
enable it in a chat, and ask a model
"what is 3917 times 517?" A model with
a head for tools answers by calling
`multiply` with 3917 and 517 and
quoting the returned 2,025,089, having
done no arithmetic of its own — which,
for arithmetic, is exactly what you
want. The same shape with a calendar,
a stock list or a door lock is why
this chapter comes with rules instead
of confetti:

- **A tool server is trusted code with
  your users' questions.** It sees
  what the model sends it. Connect
  servers you have read or have reason
  to trust, on addresses you control.
- **Keys stay in the connection**, set
  by the administrator (chapter 10's
  rule again), never typed into chats.
- **Start read-only.** A tool that can
  *change* things — send, buy, unlock —
  should arrive after the household
  has watched read-only tools behave,
  and wear an unmistakable name.
- **Expect the model to be the weak
  link** for a while: calling the right
  tool with the wrong inputs is the
  common failure, and it looks, in
  the transcript, exactly like the
  tool misbehaving. Chapter 17 shows
  how to tell them apart (the chat UI
  displays each call and its result,
  if you open the detail view).

## What the lab verified

The lab built the chapter's toy
example for real — a small stdlib
Python server offering one `multiply`
operation behind an OpenAPI spec —
connected it through Open WebUI's
settings API, and watched Open WebUI
fetch and verify the spec and save
the connection (transcript t07). The
final step, the model choosing to
call it, did **not** happen: the
lab's model is a 135M-parameter
miniature, and it confidently
multiplied wrong instead, twice.
Treat that as this chapter's honest
footnote: the wiring is proven, and
the weak link was exactly where the
rules above said it would be.
