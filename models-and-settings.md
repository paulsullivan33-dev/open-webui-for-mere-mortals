# Models and Per-Model Settings

The picker, raw, is a list of whatever
your connections serve: version tags,
quantisation suffixes, names that mean
something to the person who pulled the
model and nothing to anyone else.
**Workspace → Models** is where the
administrator turns that list into a
menu a household can use. Each entry
there is a **model preset**: a base
model wearing a name, a face and a set
of defaults you chose.

## What a preset controls

- **Name and picture.** "Careful
  Writer" instead of
  `llama3.1:8b-instruct-q4_K_M`. The
  base model is still named underneath,
  for the administrator's honesty.
- **System prompt.** Standing
  instructions the model sees before
  every conversation: who it is
  playing, what it should refuse to
  guess at, the tone to hold. The
  single most effective setting in
  this chapter — a good system prompt
  on an ordinary model beats a bare
  bigger model for most household jobs.
- **Default parameters.** Temperature
  (how adventurous the wording gets),
  answer length caps, and friends,
  under *Advanced Params*. Presets
  ship them; users can still adjust
  their own chat's controls where the
  interface offers.
- **Capabilities.** Toggles for what
  the chat window offers beside this
  model: file upload, vision (models
  that can see pictures), web search,
  image generation, code interpretation.
  Turning one on does not give the
  base model the talent — it offers
  the button. Match the toggles to
  what the backend can truly do, or
  chapter 17 gains a section.
- **Knowledge and tools.** A preset
  can carry documents (chapter 11) and
  tools (chapter 12) as defaults, so
  "the manual-answerer" arrives with
  the manual already attached.
- **Who may use it.** Access control:
  a preset is **private to its maker
  until shared** — granted to named
  users and groups (chapter 10), or
  made public to all signed-in users.
  Two facts the lab verified the hard
  way on the current version: a fresh
  user sees *no* models at all, not
  even the raw ones, until something
  is granted to them; and the person
  you share with needs a grant on the
  **base model** as well as on the
  preset, or their picker fills up
  while their first chat still answers
  "Model not found" (transcript t05
  walks it). The big expensive
  connection's models can be an
  adults-only shelf.

The raw base models remain available
under their own names unless you hide
them (the eye toggle in the same
workspace, and per-model visibility in
the admin settings) — most households
hide the raw list once the presets are
written, and lose nothing.

## Two presets worth stealing

**The house explainer.** Base: your
best everyday local model. System
prompt: *"You explain things plainly,
in short paragraphs, and say when you
are unsure. If a question is about
the household documents attached to
you, answer from them and name the
document."* Temperature modest.
Knowledge: the household folder from
chapter 11.

**The careful drafter.** Base: the
strongest model you can reach, local
or hosted. System prompt: *"You draft
and revise text. Offer the draft
first, then one line on what you
changed. Do not invent facts; mark
guesses [guess]."* Access: the adults'
group only, if the backend bills
by the word.

Check: a preset is done when a new
user, given no instructions, picks it
by its name and gets a sensible
conversation. Everything else in this
chapter is in service of that moment.
