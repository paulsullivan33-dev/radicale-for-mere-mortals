# The Moving Parts

Radicale has four ideas in it. Learn
these and every later chapter is just
detail: **collections**, **the storage
folder**, **users**, and **rights**.

## Collections: calendars and address books

Everything Radicale serves lives in a
*collection*. A collection is either a
**calendar** (holds events), an
**address book** (holds contacts), or a
plain folder that groups them. Each
collection has an address made of the
owner's name and a name you choose:

```text
/alice/club/        a calendar
/alice/contacts/    an address book
```

The first part — `/alice/` — is the
user's *principal*: their home on the
server, which the server itself fills
in when the user first appears. Inside
a calendar, each event is one item
with its own address
(`/alice/club/photo-walk.ics`); inside
an address book, each contact is one
item (`/alice/contacts/pat-j.vcf`).
Clients create, read, change and delete
items one at a time, over HTTP — the
same verbs a browser uses, plus three
DAV verbs: `PROPFIND` (list and
describe), `MKCOL`/`MKCALENDAR` (make a
collection), and `REPORT` (ask a real
question, like "events in November?").

**Check:** chapter 4 creates the club
calendar and chapter 4's transcript
(t02) shows the server's storage grow
exactly one `.Radicale.props` file for
it — the collection exists as a folder
before it holds anything.

## The storage folder: files all the way down

The storage folder mirrors the
addresses. The lab's tree, after one
calendar, one address book, two events
and a contact (transcript t07):

```text
storage/collection-root/
  alice/
    club/
      photo-walk.ics        the event itself
      darkroom-night.ics    the second event
      .Radicale.props       the collection's name and kind
    contacts/
      pat-j.vcf             the contact itself
```

An event file contains exactly the
iCalendar text the client sent; a
contact file, exactly the vCard. The
`.Radicale.props` file per collection
records its display name and type, and
a `.Radicale.cache/` folder per
collection holds indexes the server
rebuilds as needed — useful to the
server, safe to lose, and not part of
your data. There is no database server,
no schema, no migration. If Radicale
vanished tomorrow, your calendars would
still be sitting there as readable
text. Chapter 8 turns that into a
backup plan.

## Users: a name and a password

A *user* is a line in a password file:

```text
alice:$2b$12$BXzq6nR7heWMpyYUg... (a bcrypt hash)
```

The file is the classic `htpasswd`
format — the same one web servers have
used for decades — and the password
itself is never stored, only a hash
that can check a password but not
reveal it. A user's name becomes their
principal address (`/alice/`), and by
default everything under it is theirs
alone. Chapter 5 makes the users and
tests what their passwords open.

## Rights: who may do what, where

*Rights* are the rules that decide, for
each request, whether this user may do
this thing to this collection. Radicale
ships with a built-in rule,
`owner_only`, that needs no
configuration at all: your collections
are yours, full stop. The lab proved
it before changing it — a second user,
bob, asking for alice's event got a
flat **403** (transcript t05).

When you want sharing, you switch the
rights setting to read rules from a
file you write. Each rule names a user
(or a pattern), a collection (or a
pattern), and a set of permission
letters:

```ini
[club-calendar-read-for-bob]
user: bob
collection: alice/club(/.*)?
permissions: Rr
```

The letters are precise: `R` reads the
collection itself, `r` reads the items
in it, `W`/`w` are the matching writes.
`Rr` is "look, but don't touch" — and
chapter 6 shows bob reading the shared
calendar happily and being refused,
with another 403, the moment he tries
to add to it.

**Check:** with the four ideas in hand,
predict this before chapter 6 runs it —
bob, who has no rule for
`/alice/contacts/`, asks for the
contact there. If you said "403, same
as before the rights file existed",
you have the model.
