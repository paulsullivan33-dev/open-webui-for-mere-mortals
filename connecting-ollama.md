# Connecting Ollama

[Ollama](https://ollama.com) is the
usual first backend: it serves models
from a machine you own and speaks to
Open WebUI natively. This chapter
assumes Ollama is already installed
and running somewhere — the *Ollama
for Mere Mortals* guide in this series
covers that side; here we only wire
the two together.

## The one idea: a base URL

Open WebUI finds Ollama at a **base
URL**, default `http://localhost:11434`.
Three ways to set it, in the order
you will actually use them:

1. **Environment variable** before
   first start: `OLLAMA_BASE_URL`.
2. **Admin pages** afterwards:
   Admin Panel → Settings → Connections
   → *Ollama API*, where you can add
   **several** Ollama servers, each with
   its own URL — the desktop's Ollama,
   the bigger machine's Ollama, all in
   one picker.
3. The API, for scripts: the lab's
   examples file shows the calls.

Saving in the admin pages is enough;
no restart is needed. Within seconds
the model picker lists every model
that Ollama has pulled.

Check, without any clicking:

```bash
curl -s http://localhost:11434/api/tags
```

on the Ollama machine lists its models
by name. If that list is empty, Open
WebUI's picker will be empty too — the
front door cannot show models the
backend does not have. Pull one on the
Ollama side (`ollama pull <model>`),
and it appears; Open WebUI also offers
a pull box of its own in the model
menu, which asks Ollama to fetch the
model for you.

## The localhost trap

The error everyone makes once: Open
WebUI in a Docker container, Ollama on
the host, base URL `http://localhost:11434`
— and no models, forever. Inside a
container, `localhost` is *the
container*. It is listening to itself.
Two fixes:

- Use `http://host.docker.internal:11434`
  (that is why chapter 3's run command
  carries `--add-host`), or
- Use the host's ordinary network
  address, e.g. `http://192.168.1.20:11434`,
  and make sure Ollama is listening on
  the network, not just itself —
  Ollama binds to localhost by default
  and takes `OLLAMA_HOST=0.0.0.0` to
  open up. Only do that on a network
  you trust; Ollama has no password.

The lab sidestepped the trap by having
no containers at all: Open WebUI (pip)
and Ollama were both plain programs on
one machine, and a localhost base URL
simply worked — see the model list and
the chat in `open-webui-lab/transcripts/t04`.
Sidestepping is a luxury of the lab's
shape, not a virtue; on a real install
you will meet the trap, and now you
know its name.

## Several machines, one picker

Nothing limits you to one Ollama.
Each connection adds its models to the
same list, badged by name; two machines
serving the same model name collapse
into one entry that may answer from
either. In a house where one machine
is quick and small and another is slow
and clever, that is the whole game:
chapter 9 is about dressing the list
up so ordinary users see "Fast" and
"Careful" instead of model soup.
