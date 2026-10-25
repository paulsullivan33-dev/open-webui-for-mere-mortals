# Recipes

Short, complete jobs. Each assumes the
chapters before it and says which
backend it needs.

## The kitchen-table explainer

*Needs: any Ollama model, one machine.*

Open WebUI on the family machine,
Ollama beside it (chapter 6), one
preset (chapter 9) named **Explainer**
with the chapter's system prompt and a
modest temperature. Accounts for each
person (chapter 10), sign-ups off
once everyone is in. Total moving
parts: two programs and one data
folder. This is most households' whole
destination, and it is a good one.

## The manual shelf

*Needs: a model with a long context,
and the documents chapter.*

Create a knowledge collection per
shelf — *Appliances*, *House*, *Car* —
from the PDFs that currently live in a
drawer. Build one preset per shelf,
named so a guest can guess it:
**Ask the House**. Attach, don't
full-context, anything over a few
pages. Add each document's source and
date to its name when you upload it;
future-you, reading a citation, will
be grateful.

## The two-brain house

*Needs: a small fast local model and
one heavyweight (local server or
hosted).*

Presets: **Quick** (the small model,
temperature up a touch, for chat and
drafts) and **Careful** (the heavy
model, chapter 9's drafter prompt,
access limited to the adults' group
if it bills). Teach the household the
only rule that matters: start with
Quick; escalate when Quick's answer
smells thin. Costs, speed and quality
all land where they should.

## Homework rules

*Needs: any model; groups from
chapter 10.*

A **Homework** preset whose system
prompt reads: *"You are a tutor. Never
give the final answer to a school
problem. Ask what they've tried, give
one hint at a time, and praise the
working, not the answer."* Access: the
kids' group only gets this preset for
the strong models; their raw chat
presets stay modest. The software
enforces which is offered; the prompt
enforces how it behaves; both layers
earn their place.

## The offline weekend box

*Needs: everything local.*

Open WebUI + Ollama on one laptop,
models pulled in advance, data
directory on an encrypted volume.
Chat, documents and all work with the
Wi-Fi off and the cottage dark —
first start happened at home (chapter
3's embedding download), which is the
one errand to run before you leave.
Check before departure:
`curl -s http://localhost:8080/health`
and one real question to the real
model, with the network unplugged.

## The committee secretary

*Needs: a long-context model, the
documents chapter, one trusted admin.*

Knowledge collection of minutes and
papers, a preset instructed to answer
*only* from the collection and to say
"not in the papers" when retrieval
finds nothing — plus the habit,
taught in person once, of clicking
the citations (chapter 11). Access
limited to the committee group. This
recipe is the documents feature at
its most defensible: a bounded
library, a bounded claim, a checkable
answer.
