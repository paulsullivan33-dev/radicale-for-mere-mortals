# What Radicale Is

You have appointments and people — a
dentist visit, a club meeting, the
phone number of the person who frames
your prints. Your phone, your laptop
and your desktop each want a copy, and
they want the copies to agree. The
usual arrangement is that a large
company's cloud holds the master copy
and every device syncs with it. It
works. It also means your calendar —
where you are, when, and with whom —
sits on computers you do not control,
under terms you did not negotiate.

**Radicale is that master copy, on
your machine.** It is a small server
program, written in Python, that speaks
two standard protocols:

- **CalDAV**, the calendar protocol —
  how apps ask a server for events in a
  date range, store new ones, and learn
  what changed since last time.
- **CardDAV**, the contacts protocol —
  the same conversation, about address
  book entries instead of events.

Because the protocols are standards,
the apps you already own can use it:
the calendar on a phone, Thunderbird,
Apple Calendar, and many more are all
CalDAV/CardDAV clients. Radicale does
not ask you to learn a new app. It
asks to be the quiet server your
existing apps talk to.

## The storage is the surprise

Most servers keep your data in a
database you need special tools to
read. Radicale keeps each event as one
small text file — the same iCalendar
text a client sent — in an ordinary
folder tree. In this guide's lab, the
photo-walk event the club calendar
gained in chapter 4 is, on disk, a
file you can print with `cat`
(transcript t07):

```text
BEGIN:VCALENDAR
VERSION:2.0
...
SUMMARY:Riverside Camera Club photo walk
...
END:VCALENDAR
```

That one design choice — *the files are
the database* — shapes everything good
about the tool: backups are a folder
copy (chapter 8), a broken server never
strands your data, and you can inspect
exactly what the server thinks you
told it.

## What it is not

Radicale is not a groupware suite. It
has no mail server, no webmail, no
document editing, no chat — the
neighbour chapter (chapter 10) covers
Nextcloud, which is all of those, and
is a hundred times the program. It is
not a calendar *application*: it ships
a small built-in web page for managing
collections, but day-to-day you read
and write events in your usual calendar
app, not in Radicale. And it is not
built for an enterprise — no clustering,
no thousands of users; it is aimed at
a person, a household, a club.

## Who it is for

The person who wants their calendar
off a stranger's cloud without giving
up syncing between phone and laptop.
The household that wants one family
calendar and one shared address book.
The small club whose events calendar
should live on the club's own little
server. The common thread: standard
protocols, a folder of plain files,
and a server small enough to understand
whole — this guide's configuration
file is fifteen lines, and chapter 3
explains every one.

## A first taste

Once running (chapters 3–4), the whole
system answers to ordinary web
requests. This is a real client asking
the lab server "what is in alice's
club calendar in November?" — and the
server answering with the event
(transcript t03):

```bash
curl -u alice:lab-alice-123 -X REPORT \
  http://127.0.0.1:5232/alice/club/
# ... returns the photo-walk event,
# because it falls inside the range
```

Nothing about that needs an account
with anyone. That is the point of the
tool, stated as early as this guide
can state it.
