# Your First Calendar

By the end of this chapter alice has a
calendar, the calendar has an event,
and you have read the event back three
ways — as the client sees it, as a
CalDAV query returns it, and as the
file it is on disk. Everything here is
`curl` speaking the protocol directly,
so you can see exactly what a calendar
app will later do on your behalf.

## Give alice a password first

Chapter 5 goes deep on users; for now,
one command creates the password file
with the lab's two throwaway accounts
(alice: `lab-alice-123`, bob:
`lab-bob-123` — lab passwords, stated
here because the lab is disposable):

```bash
./venv/bin/python - <<'EOF'
from passlib.hash import bcrypt
with open('htpasswd', 'w') as f:
    f.write('alice:' + bcrypt.hash('lab-alice-123') + '\n')
    f.write('bob:' + bcrypt.hash('lab-bob-123') + '\n')
EOF
```

Start (or restart) the server with the
config from chapter 3, and alice can
now introduce herself (transcript t02):

```bash
curl -s -u alice:lab-alice-123 -X PROPFIND -H "Depth: 0" \
  http://127.0.0.1:5232/alice/
```

The answer is a block of XML — the
*principal*. Buried in it are the two
addresses that matter: her
**calendar home** and **address book
home**, both `/alice/`. That response
is how real clients discover where a
user's collections live.

## Make the calendar

A calendar is a collection with a
type. `MKCALENDAR` creates one and
names it in the same breath:

```bash
curl -s -o /dev/null -w '%{http_code}\n' \
  -u alice:lab-alice-123 -X MKCALENDAR \
  -H "Content-Type: application/xml" \
  --data '<?xml version="1.0" encoding="utf-8" ?>
<C:mkcalendar xmlns:D="DAV:" xmlns:C="urn:ietf:params:xml:ns:caldav">
<D:set><D:prop><D:displayname>Riverside Camera Club</D:displayname>
</D:prop></D:set></C:mkcalendar>' \
  http://127.0.0.1:5232/alice/club/
# 201
```

**201** means *created*. And in the
storage folder, the collection already
exists as a directory with one file in
it (transcript t02):

```text
storage/collection-root/alice/club/.Radicale.props
```

The `.Radicale.props` file holds what
you just set — the display name
"Riverside Camera Club" and the fact
that this collection is a calendar.

## Store an event

An event is a file of iCalendar text.
The lab's (`radicale-lab/photo-walk.ics`)
describes the club's photo walk — a
title, a start and end, a place, and a
unique ID (`UID`) every event must
carry so servers and clients can tell
events apart:

```bash
curl -s -o /dev/null -w '%{http_code}\n' \
  -u alice:lab-alice-123 -X PUT \
  -H "Content-Type: text/calendar" \
  --data-binary @photo-walk.ics \
  http://127.0.0.1:5232/alice/club/photo-walk.ics
# 201
```

## Read it back, three ways

**As an item** — a plain `GET` of its
address returns the event (transcript
t03). The server may tidy the line
order; the content is the event you
sent, `SUMMARY` and all.

**As a CalDAV question** — the
`REPORT` verb with a *calendar-query*
asks the way a real client asks when
it opens November:

```bash
curl -s -u alice:lab-alice-123 -X REPORT \
  -H "Content-Type: application/xml" -H "Depth: 1" \
  --data '<C:calendar-query ...>
    <C:filter><C:comp-filter name="VCALENDAR">
    <C:comp-filter name="VEVENT">
    <C:time-range start="20261101T000000Z"
                  end="20261201T000000Z"/>
    </C:comp-filter></C:comp-filter></C:filter>
  </C:calendar-query>' \
  http://127.0.0.1:5232/alice/club/
```

The answer names `/alice/club/photo-walk.ics`
and includes the event — because
November 7 falls inside the range.
Ask about December instead and the
same query returns an empty list: the
server did the filtering, not you. The
full query text is in transcript t03.

**As a file** — the storage folder now
holds `alice/club/photo-walk.ics`, and
`cat` shows the event (transcript
t07). Client view, protocol view and
disk view agree, because underneath,
they are the same thing.

**Check:** you are done when the GET
shows your event's `SUMMARY`, the
November query finds it, and the file
is sitting in the storage tree. The
next chapter makes the second user —
and finds out what bob can see.
