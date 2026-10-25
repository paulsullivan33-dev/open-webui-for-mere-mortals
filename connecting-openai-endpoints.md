# Connecting OpenAI-Compatible Endpoints

The second connection family is
anything that speaks the **OpenAI API
dialect** — a shape of request and
reply that OpenAI published and the
world proceeded to imitate. That one
decision bought Open WebUI a long
list of possible backends for free:

- **OpenAI itself**, and other hosted
  services with the same dialect, for
  the questions your own hardware
  can't field. These cost money per
  question and send your words to
  someone else's computer — a fair
  trade for some questions, as long
  as it is a *chosen* trade.
- **Programs on your own machines**
  that imitate the dialect:
  **llama.cpp**'s server, **LM Studio**,
  **vLLM**, **text-generation-webui**
  and others. Same privacy as Ollama,
  different serving habits.

## Wiring one up

Admin Panel → Settings → Connections →
*OpenAI API*: a base URL and, for
services that ask for one, an **API
key**. The base URL is the dialect's
front door, conventionally ending in
`/v1` — `https://api.openai.com/v1`
for the genuine article, or
`http://192.168.1.20:8001/v1` for a
llama.cpp server on your network.
Several endpoints can coexist; each
contributes its models to the picker,
and the key belongs to the connection,
not to any user.

Two habits worth forming:

- **Keys are secrets.** A hosted API
  key spends your money. Put it in the
  connection once, as the administrator,
  and let users chat without ever
  seeing it — that is the design.
- **Names will collide.** Two endpoints
  offering "the same" model are listed
  separately here, unlike the Ollama
  side. Chapter 9's presets are how
  you present a tidy list anyway.

Check at the command line, the same
shape for any dialect speaker:

```bash
curl -s http://192.168.1.20:8001/v1/models \
  -H "Authorization: Bearer your-key-if-any"
```

A JSON list of model names means the
door is the right shape. Open WebUI
asks exactly this question when you
save the connection; a red failure
there is this curl failing, dressed
up (chapter 17).

## The lab's version of this

The guide's lab verified this chapter
with the smallest honest backend it
could build: one tiny model file
(a 135-million-parameter GGUF, about
a hundred megabytes) served by
llama.cpp's OpenAI-compatible server
(the llama-cpp-python build of it)
on the same sandbox, answered through
Open WebUI — the request, the model's
reply and the token counts are in
`open-webui-lab/transcripts/t04`, next
to the Ollama round trip, and the two
read the same from the user's chair.
That is the dialect's promise kept:
Open WebUI neither knew nor cared
which program was behind the door.

A note on choosing between the two
families for your own machines: if a
program offers both an Ollama-shaped
and an OpenAI-shaped door, prefer the
native one for that program — Ollama
behind its Ollama connection (chapter
6) gains you the model pull box and
tidier names. The dialect door is for
everything else, and for the hosted
services that have no other.
