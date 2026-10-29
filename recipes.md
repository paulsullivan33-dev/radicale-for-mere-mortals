# Recipes

Three working arrangements, each built
only from pieces the earlier chapters
verified. Passwords in the snippets
are stated as the lab's throwaways;
substitute real ones.

## 1. A household server

*Goal: two people, each with a private
calendar and contacts, plus syncing
between their own devices.*

- One account per person in the
  password file (chapter 5).
- Leave `[rights] type = owner_only`
  exactly as installed. No sharing is
  configured, so chapter 5's fence is
  the whole policy: each person's
  principal is theirs alone (bob's 403
  tour is your evidence).
- Each person's phone and laptop get
  the base address and their own
  account (chapter 7).

Nothing else. Most households need no
rules file at all — resist writing one
until a calendar actually has to cross
a fence.

## 2. A club calendar the members can read

*Goal: one events calendar, maintained
by the organiser, readable by every
member — the lab's exact setup.*

- The organiser owns the calendar
  (`/alice/club/`, chapter 4).
- Each member gets an account —
  read access still requires a login,
  which is what keeps the calendar off
  the open internet.
- The rights file carries the owner
  rule from chapter 6 plus one read
  rule per member:

```ini
[club-calendar-read-for-bob]
user: bob
collection: alice/club(/.*)?
permissions: Rr
```

- Verify like the lab did: the member
  reads (200) and is refused on add
  and delete (403, 403) before you
  announce the calendar (transcript
  t06's checklist, with your names).

The organiser's own access is
unaffected, and new events are visible
to members the moment they are saved —
the share is on the collection, not on
individual events.

## 3. One shared address book

*Goal: a household contacts book that
two people keep up to date together.*

The same shape as recipe 2, with two
changes: the shared collection is an
address book (`/alice/contacts/`,
chapter 4's MKCOL), and the share is
read-**write**, because both people
maintain it:

```ini
[contacts-readwrite-for-bob]
user: bob
collection: alice/contacts(/.*)?
permissions: RWrw
```

Know what you just granted: bob can
now add, change and *delete* contacts
in alice's book. Read-write shares are
for people you trust with the delete
key — and one more reason the storage
folder belongs in a real backup
routine (chapter 8), since a sync
mistake in a shared book propagates
like any other change. The lab
verified the read-only form of this
boundary in both directions; the
read-write form is the same mechanism
with wider letters, assembled from
verified parts and labelled as such.
