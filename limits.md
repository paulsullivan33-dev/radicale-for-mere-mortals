# Limits

Radicale is small on purpose, and this
chapter names what the smallness
costs, so none of it surprises you in
service.

**Scale.** Radicale is built for a
person, a household, a club. The
filesystem storage that makes it so
inspectable is not the design you
would choose for thousands of users or
huge public calendars, and there is no
clustering or high-availability story.
If your calendar server needs an
operations team, this is the wrong
tool and the neighbours chapter names
the bigger ones.

**No push of its own.** Phones learn
about changes when their sync app
next polls, or via machinery outside
Radicale (the sync app's own push
arrangements). Radicale itself does
not reach out and tap a phone on the
shoulder; a change can wait minutes
to appear on a device that polls
rarely. For club calendars and
households this is a shrug; for
minute-critical dispatching it is a
disqualifier.

**Invitations and scheduling are
thin.** The rich meeting-invitation
dance — invites, replies, free/busy
across organisations — is mostly the
clients' and the wider protocol's
business; a small server stores the
events and does not orchestrate the
social layer the big clouds wrap
around them. The lab verified storage,
retrieval and queries, not invitation
workflows (documented-only, per the
criteria).

**The web page is an admin tool, not
a calendar.** The built-in web
interface (verified answering in the
lab) manages collections and logins.
It is not where anyone should read
their week. Your calendar app is the
interface; Radicale is the shelf
behind it.

**Security is a posture, not a
feature list.** The fence in chapters
5–6 is real, but it rests on things
outside Radicale: passwords worth the
name, the password file kept private,
and — once the server listens beyond
one machine — HTTPS in front of it,
which Radicale does not itself
provide (a reverse proxy or tunnel
does; chapter 7). A Radicale on plain
HTTP across a network is a calendar
*and* its passwords, readable in
transit. The tool does not stop you;
this chapter is the stopping.

**One file per item cuts both ways.**
Enormous single collections mean
enormous numbers of small files;
filesystems handle it, but browsing
the storage tree stops being the
pleasant inspection chapter 4 enjoyed.
And the storage format being plain
means anyone with filesystem access
has the data — encryption at rest is
your disk's job, not Radicale's.

**Etiquette of a shared book.** A
read-write share (recipe 3) has no
undo and no blame: the server records
the present, not who did what.
Radicale keeps a small history cache
for sync purposes; it is not an audit
log and this guide does not present
it as one.
