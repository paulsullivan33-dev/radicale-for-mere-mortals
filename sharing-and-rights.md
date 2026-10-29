# Sharing and Rights

By the end of this chapter the club
calendar is shared with bob — he can
read every event in it and change
none of them — and both halves of
that sentence have been tested over
the wire. Sharing in Radicale is a
file of rules you write, and the
chapter's main lesson is the one the
lab learned by construction: when you
take over the rules, you take over
*all* of them.

## From a built-in rule to a file

Chapter 5 ran on `type = owner_only`,
a rule compiled into the program. To
share anything, change the config:

```ini
[rights]
type = from_file
file = /home/hatch/workspace/radicale-lab-work/rights
```

`from_file` replaces the built-in rule
entirely. Nothing is implied any more
— if the file does not grant it, it
does not happen. So the lab's rights
file (`radicale-lab/lab-rights`) starts
by re-stating the rule it replaced:

```ini
[owner-full-access]
user: .*
collection: {user}(/.*)?
permissions: RWrw
```

Read it as: *any user (`.*`) has all
four permissions (`RWrw` — read and
write the collection, read and write
its items) on collections under their
own principal (`{user}` is replaced by
whoever is asking).* Skip this rule
and your users lose their own
calendars the moment you switch to
`from_file` — the classic Radicale
first-day mistake, avoided here by
writing the owner rule first.

## The share itself

One more rule does the sharing:

```ini
[club-calendar-read-for-bob]
user: bob
collection: alice/club(/.*)?
permissions: Rr
```

- `user: bob` — exactly bob; a name,
  or a pattern if you mean a class of
  users.
- `collection: alice/club(/.*)?` — the
  club calendar and everything in it.
  Not `alice/contacts`. Not anything
  else alice owns.
- `permissions: Rr` — read the
  collection, read its items. No `W`,
  no `w`: nothing with a write in it.

Restart the server (the rights file
is read as the server runs, but a
restart is the honest way to be sure
your config change is the live one),
 and the boundary moves — precisely
as far as the rule says and no
further.

## Tested from both sides

Transcript t06 is the proof, run
immediately after the restart:

```text
bob GET    /alice/club/photo-walk.ics    200  reads the event
bob PROPFIND /alice/club/                207  lists the calendar
bob PUT    /alice/club/bob-event.ics     403  cannot add
bob DELETE /alice/club/photo-walk.ics    403  cannot remove
bob GET    /alice/contacts/pat-j.vcf     403  contacts NOT shared
alice PUT  /alice/club/darkroom-night.ics 201 owner unaffected
bob GET    /alice/club/darkroom-night.ics 200 share covers new events
```

Three properties worth naming. The
share is **collection-wide**, not
event-by-event: the darkroom night
alice added afterwards was readable
by bob at once. The share is
**read-only in fact, not in label**:
both write verbs were tried and both
were refused. And the share is
**narrow**: the address book, named
in no rule for bob, stayed at 403.

To unshare, delete the rule. To share
with more members, add a rule per
member — or one rule whose `user:`
pattern names the group, if your
naming scheme gives members a shared
shape. Every widening is a line you
can point at, which is the whole
design: there is no checkbox somewhere
doing something you cannot see.

**Check:** before trusting any share,
run bob's two write attempts yourself.
A share you have not tried to break
is a hope, not a boundary.
