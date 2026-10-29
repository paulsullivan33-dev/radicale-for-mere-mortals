# The Neighbours

Radicale sits between doing nothing
special at all and running a whole
groupware suite. These are
characterisations from the projects'
own documentation, not benchmarks; the
lab ran Radicale only — and, for the
links below, every named guide was
checked against the series index
before being named.

## The big clouds: the default you left

Google Calendar, iCloud, and Outlook's
calendars are CalDAV-shaped on the
inside but are used as accounts, not
servers: no files you can open, no
rules you can read, and availability
and terms set elsewhere. They are
genuinely excellent at the parts
Radicale does not attempt — push to
phones, invitations across the world,
spam-proof reliability. What they
cannot be is *yours*. Radicale exists
for the reader who has weighed that
trade and chosen the folder of files.

## Nextcloud: the suite

[Nextcloud for Mere Mortals](https://paulsullivan33-dev.github.io/nextcloud-for-mere-mortals/)
is the natural big neighbour: your own
cloud for files, calendars and
contacts, with a proper web interface
for all of it, sharing by link and by
group, and an app ecosystem. Its
calendar and contacts speak CalDAV
and CardDAV too, so the clients from
chapter 7 cannot tell the difference.
The difference is everything else:
Nextcloud is a database-backed PHP
platform you patch, tune and back up
as a platform; Radicale is a Python
program and a folder. Choose Nextcloud
when you want the suite. Choose
Radicale when calendars and contacts
are the whole job and you would like
the job to stay small enough to
understand in an evening.

## DAViCal and Baïkal: the same street

Two other self-hosted DAV servers are
worth knowing by name. **DAViCal** is
the elder, database-backed (PostgreSQL)
CalDAV/CardDAV server — more machinery,
and more delegation and scheduling
features for organisations. **Baïkal**
is a lightweight PHP front end over a
DAV library, with a web admin page as
its centre rather than config files.
Radicale's distinguishing card against
both is the storage: plain per-item
files you can read with `cat`, and a
configuration you can hold in one
screen. If a calendar stored as rows
in a database does not bother you, the
street has options; if it does,
Radicale is the one that agrees.

## Nothing at all: files and a sync tool

The zero-server option is real: keep
`.ics` and `.vcf` files in a folder
and let a file-syncing tool (the
series' [Syncthing guide](https://paulsullivan33-dev.github.io/syncthing-for-mere-mortals/)
covers one) carry the folder between
machines. What you lose is the
protocol: no per-user fence, no
read-only share of one calendar, no
client discovery — and two devices
editing the same calendar file is a
conflict a DAV server exists to
arbitrate. It suits a single person
with a single calendar app that reads
local files. The moment a second
person, or a phone that expects an
account, enters the picture, you are
back to wanting a server — and this
one is small.

**Check:** name your deciding fact. If
it is "I want the web suite too", it
points at Nextcloud; if it is "the
data must sit in files I can read",
it points here.
