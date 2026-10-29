# Users and Authentication

By the end of this chapter you know
exactly what a Radicale account is —
a name and a hash in a text file — how
to add and remove one, and where the
fence stands between users before any
sharing is configured. The fence is
tested, not assumed: bob spends this
chapter trying to read alice's data
and failing in three different ways.

## The password file

The lab's `htpasswd` file (chapter 4
created it) has one line per user:

```text
alice:$2b$12$BXzq6nR7heWMpyYUg...
bob:$2b$12$wdPeXuU.maDO9mKcXuo...
```

What is after the colon is a **bcrypt
hash** — a one-way scramble of the
password. When alice logs in, Radicale
scrambles the password she presented
and compares scrambles; the password
itself is never stored, and the file
cannot be read backwards into
passwords. That is why the config says
`htpasswd_encryption = bcrypt`, and
why the `[bcrypt]` extra was installed
in chapter 3.

Adding a user is adding a line. The
project documents the `htpasswd` tool
from Apache for maintaining the file;
the lab generated its lines with the
hashing library directly (the snippet
is in chapter 4), which produces the
identical format. Removing a user is
deleting their line — their principal
stops authenticating at once. Their
files stay in the storage folder until
you delete those too; accounts and
data are separate things, and this
guide treats that as a feature (an
accidental line deletion is not data
loss).

Two honest warnings. First, whoever
can read the password file can attack
the hashes at leisure, so the file
belongs to the server's account with
tight permissions, like any password
store. Second, the lab passwords in
this guide (`lab-alice-123`,
`lab-bob-123`) are throwaways for a
disposable server on one machine —
real users get real passwords.

## What the passwords open

With `type = htpasswd`, every request
carries a name and password (HTTP
basic authentication — the login your
calendar app stores when you add the
account). The server's answers come in
three flavours, all seen in the lab:

- **401** — no credentials, or a wrong
  password (transcript t01). The server
  is saying *prove who you are*.
- **207** — the multi-part success
  answer DAV uses; bob asking about his
  own principal `/bob/` got 207
  (transcript t05).
- **403** — *I know who you are, and
  no.* The subject of the next section.

## The fence, tested

Out of the box, `[rights] type =
owner_only` means a user's world is
their own principal. Transcript t05 is
bob's tour of alice's space with his
valid password:

```text
bob GET    /alice/club/photo-walk.ics    403
bob PROPFIND /alice/                     403
bob GET    /alice/contacts/pat-j.vcf     403
bob PUT    /alice/club/forged.ics        403
bob PROPFIND /bob/                       207
```

Read, list, and write are all refused;
bob cannot even confirm what alice's
collections are called. And with no
password at all, the same event
returns 401 — the server does not
distinguish "alice's calendar" from
any other address to a stranger.

**Check:** you have the model when you
can explain why a wrong password (401)
and the right password in the wrong
person's calendar (403) are different
answers — one is about identity, the
other about permission. Chapter 6 is
about deliberately, narrowly, crossing
the second one.
