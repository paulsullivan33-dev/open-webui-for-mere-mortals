# Troubleshooting

In the order you will meet them.
Start at the top; most installs only
ever meet the first three.

## The model picker is empty

**Shape:** everything works, nobody
home. **Cause, almost always:** no
working connection (chapter 2), or a
backend with no models. Check the
backend directly (`curl` to its
`/api/tags` for Ollama, `/v1/models`
for the dialect, chapters 6 and 7),
then the connection in Admin Panel →
Settings → Connections — a red or
stale entry there is the answer, and
saving it again re-runs the check.
The containerised reader should
re-read the localhost trap in
chapter 6 now; everyone does, once.

## The page loads, sign-in spins or fails

**Cause:** usually the server is
still in its first start — database
being built, embedding model
downloading (chapter 3) — or the
`WEBUI_SECRET_KEY` changed between
restarts, retiring every session.
Watch the log until startup completes
before suspecting accounts; if you
*set* the key, set it everywhere the
server starts from (chapter 13's unit
file is the usual second place it
hides).

## "Connection refused" at the backend address

**Cause:** the backend is not
listening where Open WebUI is
knocking. From the Open WebUI
machine's chair — the container's
chair, if containerised — run the
chapter's check curl. Backend
listening on its own localhost only
(Ollama's default) plus Open WebUI
anywhere else is this error's
permanent address; the fix is the
backend's bind setting and your
network's trust, in that order.

## Answers are slow

**Cause:** the backend, essentially
always. Open WebUI moves text; the
model's machine generates it.
Check size of model against size of
machine, whether anything else is
sharing the machine, and whether the
chat has grown enormous (the whole
history is re-sent and re-read each
message — a fresh chat is the cheap
speed-up nobody believes until they
try it).

## Documents: answers cite the wrong thing, or nothing

**Cause:** retrieval (chapter 11)
fetched mush: scan-grade source,
chunks cut at bad seams, too many
pieces competing, or the question's
evidence phrased unlike the
document's. Read the citations,
which show what was actually fetched;
then fix the input — cleaner text,
saner chunks, a bigger Top K for a
broad question — before blaming the
chat model, which answered from what
it was handed.

## Uploads refused or vanish on restart

**Cause:** the file exceeded the
upload size settings, or — the
restart flavour — the data directory
is not where the server thinks it is,
so each start is a fresh install
wearing your install's clothes. The
startup log names the data directory
it used. Believe it, fix
`DATA_DIR` (or the volume) to match
the place your data actually lives,
and the vanishings stop.

## A tool call fails, or the wrong tool fires

**Shape:** the chat shows a tool
error, or a confident answer with no
call at all. Open the message's
detail view: it shows the call the
model made, its inputs, and the
server's reply. **Inputs nonsense,
reply fine** → the model misread the
tool's description; rewrite the
description like a prompt (chapter
12). **Call well-formed, reply an
error** → the tool server's own
problem; test it at its own address
with the same inputs. **No call at
all** → the tool wasn't enabled for
that chat or model, or the model is
too small to tool-call reliably —
a real capability floor, not a
setting.

## Everybody was logged out, database "reset"

**Cause:** a new data directory (see
uploads, above) or a restored backup
without its secret key (chapter 14).
Nobody's chats were deleted by the
software; they are in the directory
the server stopped looking at. Find
that directory, point the server at
it, keep the key beside the backup
from now on.

## When all else fails

Read the server's log from the last
restart with fresh eyes, check the
health endpoint, and name the failing
part in chapter 2's vocabulary —
application, database, connection,
embedding, tool — before searching
for help. Half the help online
assumes Docker; if you installed by
pip, say so when you ask, and your
answers will double in usefulness.
