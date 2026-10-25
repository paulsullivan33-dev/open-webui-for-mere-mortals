# Your First Sign-in

Fresh Open WebUI has no accounts at
all. The sign-in page offers **Sign
up**, and the first account to be
created through it is special:

> **The first user becomes the
> administrator.** Later sign-ups —
> once an administrator has turned
> sign-ups on at all — land as
> *pending* accounts that can do
> nothing until approved
> (chapter 10).

So the order of operations on a new
install is always the same, and the
first step is not optional:

1. Start the server (chapter 3).
2. **Immediately create your own
   account**, with a password you would
   use for anything you cared about.
   You are now the administrator.
3. Only then tell anyone else the
   address.

On a machine only you can reach, step
2's urgency is mild. On anything
reachable from a network you do not
control, the gap between "installed"
and "first account created" is a window
in which a stranger can become your
administrator. Close the window first;
admire the wallpaper later.

## What the administrator sees

Sign in and the place is yours: a chat
page with an **empty model picker**
(unless a backend was already connected
by an environment variable — chapters
6 and 7 fix the emptiness), a sidebar
for conversations, and, behind your
profile picture, the **Admin Panel**:
users, settings, and the connections
this guide spends the next two chapters
on. Ordinary users never see the Admin
Panel; chapter 10 covers who sees
what, and how to hand out accounts
without handing out the keys.

## The same thing without a browser

Everything the pages do, the API does,
and the lab proved its install at the
command line. Creating the first
account is one request:

```bash
curl -s -X POST http://localhost:8080/api/v1/auths/signup \
  -H 'Content-Type: application/json' \
  -d '{"name":"Ada Admin",
       "email":"ada@example.com",
       "password":"a-long-password"}'
```

Check: the reply is a JSON object with
your new account's `id`, `email`, and
`"role":"admin"` — that word, on the
*first* account only — plus a `token`
you can use as
`-H "Authorization: Bearer <token>"` on
later requests. Signing in again later
is the same shape at
`/api/v1/auths/signin`.

And note what the server did while
you weren't looking: **it turned
sign-ups off**. Current versions close
the door behind the first account
(`ENABLE_SIGNUP` reads false from now
on), so the same signup call from a
stranger — or from your partner at the
kitchen table — is refused until you,
the administrator, turn sign-ups back
on in the admin settings. What happens
then is chapter 10's subject: the new
account lands as *pending*, and waits.

## Two settings to make now

While it is just you:

- **Know the state of the door.**
  Sign-ups turned themselves off when
  your account was created (above).
  Leave them off except while you are
  deliberately enrolling people, if
  the server is reachable beyond
  people you know: Admin Panel →
  Settings → General → *Enable New
  User Sign Ups*. The environment
  variable `ENABLE_SIGNUP` sets the
  starting state before first start.
- **Note your default role for new
  users** (`DEFAULT_USER_ROLE`, shown
  in the same settings page): *pending*
  is the safe default this guide
  assumes.

The transcripts for this chapter —
first account, the `admin` role in the
reply, a second account arriving as
`pending` — are in
`open-webui-lab/transcripts/t03`.
