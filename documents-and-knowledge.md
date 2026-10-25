# Documents and Knowledge

This is the chapter people install
Open WebUI for: **upload the document,
ask the model about it.** Done well it
feels like the model has read the
manual. Done carelessly it is a
machine for confident nonsense about
your own paperwork — the difference is
understanding, at survey level, what
actually happens to a document. So
this chapter is mostly that.

## Two ways documents meet a model

**Attached to one chat.** In any
conversation, the paperclip takes a
file — PDF, text, office document,
image for a vision model — and offers
two modes:

- **Full context**: the document's
  text is placed into the conversation
  itself, in front of your question.
  Exact, quotable, and limited by how
  much the model can hold at once —
  a few dozen pages at most, and the
  whole document is re-read (and
  re-billed, on a hosted backend)
  with every message.
- **Retrieval** (the default for
  anything sizeable): the document is
  chopped and *searched* for the
  pieces nearest your question, and
  only those pieces are shown to the
  model. This is the mechanism the
  next section opens up.

**Knowledge.** A **knowledge base**
(Workspace → Knowledge) is a named,
kept collection of documents: the
household manual shelf, the project's
papers. You attach a collection to a
chat, or bake it into a model preset
(chapter 9), and every question
searches it. Collections have owners
and access like everything else here
(chapter 10), so the committee's
papers need not sit beside the kids'
homework.

## What retrieval actually does

1. **Extract.** The file's text is
   pulled out. A born-digital PDF
   gives clean text; a scan gives
   whatever the OCR makes of it —
   garbage in, gospel out is not a
   thing, so scanned documents are
   this feature's weakest input.
2. **Chunk.** The text is cut into
   overlapping pieces of a few hundred
   words. Seams matter: a fact cut in
   half can be missed by both halves.
3. **Embed.** Each piece becomes a
   list of numbers from the **embedding
   model** (chapter 2), stored in the
   vector database beside the text.
4. **Retrieve.** Your question is
   embedded the same way, and the
   pieces whose numbers are closest
   are fetched — the default is a
   handful — and handed to the chat
   model with your question.
5. **Answer.** The chat model answers
   from those pieces. It can only cite
   what it was handed; ask something
   whose evidence lives in a piece the
   search didn't fetch and it will
   answer from vibes instead, fluently.

That last step is the whole honesty
of the feature, and the citations the
interface shows under an answer —
which document, which piece — are
worth more than the answer: they are
how you check. Teach your users to
click them.

## Knobs that matter

Under Admin Panel → Settings →
Documents, and per knowledge base:

- **Embedding model**: the built-in
  default is small and honest; a
  bigger one (or Ollama/OpenAI
  embeddings) can sharpen retrieval
  for large collections, at setup and
  storage cost. Re-embedding a whole
  library on a model change takes
  time — the numbers are not
  transferable between models.
- **Chunk size and overlap**: larger
  pieces carry more context and dilute
  the search; smaller ones pinpoint
  and fragment. Defaults are sane;
  change them when a real collection
  misbehaves, not on spec.
- **Top K and hybrid search**: how
  many pieces are fetched, and whether
  plain keyword matching rides along
  with the number-matching (it helps
  with names, codes and other things
  embeddings smear — searching a part
  number by meaning alone is a mug's
  game).
- **Relevance threshold**: pieces
  below a similarity floor are left
  out, which is how you stop the model
  quoting the wrong document with
  great confidence.

## Honest limits of the survey

This chapter deliberately stops at
survey level. A serious document
library wants decisions about formats,
scanning quality, collection design
and re-embedding that fill guides of
their own, and chapter 15 lists the
practical ceilings (document size,
context length, the scan problem)
without varnish. The lab's evidence
here is one small text file, uploaded,
embedded, and quoted back with its
citation — `open-webui-lab/transcripts/t06`
— which proves the pipeline end to
end and nothing grander.
