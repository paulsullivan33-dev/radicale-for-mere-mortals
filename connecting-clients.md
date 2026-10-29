# Connecting Real Clients

By the end of this chapter you know
what to type into a calendar or
contacts app to make it a Radicale
client, and how to test every piece of
it with curl first — because the lab
drove the whole protocol with curl,
every setting below corresponds to a
request you have already seen succeed.

An honesty note before the settings:
the lab has no graphical desktop, so
no app was clicked through here. The
apps named are characterised from
their own documentation
(documented-only, as the criteria
page records); what *is* verified is
the server side of every conversation
they will have.

## The three facts every client needs

1. **The server address.** In the lab,
   `http://127.0.0.1:5232/`. Clients do
   *discovery* from an address like
   this: they ask the server who you
   are and where your homes are — the
   PROPFIND of chapter 4, which answered
   with alice's calendar-home and
   addressbook-home. Point an app at
   the base address and let it discover;
   that is the path the protocol is
   designed around.
2. **A username and password** —
   exactly the `htpasswd` pair from
   chapter 5. The app stores them and
   presents them with every request.
3. **The collection address**, if the
   app asks for it directly:
   `http://127.0.0.1:5232/alice/club/`
   for the calendar,
   `http://127.0.0.1:5232/alice/contacts/`
   for the address book. A shared
   calendar has the *owner's* name in
   its address — bob reads the club
   calendar at `/alice/club/`, signed
   in as bob.

## Test before you blame the app

Every client failure is one of the
answers from chapter 5 wearing a
dialogue box. Reproduce it with curl:

```bash
# Is the server up and the account real?
curl -s -o /dev/null -w '%{http_code}\n' \
  -u alice:lab-alice-123 -X PROPFIND -H "Depth: 0" \
  http://127.0.0.1:5232/alice/
# 207 = server fine, password fine.
# 401 = wrong password (or the account
#       is not in the file).
# connection refused = server not
#       running, or wrong host/port.
```

```bash
# Can this user see this collection?
curl -s -o /dev/null -w '%{http_code}\n' \
  -u bob:lab-bob-123 -X PROPFIND -H "Depth: 1" \
  http://127.0.0.1:5232/alice/club/
# 207 = the share is live.
# 403 = no rule grants it (chapter 6).
```

If curl gets the right answers and the
app does not, the problem is in the
app's settings — usually a discovery
address it mangled, or a calendar it
cached from before a rights change
(restarting the app, or removing and
re-adding the account, clears that).

## The usual clients, briefly

Documented-only characterisations:
**Thunderbird** speaks CalDAV and
CardDAV with its calendar enabled —
add a network calendar, give the base
address and the account. **Apple's
Calendar and Contacts** accept a
CalDAV/CardDAV account in system
settings. On **Android**, the common
route is the **DAVx5** app, which
syncs CalDAV/CardDAV accounts into
the phone's own calendar and contacts.
All three do the discovery dance from
the base address; all three hold the
account password on the device.

## Beyond one machine

The lab's server listens on
`127.0.0.1` — the same machine only.
A real household server changes two
things, both documented-only here and
flagged again in the limits chapter:
the `hosts` line must name an address
other machines can reach, and plain
HTTP must not cross a network with
passwords on it — the standard shape
is Radicale behind a web server
(Apache or nginx) that adds HTTPS, or
access only through a VPN or SSH
tunnel. The Radicale documentation's
own deployment pages describe the
reverse-proxy setup; this guide's
position is simple: *do not widen the
audience before you have added the
encryption*, and chapter 5's fence is
only as meaningful as the wire is
private.

**Check:** a client is connected when
an event created in the app appears
as a file in the storage folder —
the disk view from chapter 4 is the
ground truth every sync claim answers
to.
