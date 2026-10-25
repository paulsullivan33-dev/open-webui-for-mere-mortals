# Users and Roles

Open WebUI's accounts are the
household's front-door keys. This
chapter is short because the model is
simple, and one rule dominates it:

> **Administrators run the place. Users
> chat. Pending users wait.** Almost
> every surprise in a shared install is
> one of those three sentences being
> forgotten.

## The three roles

First, the gate in front of all of
them: after the first account exists,
**sign-ups are switched off** (chapter
4) until an administrator switches
them on. Nobody can even knock
uninvited. While the door is open,
new accounts arrive as *pending*
(the `DEFAULT_USER_ROLE` setting):

- **Admin** — the first account
  (chapter 4) and anyone an admin
  promotes. Sees the Admin Panel:
  connections, settings, all users.
  Can read the bookkeeping of the
  whole install. There is no
  "slightly admin": the role is the
  whole set of keys, so hand it out
  like one.
- **User** — chats, uploads documents,
  builds their own knowledge (chapter
  11), sees the models their access
  allows (chapter 9).
- **Pending** — a sign-up nobody has
  approved. A pending account can sign
  in far enough to be told to wait,
  and no further. Approving is one
  click in Admin Panel → Users, where
  you also choose the role they land
  with.

Which role new sign-ups wait in is the
`DEFAULT_USER_ROLE` setting (chapter 4)
— leave it at *pending* on anything
reachable beyond your own walls. You
can also set it to *user* for a
trusted home network, and some do; the
guide's advice is to feel the weight
of the choice once, on purpose, rather
than discover it later.

## Groups

Users can be gathered into **groups**
— the adults, the kids, the committee
— and groups are what permissions are
checked against: which models appear
(chapter 9), which knowledge and tools
are offered (chapters 11 and 12). A
group is a mailing list with teeth:
put a person in "adults" and the
hosted, billed-by-the-word model
appears in their picker; take them
out and it vanishes, no argument with
the software required.

## What administrators can see — honestly

The admin pages list every user and
every conversation's title and owner,
and the database holds the full text
of every chat on the server (it has
to — chapter 2). So the honest social
rule of a household install is:

- Tell your users the administrator
  *can* read their chats, the way the
  person who owns the filing cabinet
  can open the drawers.
- If someone needs a truly private
  model conversation, the answer is a
  model on their own machine, not a
  promise the software can't keep.

This is not a flaw in Open WebUI; it
is what "running the server" means,
and naming it plainly is part of
running one kindly.

## Turning the door off and on

- `ENABLE_SIGNUP=false` (or the
  settings toggle) stops new accounts
  being requested at all — the right
  state for a settled household.
- `WEBUI_AUTH=false` removes accounts
  entirely: whoever can reach the page
  is in. Defensible exactly once — a
  single-user machine the network
  can't reach — and a standing
  invitation everywhere else. The
  setting exists; the guide does not
  recommend it, and says so here so
  that the troubleshooting chapter
  doesn't have to whisper it.

Check: the lab's evidence for this
chapter is small and complete — the
first account arriving as `admin`, a
second as `pending`, the admin's
promotion of it to `user`, and the
list showing both — in
`open-webui-lab/transcripts/t05`.
