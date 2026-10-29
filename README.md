# Radicale for Mere Mortals

Your calendar and your contacts live,
by default, on somebody else's computer.
A phone maker's cloud holds them, syncs
them, and — on its side of the bargain —
can read them, lose them, or change the
terms. **Radicale** is the small way out:
a single Python program that serves
calendars and address books from your
own machine, in the standard protocols
your apps already speak, and stores
every event and contact as a plain text
file in a folder you can open, read,
and back up with ordinary tools.

This guide builds one for real. A lab
server is installed, two invented users
(alice and bob) get accounts, a calendar
for a fictional camera club is created,
an event and a contact are stored and
read back over the wire, a second user
is shown the door, and then one calendar
is deliberately shared — read-only —
and the boundary is tested from both
sides. Every step was run against the
real program, and the transcripts are
in `radicale-lab/transcripts/`.

## What this guide covers

- [**What Radicale is**](what-radicale-is.md) —
  the problem it solves, the protocols in
  one page, and what it is not
- [**The moving parts**](the-moving-parts.md) —
  collections, the storage folder, users
  and rights: the four ideas everything
  else hangs on
- [**Installing Radicale**](installing-radicale.md) —
  a Python virtual environment, the
  program, and a configuration file with
  nothing in it you cannot explain
- [**Your first calendar**](your-first-calendar.md) —
  make a calendar, store an event, read
  it back, and see the file it became
- [**Users and authentication**](users-and-authentication.md) —
  the password file, bcrypt hashing, and
  the 401/403 boundary between strangers
  and owners
- [**Sharing and rights**](sharing-and-rights.md) —
  the rights file, a read-only share,
  and proof from both sides of the fence
- [**Connecting real clients**](connecting-clients.md) —
  the addresses and settings a calendar
  app needs, and how to test them with
  curl before blaming the app
- [**Backing up**](backing-up.md) — why
  the backup is one folder, and a restore
  you can rehearse in a minute
- [**Recipes**](recipes.md) — a household
  server, a club calendar shared with
  members, a contacts book for the family
- [**The neighbours**](the-neighbours.md) —
  Nextcloud and the big clouds: what each
  does that Radicale doesn't, and vice
  versa
- [**Limits**](limits.md) — no push
  notifications of its own, no groupware
  suite, small-scale by design
- [**Troubleshooting**](troubleshooting.md) —
  the failures the lab actually met,
  and the ones the log explains
- [**Glossary**](glossary.md) — CalDAV,
  CardDAV, PROPFIND and friends, defined
  in plain words

## How to use this guide

Read chapter 1 for the model and
chapter 2 before installing anything —
knowing which part holds what turns
later confusion into a named problem.
Chapters 3 and 4 get you a running
server with a calendar in it, in one
sitting. Chapter 6 is the one to read
slowly: sharing is where a private
calendar stops being private, and the
guide tests the fence before asking
you to trust it. The lab's transcripts
live in `radicale-lab/transcripts/`;
its configuration, rights file and
sample event and contact are in
`radicale-lab/` itself.

---

## Part of the series

This guide is one of the [For Mere Mortals](https://paulsullivan33-dev.github.io/mere-mortals/) guides: plain-language guides to the tools programmers and makers use.

Related guides:

- [Nextcloud for Mere Mortals](https://paulsullivan33-dev.github.io/nextcloud-for-mere-mortals/)
- [Backups for Mere Mortals](https://paulsullivan33-dev.github.io/backups-for-mere-mortals/)
- [Syncthing for Mere Mortals](https://paulsullivan33-dev.github.io/syncthing-for-mere-mortals/)

## License

The writing in this guide is licensed under [CC BY 4.0](https://github.com/paulsullivan33-dev/radicale-for-mere-mortals/blob/main/LICENSE). Code examples and lab scripts are licensed under the [MIT License](https://github.com/paulsullivan33-dev/radicale-for-mere-mortals/blob/main/LICENSE-CODE).
