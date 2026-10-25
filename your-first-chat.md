# Your First Chat

With a connection saved (chapter 6 or
7), the model picker at the top of the
chat page stops being empty. Pick a
model, type, send. The reply streams
in word by word; the conversation
appears in the sidebar, saved as it
goes, and will still be there next
week because the database (chapter 2)
keeps it, not the browser.

Small things that are the product,
not decoration:

- **Several chats, one per subject.**
  A model answers better in a fresh
  conversation than in one that has
  wandered through six topics, because
  it can only see so much of the past
  at once (the *context* — the glossary
  has it).
- **Regenerate and edit.** Any reply
  can be regenerated; any of your
  messages can be edited and re-sent,
  forking the conversation from that
  point. Comparing two answers to the
  same question, side by side in the
  history, teaches more about a model
  than any review.
- **The model menu on your name.**
  Hover a model in the picker and you
  can start a temporary chat (nothing
  saved) or check what the model is —
  its home connection, its size, its
  owner's description if chapter 9 has
  dressed it up.

## The same chat, at the API

The lab's first chat was driven by
curl, because the API is the part you
can photograph. One request:

```bash
curl -s http://localhost:8080/api/chat/completions \
  -H "Authorization: Bearer <token>" \
  -H 'Content-Type: application/json' \
  -d '{"model":"<model-name>",
       "messages":[{"role":"user",
                     "content":"Say hello in exactly three words."}],
       "stream":false}'
```

Check: the reply is one JSON object
whose `choices[0].message.content`
holds the model's words, with a
`usage` block counting tokens in and
out. The full exchange — question,
words, counts — is in
`open-webui-lab/transcripts/t04`. The
same request with `"stream":true`
delivers the words as they are
generated, which is what the web page
itself does behind the glass.

## A word about speed and sense

The words come from the backend, at
the backend's speed, with the
backend's brains. Open WebUI adds no
delay worth measuring and no wisdom
at all. If a reply is slow, the model
or its machine is slow (chapter 17);
if a reply is nonsense, a bigger or
better-suited model is the fix, not a
setting in these pages — though
chapter 9's per-model settings can
steady a model's manners: how long
its answers run, how wild they get,
what it believes its job is.
