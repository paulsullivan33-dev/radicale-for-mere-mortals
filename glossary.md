# Glossary

**Address book** — A collection that
holds contacts instead of events;
served over CardDAV.

**CalDAV** — The standard protocol
for calendars over HTTP: how clients
ask a server for events in a range,
store new ones, and sync changes.

**CardDAV** — The same idea for
contacts: address books over HTTP,
one vCard per contact.

**Collection** — Radicale's unit of
organisation: a calendar, an address
book, or a folder grouping them. Has
an address like `/alice/club/`.

**DAV** — WebDAV, the older standard
that extends HTTP with `PROPFIND`,
`MKCOL` and friends for managing
things on a server. CalDAV and
CardDAV are built on it.

**Discovery** — How a client, given
just a server address and a login,
finds where that user's calendars
live: it asks the principal.

**Etag** — A version stamp on an
item. Clients present it when writing
so the server can refuse to overwrite
a version they never saw.

**htpasswd** — The classic password
file format Radicale uses: one line
per user, name plus password hash.

**iCalendar (.ics)** — The text
format of an event: `BEGIN:VEVENT`,
times, a summary, `END:VEVENT`. One
file per event in Radicale's storage.

**Principal** — A user's home on the
server: `/alice/`. Everything the
user owns lives under it.

**PROPFIND** — The DAV request that
lists and describes collections; the
"what is here?" verb.

**REPORT** — The DAV request that
asks a real question, such as a
CalDAV calendar-query for events in a
time range, or a CardDAV
addressbook-query for contacts.

**Rights** — Radicale's rules for who
may do what to which collection:
either the built-in `owner_only`, or
a file of rules you write.

**UID** — The unique ID every event
and contact carries, so clients and
servers can tell two items apart even
with the same title.

**vCard (.vcf)** — The text format of
a contact: name, organisation, email,
phone. One file per contact in
Radicale's storage.

**Virtual environment (venv)** — A
private folder of Python packages for
one program, so its dependencies
never tangle with the system's.
